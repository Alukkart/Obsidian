## Клиент

Импортируем и создаем экземпляр клиента `Prisma`:

```ts
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

export default prisma
```

Иногда может потребоваться делать так:

```ts
const Prisma = require('prisma')

const prisma = new Prisma.PrismaClient()

module.exports = prisma
```

### Запросы

#### findUnique

`findUnique` позволяет извлекать единичные записи по идентификатору или уникальному полю.

**Сигнатура**

```ts
findUnique({
  where: condition,
  select?: fields,
  include?: relations,
  rejectOnNotFound?: boolean
})
```

Модификатор `?` означает, что поле является опциональным.

- `condition` - условие для выборки;
- `fields` - поля для выборки;
- `relations` - отношения (связанные поля) для выборки;
- `rejectOnNotFound` - если имеет значение `true`, при отсутствии записи выбрасывается исключение `NotFoundError`. Если имеет значение `false`, при отсутствии записи возвращается `null`.

**Пример**

```ts
async function getUserById(id) {
  try {
    const user = await prisma.user.findUnique({
      where: {
        id
      }
    })
    return user
  } catch(e) {
    onError(e)
  }
}
```

#### findFirst

`findFirst` возвращает первую запись, соответствующую заданному критерию.

**Сигнатура**

```ts
findFirst({
  where?: condition,
  select?: fields,
  include?: relations,
  rejectOnNotFound?: boolean,
  distinct?: field,
  orderBy?: order,
  cursor?: position,
  skip?: number,
  take?: number
})
```

