# Кликабельные ссылки на файлы проекта в Claude Code + Kitty + VS Code

## Содержание

- [Описание](#описание)
- [Требования](#требования)
- [Настройка Kitty](#настройка-kitty)
  - [url_prefixes](#url_prefixes)
  - [open-actions.conf](#open-actionsconf)
  - [Скрипт open-vscode.sh](#скрипт-open-vscodesh)
- [Настройка CLAUDE.md](#настройка-claudemd)
- [Как это работает](#как-это-работает)
- [Проверка](#проверка)

## Описание

Настройка кликабельных ссылок в терминале Kitty, которые открывают файлы проекта
в VS Code на нужной строке. Claude Code выводит ссылки в формате markdown,
Kitty распознает протокол `vscode://`, скрипт определяет нужное окно VS Code
по корню git-репозитория.

## Требования

- Терминал Kitty (0.46+)
- VS Code с зарегистрированным обработчиком протокола `vscode://`
- Git
- Claude Code

Проверить, что обработчик `vscode://` зарегистрирован:

```bash
xdg-mime query default x-scheme-handler/vscode
# Ожидаемый результат: code-url-handler.desktop
```

## Настройка Kitty

### url_prefixes

В `~/.config/kitty/kitty.conf` добавить `vscode` в список распознаваемых протоколов:

```
url_prefixes file ftp ftps gemini git gopher http https irc ircs kitty mailto news sftp ssh vscode
```

Также рекомендуется настроить Ctrl+Click для открытия ссылок:

```
mouse_map left click ungrabbed mouse_handle_click selection prompt
mouse_map ctrl+left click ungrabbed mouse_handle_click link
```

### open-actions.conf

Создать файл `~/.config/kitty/open-actions.conf`:

```
# Перехват vscode:// ссылок для открытия в правильном окне VS Code
protocol vscode
action launch --type=background ~/.config/kitty/open-vscode.sh ${URL}
```

### Скрипт open-vscode.sh

Создать файл `~/.config/kitty/open-vscode.sh` и сделать его исполняемым (`chmod +x`):

```bash
#!/bin/bash

# Открывает vscode:// ссылки в правильном окне VS Code
# Формат: vscode://file/<абсолютный путь>:<строка>

url="$1"

# Убираем префикс vscode://file и добавляем / в начало пути
path_with_line="/${url#vscode://file/}"

# Разделяем путь и номер строки
# Номер строки — после последнего двоеточия, но только если это число
if [[ "$path_with_line" =~ ^(/.+):([0-9]+)$ ]]; then
    filepath="${BASH_REMATCH[1]}"
    line="${BASH_REMATCH[2]}"
else
    filepath="/$path_with_line"
    line=""
fi

# Проверяем, что файл существует
if [ ! -f "$filepath" ]; then
    exit 1
fi

# Определяем корень проекта через git
project_root="$(git -C "$(dirname "$filepath")" rev-parse --show-toplevel 2>/dev/null)"

if [ -n "$line" ]; then
    goto_arg="--goto ${filepath}:${line}"
else
    goto_arg="$filepath"
fi

if [ -n "$project_root" ]; then
    code "$project_root" $goto_arg
else
    code -n $goto_arg
fi
```

Логика выбора окна VS Code:
- Если файл внутри git-репозитория -- открыть в окне этого проекта (`code <project_root> --goto <file>:<line>`)
- Если git-репозиторий не найден -- открыть в новом окне (`code -n --goto <file>:<line>`)

После внесения изменений перезагрузить конфигурацию Kitty: `Ctrl+Shift+F5`.

## Настройка CLAUDE.md

Добавить в глобальный `~/.claude/CLAUDE.md` правило для генерации ссылок:

```markdown
## Ссылки на файлы проекта

При любом упоминании файлов проекта -- в тексте, таблицах, списках -- всегда
выводить кликабельные ссылки в формате:
`[[проект/ветка] относительный/путь:строка](vscode://file/абсолютный/путь:строка)`

Если номер строки не применим (например, ссылка на файл целиком), строку можно опустить:
`[[проект/ветка] относительный/путь](vscode://file/абсолютный/путь)`

Метка проекта:
- Формат: `[имя-каталога-проекта/ветка]` -- например `[fin-core/feat/foundation]`
- Если ветка не определяется (не git-репозиторий) -- только имя каталога: `[client-api]`
- Для файлов, путь которых начинается с текущего рабочего каталога (cwd), метку
  НЕ показывать. Важно: cwd может отличаться от primary working directory,
  указанного при запуске сессии -- в процессе работы каталог мог быть изменён
  через cd. При сомнениях определять cwd через `pwd`
- Имя проекта -- имя каталога из `git rev-parse --show-toplevel`
- Ветка -- из `git branch --show-current`, определяется один раз на проект

Это правило действует всегда: при перечислении файлов в таблицах, при ответе
на вопрос "покажи файлы", при упоминании файла в обсуждении. Никогда не выводить
пути к файлам проекта без кликабельной ссылки.
```

## Как это работает

1. Claude Code выводит markdown-ссылку: `[[project/branch] src/file.ts:42](vscode://file/abs/path/src/file.ts:42)`
2. Kitty распознает `vscode://` как URL (благодаря `url_prefixes`) и подчеркивает ссылку
3. По Ctrl+Click Kitty передает URL в `open-actions.conf`
4. `open-actions.conf` вызывает скрипт `open-vscode.sh` с URL в качестве аргумента
5. Скрипт парсит URL, находит корень git-проекта, вызывает `code <project> --goto <file>:<line>`
6. VS Code открывает файл на нужной строке в окне соответствующего проекта

## Проверка

1. Открыть проект в VS Code: `code /path/to/project`
2. В Kitty запустить Claude Code в каталоге проекта
3. Попросить Claude показать файлы проекта
4. Ctrl+Click по ссылке -- файл должен открыться в уже открытом окне VS Code на нужной строке
