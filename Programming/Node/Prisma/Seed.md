---
tags:
  - prisma
---
# Seed
Seed(сид) - "Зерно" генерации, позволяет генерировать тестовый(и не только) набор данных. Например можно использовать для автоматической генерации двух пользователей с разными уровнями доступа: Админ и нищеёб. Так с помощью сид'а можно не тратить время на ручное создание данных в таблице после каждого сброса этой самой таблицы.

## Необходимые настройки package.json
### Next.js:
```json
"prisma": {
	"seed": "ts-node --compiler-options {\"module\":\"CommonJS\"} prisma/seed.ts"
},
```
### В остальных случаях:
```json
  "prisma": {
    "seed": "ts-node prisma/seed.ts"
  },
```
так же необходимо установить ts-node:
`npm install -D typescript ts-node @types/node`
## Seed best practices
```ts
// ~prisma/seed.ts
async function up() { // Добавление всех необходимых записей
	... 
}

async function down() { // Удаление всех созданных таблиц
	await prisma.$executeRaw`TURNCATE TABLE "User" RESTART IDENTITY CASCADE;` // User заменяется на название таблицы для удаления
    ... 
}

async function main() { // main функция поочередно вызывает функции up и down чтобы избежать дубликации данных
    try{
        await down()
        await up()
    }catch(e){
        console.error(e)
    }
}

main()
    .then(async () => {
        await prisma.$disconnect()
    })
    .catch(async (e) => {
        console.error(e)
        await prisma.$disconnect()
        process.exit(1)
    })
```