- `distinct` - фильтрация по определенному полю;
- `orderBy` - сортировка по определенному полю и в определенном порядке;
- `cursor` - позиция начала списка (как правило, `id` или другое уникальное значение);
- `skip` - количество пропускаемых записей;
- `take` - количество возвращаемых записей (в данном случае может иметь значение `1` или `-1`: во втором случае возвращается последняя запись.

**Пример**

```ts
async function getLastPostByAuthorId(author_id) {
  try {
    const post = await prisma.post.findFirst({
      where: {
        author_id
      },
      orderBy: {
        created_at: 'asc'
      },
      take: -1
    })
    return post
  } catch(e) {
    onError(e)
  }
}
```

#### findMany

`findMany` возвращает все записи, соответствующие заданному критерию.

**Сигнатура**

```ts
findMany({
  where?: condition,
  select?: fields,
  include?: relations,
  rejectOnNotFound?: boolean,
  distinct?: field,
  orderBy?: order,
  cursor?: position,
  skip?: number,
  take?: number
})
```

**Пример**

```ts
async function getAllPostsByAuthorId(author_id) {
  try {
    const posts = await prisma.post.findMany({
      where: {
        author_id
      },
      orderBy: {
        updated_at: 'desc'
      }
    })
    return posts
  } catch(e) {
    onError(e)
  }
}
```

#### create

`create` создает новую запись.

**Сигнатура**

```ts
create({
  data: _data,
  select?: fields,
  include?: relations
})
```

- `_data` - данные создаваемой записи.

**Пример**

```ts
async function createUserWithProfile(data) {
  const { email, password, firstName, lastName, age } = data
  try {
    const hash = await argon2.hash(password)
    const user = await prisma.user.create({
      data: {
        email,
        hash,
        profile: {
          create: {
            first_name: firstName,
            last_name: lastName,
            age
          }
        }
      },
      select: {
        email: true
      },
      include: {
        profile: true
      }
    })
    return user
  } catch(e) {
    onError(e)
  }
}
```

#### update

`update` обновляет существующую запись.

**Сигнатура**

```ts
update({
  data: _data,
  where: condition,
  select?: fields,
  include?: relations
})
```

**Пример**

```ts
async function updateUserById(id, changes) {
  const { email, age } = changes
  try {
    const user = await prisma.user.update({
      where: {
        id
      },
      data: {
        email,
        profile: {
          update: {
            age
          }
        }
      },
      select: {
        email: true
      },
      include: {
        profile: true
      }
    })
    return user
  } catch(e) {
    onError(e)
  }
}
```

#### upsert

`upsert` обновляет существующую или создает новую запись.

**Сигнатура**

```ts
upsert({
  create: _data,
  update: _data,
  where: condition,
  select?: fields,
  include?: relations
})
```

**Пример**

```ts
async function updateOrCreateUser(data) {
  const { userName, email, password } = data
  try {
    const hash = await argon2.hash(password)
    const user = await prisma.user.create({
      where: { user_name: userName },
      update: {
        email,
        hash
      },
      create: {
        email,
        hash,
        user_name: userName
      },
      select: { user_name: true, email: true }
    })
    return user
  } catch(e) {
    onError(e)
  }
}
```

#### delete

`delete` удаляет существующую запись по идентификатору или уникальному полю.

**Сигнатура**

```ts
delete({
  where: condition,
  select?: fields,
  include?: relations
})
```

**Пример**

```ts
async function removeUserById(id) {
  try {
    await prisma.user.delete({
      where: {
        id
      }
    })
  } catch(e) {
    onError(e)
  }
}
```

#### createMany

`createMany` создает несколько записей с помощью одной транзакции (о транзакциях мы поговорим отдельно).

**Пример**

```ts
createMany({
  data: _data[],
  skipDuplicates?: boolean
})
```

- `_data[]` - данные для создаваемых записей в виде массива;
- `skipDuplicates` - при значении `true` создаются только уникальные записи.

**Пример**

```ts
// предположим, что `users` - это массив объектов
async function createUsers(users) {
  try {
    const users = await prisma.user.createMany({
      data: users
    })
    return users
  } catch(e) {
    onError(e)
  }
}
```

#### updateMany

`updateMany` обновляет несколько существующих записей за один раз и возвращает количество (sic) обновленных записей.

**Сигнатура**

```ts
updateMany({
  data: _data[],
  where?: condition
})
```

**Пример**

```ts
async function updateProductsByCategory(category, newDiscount) {
  try {
    const count = await prisma.product.updateMany({
      where: {
        category
      },
      data: {
        discount: newDiscount
      }
    })
    return count
  } catch(e) {
    onError(e)
  }
}
```

#### deleteMany

`deleteMany` удаляет несколько записей с помощью одной транзакции и возвращает количество удаленных записей.

**Сигнатура**

```ts
deleteMany({
  where?: condition
})
```

**Пример**

```ts
async function removeAllPostsByUserId(author_id) {
  try {
    const count = await prisma.post.deleteMany({
      where: {
        author_id
      }
    })
    return count
  } catch(e) {
    onError(e)
  }
}
```

#### count

`count` возвращает количество записей, соответствующих заданному критерию.

**Сигнатура**

```ts
count({
  where?: condition,
  select?: fields,
  cursor?: position,
  orderBy?: order,
  skip?: number,
  take?: number
})
```

**Пример**

```ts
async function countUsersWithPublishedPosts() {
  try {
    const count = await prisma.user.count({
      where: {
        post: {
          some: {
            published: true
          }
        }
      }
    })
    return count
  } catch(e) {
    onError(e)
  }
}
```

#### aggregate

`aggregate` выполняет [агрегирование](https://ru.wikipedia.org/wiki/%D0%90%D0%B3%D1%80%D0%B5%D0%B3%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5_(%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5)) полей.

**Сигнатура**

```ts
aggregate({
  where?: condition,
  select?: fields,
  cursor?: position,
  orderBy?: order,
  skip?: number,
  take?: number,

  _count: count,
  _avg: avg,
  _sum: sum,
  _min: min,
  _max: max
})
```

- `_count` - возвращает количество совпадающих записей или не `null-полей`;
- `_avg` - возвращает среднее значение определенного поля;
- `_sum` - возвращает сумму значений определенного поля;
- `_min` - возвращает наименьшее значение определенного поля;
- `_max` - возвращает наибольшее значение определенного поля.

**Пример**

```ts
async function getAllUsersCountAndMinMaxProfileViews() {
  try {
    const result = await prisma.user.aggregate({
      _count: {
        _all: true
      },
      _max: {
        profileViews: true
      },
      _min: {
        profileViews: true
      }
    })
    return result
  } catch(e) {
    onError(e)
  }
}
```

#### groupBy

`groupBy` выполняет группировку полей.

**Сигнатура**

```ts
groupBy({
  by?: by,
  having?: having,

  where?: condition,
  orderBy?: order,
  skip?: number,
  take?: number,

  _count: count,
  _avg: avg,
  _sum: sum,
  _min: min,
  _max: max
})
```

- `by` - определяет поле или комбинацию полей для группировки записей;
- `having` - позволяет фильтровать группы по агрегируемому значению.

**Пример**

В следующем примере мы выполняем группировку по `country / city`, где среднее значение `profileViews` превышает `100`, и возвращаем общее количество (`_sum`) `profileViews` для каждой группы. Запрос также возвращает количество всех (`_all`) записей в каждой группе и все записи с не `null` значениями поля `city` в каждой группе:

```ts
async function getUsers() {
  try {
    const result = await prisma.user.groupBy({
      by: ['country', 'city'],
      _count: {
        _all: true,
        city: true
      },
      _sum: {
        profileViews: true
      },
      orderBy: {
        country: 'desc'
      },
      having: {
        profileViews: {
          _avg: {
            gt: 100
          }
        }
      }
    })
    return result
  } catch(e) {
    onError(e)
  }
}
```

### Настройки

#### select

`select` определяет, какие поля включаются в возвращаемый объект.

```ts
const user = await prisma.user.findUnique({
  where: { email },
  select: {
    id: true,
    email: true,
    first_name: true,
    last_name: true,
    age: true
  }
})

// or
const usersWithPosts = await prisma.user.findMany({
  select: {
    id: true,
    email: true,
    posts: {
      select: {
        id: true,
        title: true,
        content: true,
        author_id: true,
        created_at: true
      }
    }
  }
})

// or
const usersWithPostsAndComments = await prisma.user.findMany({
  select: {
    id: true,
    email: true,
    posts: {
      include: {
        comments: true
      }
    }
  }
})
```

#### include

`include` определяет, какие отношения (связанные записи) включаются в возвращаемый объект.

```ts
const userWithPostsAndComments = await prisma.user.findUnique({
  where: { email },
  include: {
    posts: true,
    comments: true
  }
})
```

#### where

`where` определяет один или более фильтр (о фильтрах мы поговорим отдельно), применяемый к свойствам записи или связанных записей:

```ts
const admins = await prisma.user.findMany({
  where: {
    email: {
      contains: 'admin'
    }
  }
})
```

#### orderBy

`orderBy` определяет поля и порядок сортировки. Возможными значениями `orderBy` являются `asc` и `desc`.

```ts
const usersByPostCount = await prisma.user.findMany({
  orderBy: {
    posts: {
      count: 'desc'
    }
  }
})
```

#### distinct

`distinct` определяет поля, которые должны быть уникальными в возвращаемом объекте.

```ts
const distinctCities = await prisma.user.findMany({
  select: {
    city: true,
    country: true
  },
  distinct: ['city']
})
```

### Вложенные запросы

- `create: { data } | [{ data1 }, { data2 }, ...{ dataN }]` - добавляет новую связанную запись или набор записей в родительскую запись. `create` доступен при создании (`create`) новой родительской записи или обновлении (`update`) существующей родительской записи

```ts
const user = await prisma.user.create({
  data: {
    email,
    profile: {
      // вложенный запрос
      create: {
        first_name,
        last_name
      }
    }
  }
})
```

- `createMany: [{ data1 }, { data2 }, ...{ dataN }]` - добавляет набор новых связанных записей в родительскую запись. `createMany` доступен при создании (`create`) новой родительской записи или обновлении (`update`) существующей родительской записи

```ts
const userWithPosts = await prisma.user.create({
  data: {
    email,
    posts: {
      // !
      createMany: {
        data: posts
      }
    }
  }
})
```

- `update: { data } | [{ data1 }, { data2 }, ...{ dataN }]` - обновляет одну или более связанных записей

```ts
const user = await prisma.user.update({
  where: { email },
  data: {
    profile: {
      // !
      update: { age }
    }
  }
})
```

- `updateMany: { data } | [{ data1 }, { data2 }, ...{ dataN }]` - обновляет массив связанных записей. Поддерживается фильтрация

```ts
const result = await prisma.user.update({
  where: { id },
  data: {
    posts: {
      // !
      updateMany: {
        where: {
          published: false
        },
        data: {
          like_count: 0
        }
      }
    }
  }
})
```

- `upsert: { data } | [{ data1 }, { data2 }, ...{ dataN }]` - обновляет существующую связанную запись или создает новую

```ts
const user = await prisma.user.update({
  where: { email },
  data: {
    profile: {
      // !
      upsert: {
        create: { age },
        update: { age }
      }
    }
  }
})
```

- `delete: boolean | { data } | [{ data1 }, { data2 }, ...{ dataN }]` - удаляет связанную запись. Родительская запись при этом не удаляется

```ts
const user = await prisma.user.update({
  where: { email },
  data: {
    profile: {
      delete: true
    }
  }
})
```

- `deleteMany: { data } | [{ data1 }, { data2 }, ...{ dataN }]` - удаляет связанные записи. Поддерживается фильтрация

```ts
const user = await prisma.user.update({
  where: { id },
  data: {
    age,
    posts: {
      // !
      deleteMany: {}
    }
  }
})
```

- `set: { data } | [{ data1 }, { data2 }, ...{ dataN }]` - перезаписывает значение связанной записи

```ts
const userWithPosts = await prisma.user.update({
  where: { email },
  data: {
    posts: {
      // !
      set: newPosts
    }
  }
})
```

- `connect` - подключает запись к существующей связанной записи по идентификатору или уникальному полю

```ts
const user = await prisma.post.create({
  data: {
    title,
    content,
    author: {
      connect: { email }
    }
  }
})
```

- `connectOrCreate` - подключает запись к существующей связанной записи по идентификатору или уникальному полю либо создает связанную запись при отсутствии таковой;
- `disconnect` - отключает родительскую запись от связанной без удаления последней. `disconnect` доступен только если отношение является опциональным.

### Фильтры и операторы

#### Фильтры

- `equals` - значение равняется `n`

```ts
const usersWithNameHarry = await prisma.user.findMany({
  where: {
    name: {
      equals: 'Harry'
    }
  }
})

// `equals` может быть опущено
const usersWithNameHarry = await prisma.user.findMany({
  where: {
    name: 'Harry'
  }
})
```

- `not` - значение не равняется `n`;
- `in` - значение `n` содержится в списке (массиве)

```ts
const usersWithNameAliceOrBob = await prisma.user.findMany({
  where: {
    user_name: {
      // !
      in: ['Alice', 'Bob']
    }
  }
})
```

- `notIn` - `n` не содержится в списке;
- `lt` - `n` меньше `x`

```ts
const notPopularPosts = await prisma.post.findMany({
  where: {
    likeCount: {
      lt: 100
    }
  }
})
```

- `lte` - `n` меньше или равно `x`;
- `gt` - `n` больше `x`;
- `gte` - `n` больше или равно `x`;
- `contains` - `n` содержит `x`

```ts
const admins = await prisma.user.findMany({
  where: {
    email: {
      contains: 'admin'
    }
  }
})
```

- `startsWith` - `n` начинается с `x`

```ts
const usersWithNameStartsWithA = await prisma.user.findMany({
  where: {
    user_name: {
      startsWith: 'A'
    }
  }
})
```

- `endsWith` - `n` заканчивается `x`.

#### Операторы

- `AND` - все условия должны возвращать `true`

```ts
const notPublishedPostsAboutTypeScript = await prisma.post.findMany({
  where: {
    AND: [
      {
        title: {
          contains: 'TypeScript'
        }
      },
      {
        published: false
      }
    ]
  }
})
```

_Обратите внимание_: оператор указывается до названия поля (снаружи поля), а фильтр после (внутри).

- `OR` - хотя бы одно условие должно возвращать `true`;
- `NOT` - все условия должны возвращать `false`.

### Фильтры для связанных записей

- `some` - возвращает все связанные записи, соответствующие одному или более критерию фильтрации

```ts
const usersWithPostsAboutTypeScript = await prisma.user.findMany({
  where: {
    posts: {
      some: {
        title: {
          contains: 'TypeScript'
        }
      }
    }
  }
})
```

- `every` - возвращает все связанные записи, соответствующие всем критериям;
- `none` - возвращает все связанные записи, не соответствующие ни одному критерию;
- `is` - возвращает все связанные записи, соответствующие критерию;
- `notIs` - возвращает все связанные записи, не соответствующие критерию.

### Методы клиента

- `$disconnect` - закрывает соединение с БД, которое было установлено после вызова метода `$connect` (данный метод чаще всего не требуется вызывать явно), и останавливает движок запросов (query engine) `Prisma`

```ts
import { PrismaClient } from '@prisma/client'

const prisma = new PrismaClient()

async function seedDb() {
  try {
    await prisma.model.create(data)
  } catch (e) {
    onError(e)
  } finally {
    // !
    await prisma.$disconnect()
  }
}
```

- `$use` - добавляет посредника (middleware)

```ts
prisma.$use(async (params, next) => {  
console.log('Это посредник')  
  
// работаем с `params`  
  
return next(params)  
})
```

- `next` - представляет "следующий уровень" в стеке посредников. Таким уровнем может быть следующий посредник или движок запросов `Prisma`;
    
- `params` - объект со следующими свойствами:
    
    - `action` - тип запроса, например, `create` или `findMany`;
    - `args` - аргументы, переданные в запрос, например, `where` или `data`;
    - `model` - модель, например, `User` или `Post`;
    - `runInTransaction` - возвращает `true`, если запрос был запущен в контексте транзакции;
- методы `$queryRaw`, `$executeRaw` и `$runCommandRaw` предназначены для работы с `SQL`. Почитать о них можно [здесь](https://www.prisma.io/docs/concepts/components/prisma-client/raw-database-access);
    
- `$transaction` - выполняет запросы в контексте транзакции (см. ниже).
    

Подробнее о клиенте можно почитать [здесь](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference).

### Транзакции
Транзакция - это последовательность операций чтения/записи, которые обрабатываются как единое целое, т.е. либо все операции завершаются успешно, либо все операции отклоняются с ошибкой.

`Prisma` позволяет использовать транзакции тремя способами:

- вложенные запросы (см. выше): операции с родительскими и связанными записями выполняются в контексте одной транзакции

```ts
const newUserWithProfile = await prisma.user.create({
  data: {
    email,
    profile: {
      // !
      create: {
        first_name,
        last_name
      }
    }
  }
})
```

- пакетированные/массовые (batch/bulk) транзакции: выполнение нескольких операций за один раз с помощью таких запросов, как `createMany`, `updateMany` и `deleteMany`

```ts
const removedUser = await prisma.user.delete({
  where: {
    email
  }
})

// !
await prisma.post.deleteMany({
  where: {
    author_id: removedUser.id
  }
})
```

- вызов метода `$transaction`.

#### $transaction

Интерфейс `$transaction` может быть использован в двух формах:

- `$transaction([ query1, query2, ...queryN ])` - принимает массив последовательно выполняемых запросов;
- `$transaction(fn)` - принимает функцию, которая может включать запросы и другой код.

Пример транзакции, возвращающей посты, в заголовке которых встречается слово `TypeScript` и общее количество постов:

```ts
const [postsAboutTypeScript, totalPostCount] = await prisma.$transaction([
  prisma.post.findMany({ where: { title: { contains: 'TypeScript' } } }),
  prisma.post.count()
])
```

В `$transaction` допускается использование `SQL`:

```ts
const [userNames, updatedUser] = await prisma.$transaction([
  prisma.$queryRaw`SELECT 'user_name' FROM users`,
  prisma.$executeRaw`UPDATE users SET user_name = 'Harry' WHERE id = 42`
])
```

#### Интерактивные транзакции

Интерактивные транзакции предоставляют разработчикам больший контроль над выполняемыми в контексте транзакции операциями. В данный момент они имеют статус экспериментальной возможности, которую можно включить следующим образом:

```ts
generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["interactiveTransactions"]
}
```

Рассмотрим пример совершения платежа.

Предположим, что у `Alice` и `Bob` имеется по `100$` на счетах (account), и `Alice` хочет отправить `Bob` свои `100$`.

```ts
import { PrismaClient } from '@prisma/client'
const prisma = new PrismaClient()

async function transfer(from, to, amount) {
  try {
    await prisma.$transaction(async (prisma) => {
      // 1. Уменьшаем баланс отправителя
      const sender = await prisma.account.update({
        data: {
          balance: {
            decrement: amount
          }
        },
        where: {
          email: from
        }
      })

      // 2. Проверяем, что баланс отправителя после уменьшения >= 0
      if (sender.balance < 0) {
        throw new Error(`${from} имеет недостаточно средств для отправки ${amount}`)
      }

      // 3. Увеличиваем баланс получателя
      const recipient = await prisma.account.update({
        data: {
          balance: {
            increment: amount
          }
        },
        where: {
          email: to
        }
      })

      return recipient
    })
  } catch(e) {
    // обрабатываем ошибку
  }
}

async function main() {
  // эта транзакция разрешится
  await transfer('alice@mail.com', 'bob@mail.com', 100)
  // а эта провалится
  await transfer('alice@mail.com', 'bob@mail.com', 100)
}

main().finally(() => {
  prisma.$disconnect()
})
```

Подробнее о транзакциях можно почитать [здесь](https://www.prisma.io/docs/concepts/components/prisma-client/transactions).