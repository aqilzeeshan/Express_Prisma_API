# How To Build a REST API with Express, Prisma and PostgreSQL

[This](https://www.digitalocean.com/community/tutorials/how-to-build-a-rest-api-with-prisma-and-postgresql) tutorial was followed.

1. Create directory `Express_Prisma_API` and initialze npm and install prisma and initialize it.
```
npm init -y
npm install -D prisma
npx prisma init
```

2. Create PostgreSql db in `https://railway.app/` and get connection info. It also created a .env file, in case you didn’t have one already, with the DATABASE_URL environment variable. Update it with connection info of db.

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
