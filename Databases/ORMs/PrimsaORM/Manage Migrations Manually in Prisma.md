#### **1. Generate an Empty Migration**

Prisma allows you to create an empty migration folder where you can write your own SQL.

```sh
prisma migrate dev --create-only --name custom_migration
```

This command:

- Creates a new migration folder inside `prisma/migrations/`.
- Does **not** apply the migration automatically.
- Lets you manually edit the SQL before applying it.

#### **2. Write Your Own SQL**

Navigate to the newly created migration folder:

```sh
cd prisma/migrations/{timestamp}_custom_migration
```

Inside, you'll find a `migration.sql` file. Open it and write your custom SQL.

#### **3. Apply the Migration**

Once you've written your SQL, apply it using:

```sh
prisma migrate deploy
```

This will execute all pending SQL migrations on the database.

#### **4. Keep Prisma Schema in Sync**

After a manual migration, update your Prisma schema manually if necessary. Then, update the Prisma Client:

```sh
prisma generate
```