If the Strapi project was created with the starter CLI, its structure includes both a frontend and backend folder, where the backend folder has the default structure.
Structure of a project created with the starter CLI

The default structure of a Strapi project created without the starter CLI depends on whether the project was created with vanilla JavaScript or with TypeScript, and looks like the following:


The following diagram is interactive: you can click on any file or folder name highlighted in purple to go to the corresponding documentation page.

# JavaScript-based projects

```
. # root of the application
├──── .strapi # auto-generated folder — do not update manually
│     └──── client # files used by bundlers to render the application
│           ├ index.html 
│           └ app.js 
├──── .tmp
├──── build # build of the admin panel
├──── config # API configurations
│     ├ api.js
│     ├ admin.js
│     ├ cron-tasks.js
│     ├ database.js
│     ├ middlewares.js
│     ├ plugins.js
│     └ server.js
├──── database
│     └──── migrations
├──── node_modules # npm packages used by the project
├──── public # files accessible to the outside world
│     └──── uploads
├──── src
│     ├──── admin # admin customization files
│           ├──── extensions # files to extend the admin panel
│     │     ├ app.js
│     │     └ webpack.config.js
│     ├──── api # business logic of the project split into subfolders per API
│     │     └──── (api-name)
│     │           ├──── content-types
│     │           │     └──── (content-type-name)
│     │           │           └ lifecycles.js
│     │           │           └ schema.json
│     │           ├──── controllers
│     │           ├──── middlewares
│     │           ├──── policies
│     │           ├──── routes
│     │           ├──── services
│     │           └ index.js
│     ├──── components
│     │     └──── (category-name)
│     │           ├ (componentA).json
│     │           └ (componentB).json
│     ├──── extensions # files to extend installed plugins
│     │     └──── (plugin-to-be-extended)
│     │           ├──── content-types
│     │           │     └──── (content-type-name)
│     │           │           └ schema.json
│     │           └ strapi-server.js
│     ├──── middlewares
│     │     └──── (middleware-name).js
│     ├──── plugins # local plugins files
│     │     └──── (plugin-name)
│     │           ├──── admin
│     │           │     └──── src
│     │           │           └ index.js
│     │           ├──── server
│     │           │     ├──── content-types
│     │           │     ├──── controllers
│     │           │     └──── policies
│     │           ├ package.json
│     │           ├ strapi-admin.js
│     │           └ strapi-server.js
│     ├─── policies
│     └ index.js # include register(), bootstrap() and destroy() functions
├ .env
└ package.json
```

TypeScript-based projects

```
. # root of the application
├──── .strapi # auto-generated folder — do not update manually
│     └──── client # files used by bundlers to render the application
│           ├ index.html 
│           └ app.js 
├──── .tmp
├──── config # API configurations
│     ├ api.ts
│     ├ admin.ts
│     ├ cron-tasks.ts
│     ├ database.ts
│     ├ middlewares.ts
│     ├ plugins.ts
│     └ server.ts
├──── database
│     └──── migrations
├──── dist # build of the backend
│     └──── build # build of the admin panel
├──── node_modules # npm packages used by the project
├──── public # files accessible to the outside world
│     └──── uploads
├──── src
│     ├──── admin # admin customization files
│     │     ├──── extensions # files to extend the admin panel
│     │     ├ app.example.tsx
│     │     ├ webpack.config.ts
|     |     └ tsconfig.json
│     ├──── api # business logic of the project split into subfolders per API
│     │     └──── (api-name)
│     │          content-types
│     │           │     └──── (content-type-name)
│     │           │           └ lifecycles.ts
│     │           │           └ schema.json
│     │           ├──── controllers
│     │           ├──── middlewares
│     │           ├──── policies
│     │           ├──── routes
│     │           ├──── services
│     │           └ index.ts
│     │ │     ├──── components
│     │     └──── (category-name)
│     │           ├ (componentA).json
│     │           └ (componentB).json
│     ├──── extensions # files to extend installed plugins
│     │     └──── (plugin-to-be-extended)
│     │           ├──── content-types
│     │           │     └──── (content-type-name)
│     │           │           └ schema.json
│     │           └ strapi-server.js
│     ├──── middlewares
│     │     └──── (middleware-name)
│     │           ├ defaults.json
│     │           └ index.ts
│     ├──── plugins # local plugins files
│     │     └──── (plugin-name)
│     │           ├──── admin
│     │           │     └──── src
│     │           │           └ index.tsx
│     │           │           └ pluginId.ts
│     │           ├──── server
│     │           │     ├──── content-types
│     │           │     ├──── controllers
│     │           │     └──── policies
│     │           ├ package.json
│     │           ├ strapi-admin.js
│     │           └ strapi-server.js
│     ├─── policies
│     └ index.ts # include register(), bootstrap() and destroy() functions
├ .env
├ tsconfig.json
└ package.json
```