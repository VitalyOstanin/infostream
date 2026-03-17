# Поточная обработка данных MongoDB: ETL-паттерны для Node.js

## Содержание

- [Введение](#введение)
- [Приём 1: Асинхронные генераторы](#приём-1-асинхронные-генераторы)
  - [Принцип работы](#принцип-работы)
  - [Пример: драйвер mongodb](#пример-драйвер-mongodb)
  - [Пример: mongoose](#пример-mongoose)
  - [Обогащение данных в потоке](#обогащение-данных-в-потоке)
  - [Параллельная обработка потока](#параллельная-обработка-потока)
  - [ETL: чтение из БД и отдача CSV в HTTP response](#etl-чтение-из-бд-и-отдача-csv-в-http-response)
- [Приём 2: Node.js Transform Streams](#приём-2-nodejs-transform-streams)
  - [Принцип работы](#принцип-работы-1)
  - [Пример: драйвер mongodb](#пример-драйвер-mongodb-1)
  - [Пример: mongoose](#пример-mongoose-1)
  - [ETL: чтение из БД и отдача CSV в HTTP response](#etl-чтение-из-бд-и-отдача-csv-в-http-response-1)
- [Сравнение подходов](#сравнение-подходов)
- [Рекомендации](#рекомендации)

---

## Введение

При работе с большими объёмами данных в MongoDB критически важно не загружать весь результат запроса в память. Вместо этого данные обрабатываются поточно: документы читаются из курсора порциями и сразу передаются на обработку или в HTTP response.

В этом документе описаны два приёма поточной обработки:

1. **Асинхронные генераторы** (`async function*`) — гибкий и выразительный подход, позволяющий строить цепочки обработки через `for await...of`.
2. **Node.js Transform Streams** — классический подход через `stream.pipeline()`, хорошо интегрированный с HTTP response и файловыми потоками.

Примеры используют три бизнес-сущности: `User`, `Company`, `Payment`.

---

## Приём 1: Асинхронные генераторы

### Принцип работы

Асинхронный генератор — это функция, объявленная как `async function*`, которая может приостанавливать выполнение с помощью `yield`. Курсор MongoDB реализует протокол `AsyncIterable`, поэтому его можно итерировать через `for await...of`, а результат оборачивать в генератор для дополнительной трансформации.

Ключевые свойства:
- Ленивая обработка — следующий документ читается только когда потребитель готов
- Данные не накапливаются в памяти
- Простая композиция: генераторы можно вкладывать друг в друга
- Естественная обработка ошибок через try/catch

### Пример: драйвер mongodb

```typescript
import { MongoClient, Collection } from 'mongodb';

interface Payment {
  _id: string;
  userId: string;
  companyId: string;
  amount: number;
  currency: string;
  status: string;
  createdAt: Date;
}

class ReportService {
  private paymentsCollection: Collection<Payment>;

  constructor(private readonly mongoClient: MongoClient) {
    this.paymentsCollection = this.mongoClient
      .db('billing')
      .collection('payments');
  }

  /**
   * Базовый генератор: читает платежи из курсора агрегации
   */
  async *getPaymentsCursor(fromDate: Date, toDate: Date) {
    const pipeline = [
      {
        $match: {
          createdAt: { $gte: fromDate, $lt: toDate },
          status: 'completed',
        },
      },
      {
        $lookup: {
          from: 'companies',
          localField: 'companyId',
          foreignField: '_id',
          as: 'company',
        },
      },
      { $unwind: '$company' },
    ];

    const cursor = this.paymentsCollection.aggregate(pipeline);

    try {
      for await (const doc of cursor) {
        yield doc;
      }
    } finally {
      await cursor.close();
    }
  }
}
```

### Пример: mongoose

```typescript
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Payment } from './payment.schema';

@Injectable()
class ReportService {
  constructor(
    @InjectModel(Payment.name) private paymentModel: Model<Payment>,
  ) {}

  async *getPaymentsCursor(fromDate: Date, toDate: Date) {
    const cursor = this.paymentModel
      .find({
        createdAt: { $gte: fromDate, $lt: toDate },
        status: 'completed',
      })
      .populate('companyId')
      .cursor({ batchSize: 500 });

    try {
      for await (const doc of cursor) {
        yield doc.toObject();
      }
    } finally {
      await cursor.close();
    }
  }
}
```

### Обогащение данных в потоке

Генератор может обогащать каждый документ дополнительными данными из других коллекций. Это удобнее, чем делать множество `$lookup` в одном агрегационном запросе, особенно когда данные лежат в разных базах.

**Важно: используйте кэш для справочных данных.** В потоке платежей один и тот же пользователь или компания встречаются многократно. Без кэша каждый документ порождает запрос в БД, что приводит к тысячам дублирующихся запросов. Простой `Map` в памяти решает эту проблему — справочные сущности (User, Company) обычно занимают мало места и хорошо кэшируются.

> В примере ниже используется in-memory `Map` для простоты. В реальных системах, где несколько инстансов приложения обрабатывают данные параллельно, кэш должен быть распределённым — например, через Redis. Это исключает дублирование запросов между инстансами и позволяет переиспользовать кэш между запусками ETL.

```typescript
async *getEnrichedPayments(fromDate: Date, toDate: Date) {
  const pipeline = [
    {
      $match: {
        createdAt: { $gte: fromDate, $lt: toDate },
        status: 'completed',
      },
    },
  ];

  const cursor = this.paymentsCollection.aggregate(pipeline);

  // Кэш справочных данных — избегаем повторных запросов
  // для одного и того же userId/companyId
  const usersCache = new Map<string, any>();
  const companiesCache = new Map<string, any>();

  try {
    for await (const payment of cursor) {
      let user = usersCache.get(payment.userId);

      if (!user) {
        user = await this.usersCollection.findOne({ _id: payment.userId });
        usersCache.set(payment.userId, user);
      }

      let company = companiesCache.get(payment.companyId);

      if (!company) {
        company = await this.companiesCollection.findOne({
          _id: payment.companyId,
        });
        companiesCache.set(payment.companyId, company);
      }

      yield {
        paymentId: payment._id,
        amount: payment.amount,
        currency: payment.currency,
        userName: user?.name ?? 'Unknown',
        userEmail: user?.email ?? '',
        companyName: company?.name ?? 'Unknown',
        companyInn: company?.inn ?? '',
        createdAt: payment.createdAt,
      };
    }
  } finally {
    await cursor.close();
  }
}
```

### Параллельная обработка потока

Когда обработка каждого элемента включает медленные I/O-операции (запросы к внешним API, запись в другую БД), можно распараллелить обработку, сохранив последовательное чтение из генератора.

Для этого удобно использовать библиотеку [`@vitalyostanin/mutex-pool`](https://github.com/VitalyOstanin/mutex-pool) — пул воркеров на основе семафоров из `async-mutex`. Она решает ключевую проблему: как потреблять асинхронный итератор (курсор MongoDB, генератор) с ограничением параллелизма, не накапливая весь ввод в памяти.

Принцип работы:
- `pool.start(job)` захватывает слот через семафор и запускает задачу, не дожидаясь её завершения
- Если все слоты заняты, `start()` блокируется до освобождения слота — это создаёт естественный backpressure на источник данных
- `pool.allJobsFinished()` ожидает завершения всех запущенных задач

```bash
npm install @vitalyostanin/mutex-pool
```

```typescript
import { MutexPool } from '@vitalyostanin/mutex-pool';

// 50 параллельных воркеров обрабатывают поток платежей
const pool = new MutexPool(50);
const cursor = reportService.getEnrichedPayments(fromDate, toDate);

for await (const item of cursor) {
  await pool.start(async () => {
    await externalApi.sendPaymentReport(item);
  });
}

await pool.allJobsFinished();
```

Преимущества перед самописным решением:
- Не нужно вручную синхронизировать чтение из генератора — цикл `for await` читает последовательно, а `pool.start()` управляет параллелизмом обработки
- Корректный backpressure: если все 50 слотов заняты, цикл приостанавливается и не читает следующий элемент из курсора
- Можно отслеживать доступные слоты через `pool.getSemaphoreValue()`
- Можно дождаться завершения конкретной задачи через `jobFinished`:

```typescript
const { jobFinished } = await pool.start(async () => {
  await externalApi.sendPaymentReport(item);
});

// при необходимости дождаться именно этой задачи
await jobFinished;
```

### ETL: чтение из БД и отдача CSV в HTTP response

#### Драйвер mongodb + NestJS

```typescript
import { Controller, Get, Query, Res } from '@nestjs/common';
import { Response } from 'express';

@Controller('reports')
class ReportsController {
  constructor(private readonly reportService: ReportService) {}

  @Get('payments.csv')
  async getPaymentsCsv(
    @Query('from') from: string,
    @Query('to') to: string,
    @Res() res: Response,
  ) {
    const fromDate = new Date(from);
    const toDate = new Date(to);

    res.setHeader('Content-Type', 'text/csv; charset=utf-8');
    res.setHeader(
      'Content-Disposition',
      'attachment; filename="payments.csv"',
    );

    // BOM для корректного отображения кириллицы в Excel
    res.write('\uFEFF');
    res.write('ID;Сумма;Валюта;Пользователь;Email;Компания;ИНН;Дата\n');

    const cursor = this.reportService.getEnrichedPayments(fromDate, toDate);

    try {
      for await (const row of cursor) {
        const line = [
          row.paymentId,
          row.amount,
          row.currency,
          row.userName,
          row.userEmail,
          row.companyName,
          row.companyInn,
          row.createdAt.toISOString(),
        ].join(';');

        // res.write возвращает false, если буфер заполнен
        const canContinue = res.write(line + '\n');

        if (!canContinue) {
          await new Promise<void>((resolve) => res.once('drain', resolve));
        }
      }
    } finally {
      res.end();
    }
  }
}
```

#### Mongoose + NestJS

```typescript
@Get('payments.csv')
async getPaymentsCsv(
  @Query('from') from: string,
  @Query('to') to: string,
  @Res() res: Response,
) {
  const fromDate = new Date(from);
  const toDate = new Date(to);

  res.setHeader('Content-Type', 'text/csv; charset=utf-8');
  res.setHeader('Content-Disposition', 'attachment; filename="payments.csv"');

  res.write('\uFEFF');
  res.write('ID;Сумма;Валюта;Пользователь;Email;Компания;ИНН;Дата\n');

  const cursor = this.paymentModel
    .find({
      createdAt: { $gte: fromDate, $lt: toDate },
      status: 'completed',
    })
    .populate('companyId')
    .populate('userId')
    .cursor({ batchSize: 500 });

  try {
    for await (const doc of cursor) {
      const payment = doc.toObject();
      const line = [
        payment._id,
        payment.amount,
        payment.currency,
        payment.userId?.name ?? '',
        payment.userId?.email ?? '',
        payment.companyId?.name ?? '',
        payment.companyId?.inn ?? '',
        payment.createdAt.toISOString(),
      ].join(';');

      const canContinue = res.write(line + '\n');

      if (!canContinue) {
        await new Promise<void>((resolve) => res.once('drain', resolve));
      }
    }
  } finally {
    await cursor.close();
  }

  res.end();
}
```

---

## Приём 2: Node.js Transform Streams

### Принцип работы

Курсор MongoDB можно получить как `Readable` поток через `.stream()` (драйвер) или `.cursor()` (mongoose). Далее данные пропускаются через цепочку `Transform` потоков и направляются в `Writable` (HTTP response, файл и т.д.).

Ключевые свойства:
- Встроенный backpressure — если потребитель не успевает, чтение автоматически приостанавливается
- Хорошая интеграция с `stream.pipeline()`, который корректно обрабатывает ошибки и закрывает все потоки
- Можно использовать готовые библиотеки трансформации (csv-stringify, zlib и др.)
- Подходит для случаев, когда нужно отдать результат напрямую в response без промежуточной буферизации

### Пример: драйвер mongodb

```typescript
import { Transform } from 'stream';
import { pipeline } from 'stream/promises';

class PaymentCsvTransform extends Transform {
  constructor() {
    super({ objectMode: true });
  }

  _transform(
    doc: any,
    _encoding: string,
    callback: (error?: Error, data?: string) => void,
  ) {
    const line = [
      doc._id,
      doc.amount,
      doc.currency,
      doc.company?.name ?? '',
      doc.company?.inn ?? '',
      doc.createdAt?.toISOString() ?? '',
    ].join(';');

    callback(null, line + '\n');
  }
}

class ReportService {
  constructor(private readonly mongoClient: MongoClient) {}

  getPaymentsStream(fromDate: Date, toDate: Date) {
    const collection = this.mongoClient.db('billing').collection('payments');

    const cursor = collection.aggregate([
      {
        $match: {
          createdAt: { $gte: fromDate, $lt: toDate },
          status: 'completed',
        },
      },
      {
        $lookup: {
          from: 'companies',
          localField: 'companyId',
          foreignField: '_id',
          as: 'company',
        },
      },
      { $unwind: '$company' },
    ]);

    // .stream() возвращает Node.js Readable
    return cursor.stream();
  }
}
```

### Пример: mongoose

```typescript
class ReportService {
  constructor(
    @InjectModel(Payment.name) private paymentModel: Model<Payment>,
  ) {}

  getPaymentsStream(fromDate: Date, toDate: Date) {
    return this.paymentModel
      .find({
        createdAt: { $gte: fromDate, $lt: toDate },
        status: 'completed',
      })
      .populate('companyId')
      .cursor({ batchSize: 500 })
      // mongoose cursor реализует Readable
      .pipe(
        new Transform({
          objectMode: true,
          transform(doc, _encoding, callback) {
            const payment = doc.toObject();
            callback(null, payment);
          },
        }),
      );
  }
}
```

### ETL: чтение из БД и отдача CSV в HTTP response

#### Драйвер mongodb + NestJS

```typescript
import { pipeline } from 'stream/promises';
import { Transform } from 'stream';

@Controller('reports')
class ReportsController {
  constructor(private readonly reportService: ReportService) {}

  @Get('payments.csv')
  async getPaymentsCsv(
    @Query('from') from: string,
    @Query('to') to: string,
    @Res() res: Response,
  ) {
    const fromDate = new Date(from);
    const toDate = new Date(to);

    res.setHeader('Content-Type', 'text/csv; charset=utf-8');
    res.setHeader(
      'Content-Disposition',
      'attachment; filename="payments.csv"',
    );
    res.write('\uFEFF');
    res.write('ID;Сумма;Валюта;Компания;ИНН;Дата\n');

    const source = this.reportService.getPaymentsStream(fromDate, toDate);

    const toCsv = new Transform({
      objectMode: true,
      transform(doc, _encoding, callback) {
        const line = [
          doc._id,
          doc.amount,
          doc.currency,
          doc.company?.name ?? '',
          doc.company?.inn ?? '',
          doc.createdAt?.toISOString() ?? '',
        ].join(';');

        callback(null, line + '\n');
      },
    });

    // pipeline автоматически:
    // - обрабатывает backpressure
    // - закрывает все потоки при ошибке
    // - закрывает курсор MongoDB
    await pipeline(source, toCsv, res);
  }
}
```

#### Mongoose + NestJS

```typescript
import { pipeline } from 'stream/promises';
import { Transform } from 'stream';

@Get('payments.csv')
async getPaymentsCsv(
  @Query('from') from: string,
  @Query('to') to: string,
  @Res() res: Response,
) {
  const fromDate = new Date(from);
  const toDate = new Date(to);

  res.setHeader('Content-Type', 'text/csv; charset=utf-8');
  res.setHeader('Content-Disposition', 'attachment; filename="payments.csv"');
  res.write('\uFEFF');
  res.write('ID;Сумма;Валюта;Пользователь;Email;Компания;ИНН;Дата\n');

  const source = this.paymentModel
    .find({
      createdAt: { $gte: fromDate, $lt: toDate },
      status: 'completed',
    })
    .populate('companyId')
    .populate('userId')
    .cursor({ batchSize: 500 });

  const toCsv = new Transform({
    objectMode: true,
    transform(doc, _encoding, callback) {
      const p = doc.toObject();
      const line = [
        p._id,
        p.amount,
        p.currency,
        p.userId?.name ?? '',
        p.userId?.email ?? '',
        p.companyId?.name ?? '',
        p.companyId?.inn ?? '',
        p.createdAt?.toISOString() ?? '',
      ].join(';');

      callback(null, line + '\n');
    },
  });

  await pipeline(source, toCsv, res);
}
```

---

## Сравнение подходов

| Критерий | Async Generators | Transform Streams |
|---|---|---|
| **Читаемость кода** | Высокая, обычный императивный стиль | Средняя, callback-based API |
| **Backpressure** | Ручной (`res.write` + `drain`) | Автоматический через `pipeline()` |
| **Обогащение данных** | Просто — await внутри цикла | Сложнее — callback не поддерживает await напрямую |
| **Композиция** | Вложенные генераторы, `yield*` | `.pipe()` цепочки |
| **Параллельная обработка** | Через ConcurrentGeneratorWorker | Через `parallel-transform` или подобные библиотеки |
| **Интеграция с fs/http** | Требует ручного моста | Нативная через `pipeline()` |
| **Обработка ошибок** | try/catch | `pipeline()` обрабатывает автоматически |
| **Закрытие курсора** | Ручное `cursor.close()` | Автоматическое через `pipeline()` |

---

## Рекомендации

**Используйте async generators когда:**
- Нужно обогащать каждый документ дополнительными запросами к другим коллекциям/сервисам
- Логика трансформации сложная и содержит условные ветвления
- Нужна параллельная обработка с контролем concurrency (ConcurrentGeneratorWorker)
- Важна читаемость и простота отладки

**Используйте Transform Streams когда:**
- Данные идут напрямую из курсора в HTTP response или файл
- Трансформация простая (форматирование, фильтрация)
- Важен автоматический backpressure без ручного кода
- Нужна интеграция со сжатием (gzip), шифрованием или другими потоковыми библиотеками

**Общие рекомендации:**
- Всегда указывайте `batchSize` при создании курсора (500-1000 — разумное значение)
- Для CSV-отчётов добавляйте BOM (`\uFEFF`) для корректного отображения в Excel
- Используйте `;` как разделитель в CSV для совместимости с русскоязычными локалями Excel
- При использовании `$lookup` в агрегации убедитесь, что есть индексы на полях join
- Для больших отчётов добавляйте `allowDiskUse: true` в опции агрегации
- Читайте данные для отчётов с secondary-узлов replica set (см. раздел ниже)

### Чтение с secondary-узлов MongoDB

ETL-запросы и формирование отчётов — это тяжёлые read-операции, которые могут нагружать primary-узел и влиять на производительность основного приложения. MongoDB позволяет направлять такие запросы на secondary-узлы через настройку `readPreference`.

Рекомендуемые значения для ETL:

- `secondaryPreferred` — читать с secondary, если доступен; fallback на primary. Оптимальный выбор для большинства случаев.
- `secondary` — строго с secondary. Используйте, если допустимо получить ошибку при отсутствии доступных secondary-узлов.

> Данные на secondary могут отставать от primary на время репликации (обычно миллисекунды, но при нагрузке — секунды). Для отчётов это допустимо, но не подходит для операций, требующих чтения актуальных данных сразу после записи.

#### Драйвер mongodb

```typescript
// Вариант 1: на уровне подключения (для выделенного ETL-клиента)
const etlClient = new MongoClient(uri, {
  readPreference: 'secondaryPreferred',
});

// Вариант 2: на уровне коллекции
const collection = db.collection('payments', {
  readPreference: 'secondaryPreferred',
});

// Вариант 3: на уровне конкретного запроса
const cursor = collection.aggregate(pipeline, {
  readPreference: 'secondaryPreferred',
  allowDiskUse: true,
});
```

#### Mongoose

```typescript
// Вариант 1: на уровне запроса
const cursor = this.paymentModel
  .find({ status: 'completed' })
  .read('secondaryPreferred')
  .cursor({ batchSize: 500 });

// Вариант 2: на уровне схемы (для моделей, используемых только в отчётах)
const PaymentReportSchema = new Schema({ ... }, {
  read: 'secondaryPreferred',
});
```

Рекомендуется создавать отдельное подключение (`MongoClient`) для ETL-процессов с `readPreference: 'secondaryPreferred'`, чтобы не влиять на настройки основного подключения приложения.
