[Medium](https://medium.com/@peturgeorgievv/how-to-setup-automatic-migrations-with-typeorm-on-deployment-2224544a4838)

Migrations are something everyone ignores until they decide to go to production. A common problem is how to add them to your automated deployment process. It really depends on your use case. There are a lot of ways.

Let’s start with a minor overview. You can use TypeORM with any of its supported types of databases (Postgres, MySQL, MongoDB, etc). To set it up, we need to create a [Data Source](https://typeorm.io/data-source). In general, we may need only one, but as I am doing different logic (seed and stuff), I prefer to have two sources, one for local and one for production. To start let’s first install TypeORM. We need typeorm package with reflect-metadata and pg (Postgres).

```bash
npm install typeorm reflect-metadata pg  
npm install @types/node --save-dev
```

Time to create our db folder. We are gonna need a few stuff as shown below. Let’s explain them one by one.

- Migrations folder will hold all of the migrations which we generate.
- dataSource — The source which we will use everywhere in our app
- dataSourceLocal — Data Source with local setup
- dataSourceProduction — Data Source with production (environment variables) setup

> Its possible to just use **one file** dataSource.ts and add all your environments inside, but for me this is clearer.

Our **dataSource.ts** file will include just a check for which environment data source file to use.

import DataSourceProd from "./dataSourceProd";  
import DataSourceLocal from "./dataSourceLocal";  
  
export default process.env.NODE_ENV === "production"  
  ? DataSourceProd  
  : DataSourceLocal;

For the local/production sources it’s a little bit more specific. Shown below is **dataSourceLocal.ts**

import { DataSource } from "typeorm";  
import { DataSourceOptions } from "typeorm/data-source/DataSourceOptions";  
  
let connectionOptions: DataSourceOptions = {  
  type: "postgres" as "postgres", // It could be mysql, mongo, etc  
  host: "localhost",  
  port: 5432,  
  username: "postgres", // postgre username  
  password: "root", // postgre password  
  database: "db-name", // postgre db, needs to be created before  
  synchronize: false, // if true, you don't really need migrations  
  logging: true,  
  entities: ["src/**/*.entity{.ts,.js}"], // where our entities reside  
  migrations: ["src/db/migrations/*{.ts,.js}"], // where our migrations reside  
};  
  
export default new DataSource({  
  ...connectionOptions,  
});

For production, we add everything from environment variables — **dataSourceProd.ts**

import { DataSource } from "typeorm";  
import { DataSourceOptions } from "typeorm/data-source/DataSourceOptions";  
  
let connectionOptions: DataSourceOptions = {  
  type: process.env.DB_TYPE as "postgres",  
  host: process.env.DB_HOST,  
  port: process.env.DB_PORT ? +process.env.DB_PORT : 5432, // Don't forget to cast to number with +  
  username: process.env.DB_USERNAME,  
  password: process.env.DB_PASSWORD,  
  database: process.env.DB_DATABASE,  
  synchronize: false,  
  logging: true,  
  entities: ["dist/**/*.entity{.ts,.js}"],  
  migrations: ["dist/db/migrations/*{.ts,.js}"],  
};  
  
export default new DataSource({  
  ...connectionOptions,  
});

In general, this data source is what TypeORM uses for everything, meaning that you get repositories with it. You can get data from any repository and use its methods. To be honest, if you need relations and complex search queries you are probably gonna use all the time **find,** as it’s rich in options, but we are not gonna talk about it in this article.

// Using getRepository you can get any repository you included in the   
// entities array in dataSource.  
const exercises: Exercise[] = await dataSource.getRepository(Exercise).find();

> Also don’t forget that when you start your express application, you need to do **dataSource.initialize()** so you can use it across the app.
> 
> Same is for **reflect-metadata,** dont forget to add it at the top of the index.ts file, where you start your express server.

// --- index.ts ---  
import "reflect-metadata";  
// Other imports and Express app below

Time to show and explain to you the scripts shown below that we are going to add to our **package.json** file. Building the app is crucial for our deployment, so **_npm run build_** (using tsc) will compile our TypeScript code to JavaScript. Next, let’s discuss all the typeorm scripts.

- **Generate** — used to create a migration when we modified anything on our entities. When we did, we just want to know where our directory is. In our case, its db/migrations, and the name of our migration is my-migration-name (**_it could be literally anything_**), so it’s as simple as -> **_npm run typeorm:generate db/migrations/my-migration-name_**
- **Migrate** — used to update our database if it’s not done automatically. Mainly the flow to follow is, you generate a migration and then simply run -> **_npm run typeorm:migrate_** and you are done. Your database is updated!
- **Revert** — Let’s say you made a mistake in the latest migration. Just run -> **_npm run typeorm:revert_** and your previous migrations should be reverted. An important note here, the file is **NOT** deleted, so you should **manually** delete that latest migration. So if you like to revert more than one migration, just run the command for reverting, delete the file, and repeat as many times as you like.
- **Drop** — as the command states, you just drop the database tables with the simple command **_npm run typeorm:drop_**

Beware that we are using our **dataSourceLocal.ts** file here, as we want to be specific about which environment data source we are using.

  "scripts": {  
    "build": "tsc",  
    "typeorm:generate": "npx typeorm-ts-node-esm migration:generate -d src/db/dataSourceLocal.ts",  
    "typeorm:migrate": "npx typeorm-ts-node-esm migration:run -d src/db/dataSourceLocal.ts",  
    "typeorm:revert": "npx typeorm-ts-node-esm migration:revert -d src/db/dataSourceLocal.ts",  
    "typeorm:drop": "npx typeorm-ts-node-esm schema:drop -d src/db/dataSourceLocal.ts",  
  },

We already know what our scripts are doing, we added them to **package.json** and we have our **data sources** and **migrations** set up.

We can host everywhere our application, but if you like to know how to do it in AWS EBS, you can find more about it here -> [**_How to Deploy Express App to AWS EBS_**](https://medium.com/@peturgeorgievv/how-to-deploy-express-app-to-aws-ebs-with-postgres-db-automate-deployment-with-codepipeline-cd818caf8aa4). Assuming we have everything ready, we just need a **Dockerfile** to specify the steps which are needed for our server to be built and how to be run on the cloud. Look at the below file for reference.

# --- Dockerfile ---  
  
FROM node:16 as development # we specify which node version we want  
  
WORKDIR /usr/src/app # directory to use  
  
COPY package*.json ./ # we copy both package-lock & package.json   
  
RUN npm ci --development # install deps  
  
COPY tsconfig*.json ./ # Copy tsconfig and src to workdir  
COPY src/ src/  
  
RUN npm run build # Build our app with tsc  
  
FROM node:16 as production # prod build step  
  
WORKDIR /app   
  
COPY package*.json ./  
  
RUN npm ci --production # don't install devDependecies  
  
COPY --from=development /usr/src/app/dist ./dist # copy from previous step  
  
# Exposing our port to public really depends on your implementation and hosting  
EXPOSE 8443  
  
# 1. Runs our migrations with the prod data source  
# 2. Runs our previously built project  
CMD npx typeorm migration:run -d dist/db/dataSourceProd.js && node dist/index.js

That’s it actually. It’s easy to do, just follow the right steps. I’m planning to write an article with the full setup of Express application with TypeORM.

For any questions **don’t hesitate** to ask in the comments section or in my **Twitter** account linked below.

> If you enjoyed reading and want to get notified for my next article, don’t forget to **Follow me** here in **Medium** and in **Twitter —** [https://twitter.com/peturgeorgievv](https://twitter.com/peturgeorgievv) Thank you!