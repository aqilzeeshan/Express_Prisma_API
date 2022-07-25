# How To Build a REST API with Express, Prisma and PostgreSQL

[This](https://www.digitalocean.com/community/tutorials/how-to-build-a-rest-api-with-prisma-and-postgresql) tutorial was followed.

1. Create directory `Express_Prisma_API` and initialze npm and install prisma and initialize it.
```
npm init -y
npm install -D prisma
npx prisma init
```

2. Create PostgreSql db in `https://railway.app/` and get connection info. It also created a `.env` file, in case you didn’t have one already, with the DATABASE_URL environment variable. Update it with connection info of db.

3. This will create a `prisma` folder, and inside it, a `schema.prisma` file. Update it.
```
model User {
  id    Int     @default(autoincrement()) @id
  email String  @unique
  name  String?
  posts Post[]
}

model Post {
  id        Int     @default(autoincrement()) @id
  title     String
  content   String?
  published Boolean @default(false)
  author    User?   @relation(fields: [authorId], references: [id])
  authorId  Int?
}
```

4. Now we need to sync the database with our schema. We do that by running the command npx prisma migrate to create our first migration:
```
npx prisma migrate dev --name "init"
```
and a file in your codebase in the prisma/migrations folder with the commands used to create those tables.
Any time you change the schema you need to run this `npx prisma migrate dev` command to apply the changes.

5. Now install the @prisma/client package with
```
npm install @prisma/client
```

6. Test it with `node test.js`
```
const {PrismaClient} = require('@prisma/client')

const prisma = new PrismaClient()

async function main() {
    const newUser = await prisma.user.create({
      data: {
        name: 'Alice',
        email: 'alice@prisma.io',
        posts: {
          create: {
            title: 'Hello World',
          },
        },
      },
    })
    console.log('Created new user: ', newUser)
  
    const allUsers = await prisma.user.findMany({
      include: { posts: true },
    })
    console.log('All users: ')
    console.dir(allUsers, { depth: null })
}

main()
  .catch((e) => console.error(e))
```
