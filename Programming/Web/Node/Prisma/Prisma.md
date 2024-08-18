---
tags:
  - DB
  - nextjs
  - ts
  - prisma
---
[Prisma](https://www.prisma.io/) - это современное [ORM](https://ru.wikipedia.org/wiki/ORM) (Object Relational Mapping - объектно-реляционное отображение или связывание) для [Node.js](https://nodejs.org/en/) и [TypeScript](https://www.typescriptlang.org/). Проще говоря, `Prisma` - это инструмент, позволяющий работать с реляционными (`PostgreSQL`, `MySQL`, `SQL Server`, `SQLite`) и нереляционной (`MongoDB`) базами данных с помощью `JavaScript` или `TypeScript` без использования [`SQL`](https://ru.wikipedia.org/wiki/SQL) (хотя такая возможность имеется).

Преимущества:
- Лучший ORM для Node.js.
- Работает только с TypeScript, что позволяет на моменте написания кода понимать что вернет запрос.
- Подходит как для реляционных так и не реляционных баз данных.
- Не требует умения писать SQL-запросы(но такая возможность есть), что упрощает использование.
- Соответствует [[ACID (noSQL)]]  (вроде) 
Недостатки:
- Работает только с TypeScript, что делает его использование практически невозможным с ванильным JS.
- Невозможно построить сложные запросы без использования SQL-запросов.

## Помощь
В [документации](https://www.prisma.io/docs/getting-started) в правом нижнем углу страницы находится кнопка `Ask AI` для вызова диалогового окна с ИИ, который ответит на все ваши вопросы связанные с `Prisma`.
![[Pasted image 20240818180725.png]]
## Инициализация проекта

Создаем директорию, переходим в нее и инициализируем `Node.js-проект`:

```bash
mkdir prisma-test
cd prisma-test

yarn init -yp
# or
npm init -y
```

Устанавливаем `Prisma` в качестве зависимости для разработки:

```bash
yarn add -D prisma
# or
npm i -D prisma
```

Инициализируем проект `Prisma`:

```bash
npx prisma init
```

Это приводит к генерации файлов `prisma/schema.prisma` и `.env`.

В файле `.env` содержится переменная `DATABASE_URL`, значением которой является путь к (адрес) БД. Файл `schema.prisma` мы рассмотрим в [[Schema]].

## CLI

[Интерфейс командной строки](https://ru.wikipedia.org/wiki/%D0%98%D0%BD%D1%82%D0%B5%D1%80%D1%84%D0%B5%D0%B9%D1%81_%D0%BA%D0%BE%D0%BC%D0%B0%D0%BD%D0%B4%D0%BD%D0%BE%D0%B9_%D1%81%D1%82%D1%80%D0%BE%D0%BA%D0%B8) (Command line interface, CLI) `Prisma` предоставляет следующие основные возможности (команды):

- `init` - создает шаблон `Prisma-проекта`:
    - `--datasource-provider` - провайдер для работы с БД: `sqlite`, `postgresql`, `mysql`, `sqlserver` или `mongodb` (перезаписывает `datasource` из `schema.prisma`);
    - `--url` - адрес БД (перезаписывает `DATABASE_URL`)

```bash
npx prisma init --datasource-provider mysql --url mysql://user:password@localhost:3306/mydb
```

- `generate` - генерирует клиента `Prisma` на основе схемы (`schema.prisma`). Клиент `Prisma` предоставляет [программный интерфейс приложения](https://ru.wikipedia.org/wiki/API) (Application Programming Interface, API) для работы с моделями и типы для `TypeScript`

```bash
npx prisma generate
```

- `db`
    - `pull` - генерирует модели на основе существующей схемы БД

```bash
npx prisma db pull
```

- `push` - синхронизирует состояние схемы `Prisma` с БД без выполнения миграций. БД создается при отсутствии. Используется для прототипировании БД и в локальной разработке. Также может быть полезной в случае ограниченного доступа к БД, например, при использовании БД, предоставляемой облачными провайдерами, такими как [`ElephantSQL`](https://www.elephantsql.com/) или [`Heroku`](https://www.heroku.com/)

```
npx prisma db push
```

- `seed` - выполняет скрипт для наполнения БД начальными (фиктивными) данными. Путь к соответствующему файлу определяется в `package.json`, подробнее в [[Seed]]

```json
"prisma": {  "seed": "node prisma/seed.js"}
```

```bash
npx prisma seed
```

- `migrate`
    - `dev` - выполняет миграцию для разработки:
        - `--name` - название миграции

```bash
npx prisma migrate dev --name init
```

Это приводит к созданию БД при ее отсутствии, генерации файла `prisma/migrations/migration_name.sql`, выполнению инструкции из этого файла (синхронизации БД со схемой) и генерации (регенерации) клиента (`prisma generate`).

Данная команда должна выполняться после каждого изменения схемы.

- `reset` - удаляет и заново создает БД или выполняет "мягкий сброс", удаляя все данные, таблицы, индексы и другие артефакты

```bash
npx prisma migrate reset
```

- `deploy` - выполняет производственную миграцию

```bash
npx prisma migrate deploy
```

- `studio` - позволяет просматривать и управлять данными, хранящимися в БД, в интерактивном режиме:
    - `--browser`, `-b` - название браузера (по умолчанию используется дефолтный браузер);
    - `--port`, `-p` - номер порта (по умолчанию - `5555`)

```bash
npx prisma studio

npx prisma studio -b none # без автоматического открытия вкладки браузера
```

Связи:
- [[Schema]]
- [[Seed]]
- [[Client]]