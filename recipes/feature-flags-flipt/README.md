# Feature Flags с Flipt v2 через Podman

## Содержание

- [Обзор](#обзор)
- [Архитектура](#архитектура)
  - [Формат файлов feature flags](#формат-файлов-feature-flags)
- [Настройка Flipt](#настройка-flipt)
  - [Конфигурационный файл flipt.yml](#конфигурационный-файл-fliptyml)
  - [Аутентификация для Git-репозитория](#аутентификация-для-git-репозитория)
  - [Аутентификация UI](#аутентификация-ui)
  - [Credentials](#credentials)
- [Запуск через Podman](#запуск-через-podman)
  - [Сервис в podman-compose.yaml](#сервис-в-podman-composeyaml)
  - [Тонкости настройки podman-compose](#тонкости-настройки-podman-compose)
  - [Запуск и проверка](#запуск-и-проверка)
- [Использование в NestJS](#использование-в-nestjs)
  - [Подход 1: OpenFeature (рекомендуемый)](#подход-1-openfeature-рекомендуемый)
  - [Подход 2: Flipt SDK напрямую](#подход-2-flipt-sdk-напрямую)
  - [Типы флагов и примеры вычисления](#типы-флагов-и-примеры-вычисления)
- [Работа с UI](#работа-с-ui)
- [Управление окружениями](#управление-окружениями)
- [Ссылки](#ссылки)

## Обзор

Flipt v2 -- система управления feature flags с открытым исходным кодом, использующая Git как источник истины. Флаги описываются в YAML-файлах, хранятся в Git-репозитории и автоматически синхронизируются с Flipt-сервером.

Ключевые особенности:

- GitOps-подход: флаги хранятся в Git, изменения проходят через обычный PR-процесс
- Несколько окружений (stage, production) в одной Git-ветке через разные подкаталоги
- REST API и gRPC для вычисления флагов из приложений
- Web UI для просмотра и (при наличии записи) управления флагами
- Поддержка boolean и variant флагов с сегментацией и rollout

## Архитектура

```
Git-репозиторий (приватный, GitHub)
├── src/                          # Код приложения (фронт, бэкенд)
├── ...
└── feature-flags/                # Все feature flags
    ├── stage/                    # Окружение stage
    │   └── default/              # Namespace "default"
    │       └── features.yml
    └── production/               # Окружение production
        └── default/              # Namespace "default"
            └── features.yml
```

Flipt v2 поддерживает несколько схем разделения окружений: по веткам (branch-based), по подкаталогам (directory-based) и по отдельным репозиториям. В данном рецепте используется **directory-based** подход -- все окружения находятся в одной Git-ветке, но в разных подкаталогах. Это удобнее, чем branch-based подход, потому что:

- Не нужно синхронизировать структуру между ветками
- Проще ревьюить изменения -- всё в одном PR
- Нет риска потерять флаги при мерже веток

Внутри каждого каталога окружения (`stage/`, `production/`) Flipt ожидает подкаталог с именем namespace (в данном случае `default`). Flipt сканирует каталог рекурсивно и загружает все `.yml`/`.yaml` файлы, поэтому флаги можно разбить на несколько файлов:

```
feature-flags/stage/default/
├── billing.yml
├── search.yml
└── ui.yml
```

### Формат файлов feature flags

```yaml
namespace: default

flags:
  # Boolean flag -- простое включение/выключение
  - key: new-checkout-flow
    name: New Checkout Flow
    type: BOOLEAN_FLAG_TYPE
    enabled: false
    rollouts:
      - threshold:
          percentage: 0
          value: true

  # Variant flag -- выбор одного из вариантов
  - key: ui-theme
    name: UI Theme
    type: VARIANT_FLAG_TYPE
    enabled: true
    variants:
      - key: light
        name: Light Theme
      - key: dark
        name: Dark Theme
    rules:
      - segment:
          keys: ["all-users"]
        distributions:
          - variant: light
            rollout: 100

  # Variant flag с attachment -- произвольные данные в варианте
  - key: rate-limit
    name: API Rate Limit
    type: VARIANT_FLAG_TYPE
    enabled: true
    variants:
      - key: standard
        name: Standard Limit
        attachment:
          requests_per_minute: 60
          burst: 10
      - key: elevated
        name: Elevated Limit
        attachment:
          requests_per_minute: 120
          burst: 30
    rules:
      - segment:
          keys: ["all-users"]
        distributions:
          - variant: standard
            rollout: 100

segments:
  - key: all-users
    name: All Users
    match_type: ALL_MATCH_TYPE

  - key: internal-users
    name: Internal Users
    match_type: ALL_MATCH_TYPE
    constraints:
      - type: STRING_COMPARISON_TYPE
        property: organization
        operator: eq
        value: internal
```

## Настройка Flipt

### Конфигурационный файл flipt.yml

Основной конфигурационный файл для Flipt v2 с remote Git-репозиторием и двумя окружениями:

```yaml
version: "2.0"

storage:
  flags-repo:
    remote: "https://github.com/<owner>/<repo>.git"
    branch: "main"
    poll_interval: "30s"
    credentials: "github-pat"
    backend:
      type: memory

credentials:
  github-pat:
    type: access_token
    access_token: "${env:FLIPT_GIT_TOKEN}"

environments:
  stage:
    name: "Stage"
    storage: "flags-repo"
    directory: "feature-flags/stage"
  production:
    name: "Production"
    storage: "flags-repo"
    directory: "feature-flags/production"
    default: true

authentication:
  required: true
  methods:
    token:
      enabled: true
      storage:
        type: static
        tokens:
          bootstrap:
            credential: "${env:FLIPT_AUTH_TOKEN}"
```

Ключевые моменты конфигурации:

- **`storage`** -- определяет backend хранения. Один storage может обслуживать несколько окружений. `type: memory` означает, что Flipt клонирует репозиторий в память (не на диск).
- **`credentials`** -- именованные учётные данные. Ссылка из storage по имени. Поддерживает `access_token`, `basic`, `ssh`, `github_app`.
- **`environments`** -- окружения с указанием storage и directory. Каждое окружение читает флаги из своего подкаталога в репозитории. Только одно окружение может быть `default: true`.
- **`poll_interval`** -- как часто Flipt проверяет обновления в Git. Значение `30s` -- разумный баланс между актуальностью и нагрузкой.
- **`${env:VAR}`** -- подстановка переменных окружения. Используется для секретов.

### Аутентификация для Git-репозитория

Для приватного GitHub-репозитория через PAT (Personal Access Token):

1. Создать PAT на GitHub: Settings -> Developer settings -> Personal access tokens -> Fine-grained tokens
2. Минимальные разрешения: **Contents: Read** для нужного репозитория
3. Если нужно управлять флагами из Flipt UI (запись в Git): **Contents: Read and write**

Передать токен через переменную окружения `FLIPT_GIT_TOKEN`.

Альтернативные способы аутентификации:

```yaml
# SSH-ключ
credentials:
  git-ssh:
    type: ssh
    ssh:
      user: git
      private_key_path: "/etc/flipt/ssh/id_ed25519"
      # или private_key_bytes: "${env:FLIPT_SSH_KEY}"

# Basic auth (username + PAT как пароль)
credentials:
  git-basic:
    type: basic
    basic:
      username: "${env:FLIPT_GIT_USER}"
      password: "${env:FLIPT_GIT_TOKEN}"

# GitHub App
credentials:
  github-app:
    type: github_app
    github_app:
      client_id: "${env:GITHUB_APP_CLIENT_ID}"
      installation_id: 12345678
      private_key_path: "/etc/flipt/github-app-key.pem"
```

### Аутентификация UI

Flipt поддерживает несколько методов аутентификации для защиты UI и API.

#### Статический токен (простейший вариант)

Подходит для dev/stage окружений:

```yaml
authentication:
  required: true
  methods:
    token:
      enabled: true
      storage:
        type: static
        tokens:
          bootstrap:
            credential: "${env:FLIPT_AUTH_TOKEN}"
```

При входе в UI нужно ввести значение `FLIPT_AUTH_TOKEN`.

#### GitHub OAuth (для команды)

Подходит для production -- вход через GitHub-аккаунт с ограничением по организации:

```yaml
authentication:
  required: true
  session:
    domain: "flipt.example.com"
    secure: true
    csrf:
      key: "${env:FLIPT_CSRF_KEY}"
  methods:
    github:
      enabled: true
      client_id: "${env:FLIPT_GITHUB_CLIENT_ID}"
      client_secret: "${env:FLIPT_GITHUB_CLIENT_SECRET}"
      redirect_address: "https://flipt.example.com/auth/v1/method/github/callback"
      scopes:
        - "user:email"
        - "read:org"
      allowed_organizations:
        - "your-org"
```

Для настройки GitHub OAuth:

1. GitHub -> Settings -> Developer settings -> OAuth Apps -> New OAuth App
2. Homepage URL: `https://flipt.example.com`
3. Authorization callback URL: `https://flipt.example.com/auth/v1/method/github/callback`

#### OIDC (для интеграции с корпоративным IdP)

```yaml
authentication:
  required: true
  session:
    domain: "flipt.example.com"
    secure: true
  methods:
    oidc:
      enabled: true
      providers:
        keycloak:
          issuer_url: "https://keycloak.example.com/realms/your-realm"
          client_id: "${env:FLIPT_OIDC_CLIENT_ID}"
          client_secret: "${env:FLIPT_OIDC_CLIENT_SECRET}"
          redirect_address: "https://flipt.example.com/auth/v1/method/oidc/keycloak/callback"
          scopes:
            - "openid"
            - "email"
            - "profile"
```

#### Исключение evaluation API из аутентификации

Чтобы приложения могли вычислять флаги без токена (если Flipt внутри приватной сети):

```yaml
authentication:
  required: true
  exclude:
    evaluation: true
  methods:
    token:
      enabled: true
      # ...
```

### Credentials

Секция `credentials` -- это именованные учётные данные, на которые ссылаются storage и SCM. Все секреты подставляются через `${env:VAR}`.

## Запуск через Podman

### Сервис в podman-compose.yaml

```yaml
services:
  flipt:
    image: docker.flipt.io/flipt/flipt:v2
    container_name: flipt
    command: ["./flipt", "server", "--config", "/etc/flipt/config/default.yml"]
    ports:
      - "8080:8080"
    volumes:
      - ./flipt/flipt.yml:/etc/flipt/config/default.yml:ro
    environment:
      - FLIPT_GIT_TOKEN=${FLIPT_GIT_TOKEN}
      - FLIPT_AUTH_TOKEN=${FLIPT_AUTH_TOKEN}
    restart: unless-stopped
```

### Тонкости настройки podman-compose

#### 1. Команда запуска

Flipt v2 требует явного указания пути к конфигурации:

```yaml
command: ["./flipt", "server", "--config", "/etc/flipt/config/default.yml"]
```

Без `--config` Flipt будет искать конфигурацию по умолчанию и не найдёт кастомный файл.

#### 2. Монтирование конфигурации

```yaml
volumes:
  - ./flipt/flipt.yml:/etc/flipt/config/default.yml:ro
```

- Монтировать **`:ro`** (read-only) -- конфигурация не должна изменяться контейнером
- Путь `/etc/flipt/config/default.yml` -- стандартный для Flipt

#### 3. Передача секретов

Секреты передаются через переменные окружения:

```yaml
environment:
  - FLIPT_GIT_TOKEN=${FLIPT_GIT_TOKEN}
  - FLIPT_AUTH_TOKEN=${FLIPT_AUTH_TOKEN}
```

В конфигурации `flipt.yml` они подставляются через синтаксис `${env:FLIPT_GIT_TOKEN}`.

Для podman-compose секреты можно хранить в `.env` файле рядом с `podman-compose.yaml`:

```env
FLIPT_GIT_TOKEN=ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
FLIPT_AUTH_TOKEN=my-secret-flipt-token
```

**Не коммитить `.env` в Git!** Добавить в `.gitignore`.

#### 4. Нет необходимости в volumes для данных

При использовании `backend.type: memory` и remote Git-репозитория Flipt хранит все данные в оперативной памяти. Нет необходимости в persistent volume для данных флагов -- при перезапуске контейнер заново клонирует репозиторий.

#### 5. Для локальной разработки с local backend

Если для локальной разработки используется локальный Git-репозиторий вместо remote:

```yaml
services:
  flipt:
    image: docker.flipt.io/flipt/flipt:v2
    container_name: flipt
    command: ["./flipt", "server", "--config", "/etc/flipt/config/default.yml"]
    ports:
      - "8080:8080"
    volumes:
      - ./flipt/flipt.yml:/etc/flipt/config/default.yml:ro
      - ./flipt-flags-production:/var/data/flipt-flags-production:rw
      - ./flipt-flags-stage:/var/data/flipt-flags-stage:rw
```

При этом `flipt.yml` для локальной разработки:

```yaml
version: "2.0"

storage:
  stage-flags:
    backend:
      type: local
      path: "/var/data/flipt-flags-stage"
  production-flags:
    backend:
      type: local
      path: "/var/data/flipt-flags-production"

environments:
  stage:
    name: "Stage"
    storage: "stage-flags"
  production:
    name: "Production"
    storage: "production-flags"
    default: true
```

Тонкости local backend:

- Каталоги монтируются **`:rw`** -- Flipt нужен доступ на запись к `.git` (для операций с индексом)
- Каталоги `flipt-flags-stage/` и `flipt-flags-production/` должны быть отдельными Git-репозиториями (каждый со своим `.git/`)
- При local backend не указывается `branch` -- Flipt читает текущее состояние рабочей копии (HEAD)

#### 6. Сеть и доступ из других контейнеров

Если NestJS-приложение тоже запускается в podman-compose, обращение к Flipt по имени сервиса:

```yaml
services:
  app:
    # ...
    environment:
      - FLIPT_URL=http://flipt:8080
    depends_on:
      - flipt

  flipt:
    # ...
```

#### 7. Health check

```yaml
services:
  flipt:
    # ...
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "http://localhost:8080/health"]
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 10s
```

### Запуск и проверка

```bash
# Запуск
podman-compose up -d flipt

# Проверка логов
podman-compose logs flipt

# Проверка API
curl http://localhost:8080/health
curl http://localhost:8080/internal/v1/evaluation/snapshot/namespace/default

# Открыть UI
# http://localhost:8080
```

## Использование в NestJS

Flipt v2 сохраняет REST API v1 для evaluation -- эндпоинты `POST /evaluate/v1/boolean`, `POST /evaluate/v1/variant`, `POST /evaluate/v1/batch`. SDK для Node.js обратно и прямо совместимы с обеими версиями сервера, поэтому один и тот же клиентский код работает и с Flipt v1, и с v2.

Два подхода к интеграции:

1. **OpenFeature** (рекомендуемый) -- vendor-agnostic стандарт с NestJS-декораторами. Позволяет менять провайдера (Flipt, LaunchDarkly, и т.д.) без изменения кода приложения.
2. **Flipt SDK напрямую** -- проще в настройке, но привязывает код к Flipt.

### Подход 1: OpenFeature (рекомендуемый)

[OpenFeature](https://openfeature.dev/) -- открытый стандарт для feature flag evaluation. Flipt реализует протокол OFREP (OpenFeature Remote Evaluation Protocol) и имеет официальный OpenFeature-провайдер для Node.js.

#### Установка

```bash
npm install @openfeature/nestjs-sdk @openfeature/server-sdk @openfeature/flipt-provider
```

#### Подключение модуля

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { OpenFeatureModule } from '@openfeature/nestjs-sdk';
import { FliptProvider } from '@openfeature/flipt-provider';

@Module({
  imports: [
    OpenFeatureModule.forRoot({
      defaultProvider: new FliptProvider('default', {
        url: process.env.FLIPT_URL || 'http://localhost:8080',
        authToken: process.env.FLIPT_AUTH_TOKEN,
      }),
    }),
  ],
})
export class AppModule {}
```

#### Декораторы в контроллерах

```typescript
// checkout.controller.ts
import { Controller, Get, Req } from '@nestjs/common';
import {
  BooleanFeatureFlag,
  StringFeatureFlag,
  OpenFeatureClient,
} from '@openfeature/nestjs-sdk';
import { Client } from '@openfeature/server-sdk';

@Controller('checkout')
export class CheckoutController {
  // Инъекция OpenFeature-клиента через декоратор
  constructor(
    @OpenFeatureClient() private readonly featureClient: Client,
  ) {}

  // Декоратор @BooleanFeatureFlag -- вычисляет boolean-флаг
  // и передаёт результат в параметр метода
  @Get()
  async getCheckout(
    @BooleanFeatureFlag({
      flagKey: 'new-checkout-flow',
      defaultValue: false,
    })
    useNewFlow: boolean,
  ) {
    if (useNewFlow) {
      return { flow: 'new' };
    }
    return { flow: 'legacy' };
  }

  // Декоратор @StringFeatureFlag -- вычисляет variant-флаг
  @Get('theme')
  async getTheme(
    @StringFeatureFlag({
      flagKey: 'ui-theme',
      defaultValue: 'light',
    })
    theme: string,
  ) {
    return { theme };
  }

  // Программный доступ через инжектированный клиент
  @Get('config')
  async getConfig(@Req() req: any) {
    const userId = req.user?.id || 'anonymous';

    const enabled = await this.featureClient.getBooleanValue(
      'new-checkout-flow',
      false,
      { targetingKey: userId },
    );

    const theme = await this.featureClient.getStringValue(
      'ui-theme',
      'light',
      { targetingKey: userId, organization: req.user?.org },
    );

    return { enabled, theme };
  }
}
```

#### Использование в сервисах

```typescript
// search.service.ts
import { Injectable } from '@nestjs/common';
import { OpenFeatureClient } from '@openfeature/nestjs-sdk';
import { Client } from '@openfeature/server-sdk';

interface SearchConfig {
  algorithm: string;
  max_results: number;
  fuzzy: boolean;
  boost_fields: string[];
}

@Injectable()
export class SearchService {
  constructor(
    @OpenFeatureClient() private readonly featureClient: Client,
  ) {}

  async search(userId: string, query: string) {
    // getObjectValue для получения attachment как объекта
    const config = await this.featureClient.getObjectValue<SearchConfig>(
      'search-config',
      { algorithm: 'bm25', max_results: 20, fuzzy: false, boost_fields: [] },
      { targetingKey: userId },
    );

    return this.executeSearch(query, config);
  }

  async getRateLimit(userId: string, organization?: string) {
    const limits = await this.featureClient.getObjectValue<{
      requests_per_minute: number;
      burst: number;
    }>(
      'rate-limit',
      { requests_per_minute: 60, burst: 10 },
      { targetingKey: userId, organization: organization || '' },
    );

    return limits;
  }
}
```

#### Контекст evaluation

OpenFeature передаёт контекст через `EvaluationContext`:

- `targetingKey` -- идентификатор сущности (user ID). Используется для percentage rollout
- Остальные поля -- произвольные строковые пары для сегментации (constraints в Flipt)

```typescript
const ctx = {
  targetingKey: userId,
  organization: 'internal',    // для сегмента internal-users
  plan: 'enterprise',          // для кастомных сегментов
};

await client.getBooleanValue('my-flag', false, ctx);
```

### Подход 2: Flipt SDK напрямую

Прямое использование Flipt SDK без абстракции OpenFeature. Проще, но привязывает к Flipt.

#### Установка

```bash
npm install @flipt-io/flipt
```

#### Flipt-модуль

```typescript
// flipt.module.ts
import { Module, Global } from '@nestjs/common';
import { FliptService } from './flipt.service';

@Global()
@Module({
  providers: [FliptService],
  exports: [FliptService],
})
export class FliptModule {}
```

```typescript
// flipt.service.ts
import { Injectable, OnModuleInit, Logger } from '@nestjs/common';
import { FliptClient } from '@flipt-io/flipt';

@Injectable()
export class FliptService implements OnModuleInit {
  private readonly logger = new Logger(FliptService.name);
  private client: FliptClient;

  private readonly fliptUrl = process.env.FLIPT_URL || 'http://localhost:8080';
  private readonly fliptToken = process.env.FLIPT_AUTH_TOKEN;
  private readonly namespace = 'default';

  onModuleInit() {
    this.client = new FliptClient({
      url: this.fliptUrl,
      authToken: this.fliptToken,
    });
    this.logger.log(`Flipt client initialized: ${this.fliptUrl}`);
  }

  async isEnabled(flagKey: string, entityId: string, context?: Record<string, string>): Promise<boolean> {
    const response = await this.client.evaluation.boolean({
      namespaceKey: this.namespace,
      flagKey,
      entityId,
      context: context || {},
    });
    return response.enabled;
  }

  async getVariant(flagKey: string, entityId: string, context?: Record<string, string>): Promise<{ variantKey: string; attachment: string }> {
    const response = await this.client.evaluation.variant({
      namespaceKey: this.namespace,
      flagKey,
      entityId,
      context: context || {},
    });
    return {
      variantKey: response.variantKey,
      attachment: response.variantAttachment,
    };
  }

  async getVariantConfig<T = unknown>(flagKey: string, entityId: string, context?: Record<string, string>): Promise<T> {
    const { attachment } = await this.getVariant(flagKey, entityId, context);
    return attachment ? JSON.parse(attachment) as T : {} as T;
  }
}
```

#### Использование

```typescript
@Injectable()
export class SearchService {
  constructor(private readonly flipt: FliptService) {}

  async search(userId: string, query: string) {
    const config = await this.flipt.getVariantConfig<SearchConfig>(
      'search-config', userId,
    );
    return this.executeSearch(query, config);
  }

  async getCheckoutFlow(userId: string) {
    const useNewFlow = await this.flipt.isEnabled('new-checkout-flow', userId);
    return useNewFlow ? this.newCheckoutFlow() : this.legacyCheckoutFlow();
  }
}
```

### Типы флагов и примеры вычисления

| Тип | Метод SDK | Возвращает | Когда использовать |
|-----|-----------|------------|--------------------|
| Boolean | `evaluation.boolean()` | `enabled: boolean` | Включение/выключение фичи, A/B-тест on/off |
| Variant | `evaluation.variant()` | `variantKey`, `variantAttachment` | Выбор из нескольких вариантов, конфигурация через attachment |

**Boolean-флаг** -- для простого on/off. Поддерживает percentage rollout (постепенное включение для N% пользователей).

**Variant-флаг** -- для выбора одного из нескольких вариантов. Каждый вариант может иметь `attachment` -- произвольные данные (JSON), которые передаются приложению. Attachment позволяет хранить конфигурацию прямо во флаге, не захардкоживая значения в коде.

## Работа с UI

Flipt UI доступен по адресу `http://localhost:8080` (или по настроенному URL).

Возможности UI:

- **Просмотр флагов** -- все флаги, их состояние, варианты, правила распределения
- **Переключение окружений** -- выбор environment (Stage / Production) в навигации
- **Управление флагами** -- при записываемом storage (local с :rw или remote Git с PAT на запись) можно создавать, редактировать и удалять флаги прямо из UI. Изменения коммитятся в Git
- **Просмотр сегментов** -- сегменты пользователей и их constraints
- **Evaluation console** -- тестирование вычисления флагов прямо в UI

При включённой аутентификации (рекомендуется для всех окружений, кроме локальной разработки):

- **Статический токен**: при первом входе UI запросит токен. Ввести значение `FLIPT_AUTH_TOKEN`
- **GitHub OAuth / OIDC**: кнопка "Sign in with GitHub" / SSO провайдер

## Управление окружениями

Рабочий процесс управления флагами:

1. Разработчик создаёт/изменяет YAML-файл в каталоге `feature-flags/stage/default/`
2. Создаёт PR, проходит код-ревью
3. После мержа Flipt автоматически подхватывает изменения (poll_interval)
4. После проверки на stage -- аналогичный PR для `feature-flags/production/default/`

Для добавления нового окружения:

1. Создать каталог в Git-репозитории: `feature-flags/<env-name>/default/`
2. Добавить `features.yml` с нужными флагами
3. Добавить environment в `flipt.yml`:

```yaml
environments:
  # ... existing environments ...
  new-env:
    name: "New Environment"
    storage: "flags-repo"
    directory: "feature-flags/new-env"
```

4. Перезапустить Flipt

## Ссылки

**Flipt:**

- Flipt v2 Docs: https://docs.flipt.io/
- Flipt Configuration: https://docs.flipt.io/v2/configuration/overview
- Flipt Environments: https://docs.flipt.io/v2/configuration/environments
- Flipt Authentication: https://docs.flipt.io/v2/configuration/authentication
- Flipt Git Sync: https://docs.flipt.io/v2/guides/operations/environments/git-sync
- Flipt REST SDKs: https://docs.flipt.io/v2/integration/server/rest
- Flipt Client SDKs: https://github.com/flipt-io/flipt-client-sdks
- Flipt Docker image: https://hub.docker.com/r/flipt/flipt

**OpenFeature:**

- OpenFeature: https://openfeature.dev/
- OpenFeature NestJS SDK: https://openfeature.dev/docs/reference/sdks/server/javascript/nestjs
- OpenFeature Flipt Provider (Node.js): https://github.com/open-feature/js-sdk-contrib/tree/main/libs/providers/flipt
- OFREP (OpenFeature Remote Evaluation Protocol): https://github.com/open-feature/protocol
