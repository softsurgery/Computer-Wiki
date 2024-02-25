Strapi projects can be installed either locally on a computer or on a remote server. The following installation guide provides step-by-step instructions on how to install and create a new Strapi project on your local machine.

# 📄️ CLI 
---

Strapi CLI (Command Line Interface) installation scripts are the fastest way to get Strapi running locally. The following guide is the installation option most recommended by Strapi.

## Preparing the installation

Before installing Strapi, the following requirements must be installed on your computer:
* Node.js: Only Active LTS or Maintenance LTS versions are supported (currently v18 and v20). Odd-number releases of Node, known as "current" versions of Node.js, are not supported (e.g. v19, v21).
* Your preferred Node.js package manager:
    npm (v6 and above)
    yarn
* Python (if using a SQLite database)

A supported database is also required for any Strapi project:
* MySQL
* MariaDB
* PostgreSQL
* SQLite

Strapi v4 does not support MongoDB.

## Creating a Strapi project

Follow the steps below to create a new Strapi project, being sure to use the appropriate command for your installed package manager:

In a terminal, run the following command:
```
yarn create strapi-app my-project
```

'yarn create' creates a new project
'strapi-app' is the Strapi package
'my-project' is the name of your Strapi project

Choose an installation type:
    * Quickstart (recommended), which uses the default database (SQLite)
    * Custom (manual settings), which allows to choose your preferred database

(Custom installation type only) Among the list of databases, choose a database for your Strapi project.
(Custom installation type only) Name your project's database.

## CLI installation options

The above installation guide only covers the basic installation option using the CLI. There are other options that can be used when creating a new Strapi project, for example:

    --quickstart: Directly create the project in quickstart mode.
    --template: Create a project with pre-made Strapi configurations (see Templates).
    --typescript/--ts: Create a project in TypeScript.
    --no-run: Prevent Strapi from automatically starting the server (useful in combination with --quickstart).

** Strapi also offers a starters CLI to create a project with a pre-made frontend application **

To start the Strapi application, run the following command in the project folder:

```
yarn develop
```

## 👀 Where is my content?

For self-hosted Strapi projects, all your content is saved in a database file (by default, SQLite) found in the .tmp subfolder in your project's folder. So anytime you start the Strapi application from the folder where you created your Strapi project, your content will be available (see database configuration for additional information).

If the content was added to a Strapi Cloud project, it is stored in the database managed with your Strapi Cloud project (see advanced database configuration for Strapi Cloud for additional information).

# 📄️ Docker
---

### ✋Caution
Strapi does not build any official container images. The following instructions are provided as a courtesy to the community. If you have any questions please reach out on Discord.

--- 

### ❗️Warning

Strapi applications are not meant to be connected to a pre-existing database, not created by a Strapi application, nor connected to a Strapi v3 database. The Strapi team will not support such attempts. Attempting to connect to an unsupported database may, and most likely will, result in lost data such as dropped tables.

---

## Introduction 
Docker is an open platform that allows developing, shipping, and running applications by using containers (i.e. packages containing all the parts an application needs to function, such as libraries and dependencies). Containers are isolated from each other and bundle their own software, libraries, and configuration files; they can communicate with each other through well-defined channels.

## Prerequisites
* Docker installed on your machine
* A supported version of Node.js
* An existing Strapi v4 project, or a new one created with the Quick Start guide
* (optional) Yarn installed on your machine
* (optional) Docker Compose installed on your machine

## Development and/or Staging environments
For working with Strapi locally on your host machine you can use the Dockerfile, and if needed the docker-compose.yml can also be used to start up a database container.

Both methods require an existing Strapi project or a new one created (see Quick Start guide).

### Development Dockerfile

The following Dockerfile can be used to build a non-production Docker image for a Strapi project.

--- 
✋ Note
If you are using docker-compose, you can skip setting the environment variables manually, as they will be set in the docker-compose.yml file or a .env file.

---

The following environment variables are required in order to run Strapi in a Docker container:
* NODE_ENV	The environment in which the application is running.
* DATABASE_CLIENT	The database client to use.
* DATABASE_HOST	The database host.
* DATABASE_PORT	The database port.
* DATABASE_NAME	The database name.
* DATABASE_USERNAME	The database username.
* DATABASE_PASSWORD	The database password.
* JWT_SECRET	The secret used to sign the JWT for the Users-Permissions plugin.
* ADMIN_JWT_SECRET	The secret used to sign the JWT for the Admin panel.
* APP_KEYS	The secret keys used to sign the session cookies.

You can also set some optional environment variables.


./Dockerfile :
```dockerfile
FROM node:18-alpine
# Installing libvips-dev for sharp Compatibility
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev nasm bash vips-dev git
ARG NODE_ENV=development
ENV NODE_ENV=${NODE_ENV}

WORKDIR /opt/
COPY package.json yarn.lock ./
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install
ENV PATH /opt/node_modules/.bin:$PATH

WORKDIR /opt/app
COPY . .
RUN chown -R node:node /opt/app
USER node
RUN ["yarn", "build"]
EXPOSE 1337
CMD ["yarn", "develop"]
```

### Docker Compose

The following docker-compose.yml can be used to start up a database container and a Strapi container along with a shared network for communication between the two.

Sample docker-compose.yml:

    MySQL
    MariaDB
    PostgreSQL

./docker-compose.yml :
```yaml
version: "3"
services:
  strapi:
    container_name: strapi
    build: .
    image: strapi:latest
    restart: unless-stopped
    env_file: .env
    environment:
      DATABASE_CLIENT: ${DATABASE_CLIENT}
      DATABASE_HOST: strapiDB
      DATABASE_PORT: ${DATABASE_PORT}
      DATABASE_NAME: ${DATABASE_NAME}
      DATABASE_USERNAME: ${DATABASE_USERNAME}
      DATABASE_PASSWORD: ${DATABASE_PASSWORD}
      JWT_SECRET: ${JWT_SECRET}
      ADMIN_JWT_SECRET: ${ADMIN_JWT_SECRET}
      APP_KEYS: ${APP_KEYS}
      NODE_ENV: ${NODE_ENV}
    volumes:
      - ./config:/opt/app/config
      - ./src:/opt/app/src
      - ./package.json:/opt/package.json
      - ./yarn.lock:/opt/yarn.lock
      - ./.env:/opt/app/.env
      - ./public/uploads:/opt/app/public/uploads
    ports:
      - "1337:1337"
    networks:
      - strapi
    depends_on:
      - strapiDB

  strapiDB:
    container_name: strapiDB
    platform: linux/amd64 #for platform error on Apple M1 chips
    restart: unless-stopped
    env_file: .env
    image: mysql:5.7
    command: --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_USER: ${DATABASE_USERNAME}
      MYSQL_ROOT_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_PASSWORD: ${DATABASE_PASSWORD}
      MYSQL_DATABASE: ${DATABASE_NAME}
    volumes:
      - strapi-data:/var/lib/mysql
      #- ./data:/var/lib/mysql # if you want to use a bind folder
    ports:
      - "3306:3306"
    networks:
      - strapi

volumes:
  strapi-data:

networks:
  strapi:
    name: Strapi
    driver: bridge
```

### .dockerignore

When building Docker images, it's essential to include only the files necessary for running the application inside the container. This is where the .dockerignore file comes into play. Similar to how .gitignore works for Git, specifying files and directories that should not be tracked or uploaded, .dockerignore tells Docker which files and directories to ignore when building an image.

Sample .dockerignore:

```
.tmp/
.cache/
.git/
.env
build/
node_modules/
# Ingoring folders that might be used in starter templates
data/
backup/
```

#### Why Use .dockerignore?

Including unnecessary files in a Docker build context can significantly slow down the build process. By excluding files and directories like .tmp, .cache, .git, build, node_modules, and .env through .dockerignore, the amount of data sent to the Docker daemon is reduced. This leads to faster build times since Docker has fewer files to process and include in the image.

##### Security

Excluding files and directories can also enhance the security of the Docker image. Sensitive files, such as .env, which might contain environment-specific secrets or credentials, should not be included in Docker images. This prevents accidental exposure of sensitive information.

##### Cleaner Images

A Docker image cluttered with unnecessary files can lead to potential confusion and issues, especially when the image is shared across teams or used in production. By keeping the image clean and focused only on the essentials, it becomes easier to maintain and troubleshoot.

##### Reduced Image Size

Smaller Docker images are more efficient to store, transfer, and launch. By excluding non-essential files, the final image size can be significantly reduced, leading to quicker pull and start times, especially in distributed and cloud environments.

##### Production Environments

The Docker image in production is different from the one used in development/staging environments because of the differences in the admin build process in addition to the command used to run the application. Typical production environments will use a reverse proxy to serve the application and the admin panel. The Docker image is built with the production build of the admin panel and the command used to run the application is strapi start.

Once the Dockerfile is created, the production container can be built. Optionally, the container can be published to a registry to make it available to the community. Community tools can help you in the process of building a production Docker image and deploying it to a production environment.

## Production Dockerfile

The following Dockerfile can be used to build a production Docker image for a Strapi project.

./Dockerfile.prod :

# Creating multi-stage build for production
```dockerfile
FROM node:18-alpine as build
RUN apk update && apk add --no-cache build-base gcc autoconf automake zlib-dev libpng-dev vips-dev git > /dev/null 2>&1
ENV NODE_ENV=production

WORKDIR /opt/
COPY package.json yarn.lock ./
RUN yarn global add node-gyp
RUN yarn config set network-timeout 600000 -g && yarn install --production
ENV PATH /opt/node_modules/.bin:$PATH
WORKDIR /opt/app
COPY . .
RUN yarn build
```

# Creating final production image
```dockerfile
FROM node:18-alpine
RUN apk add --no-cache vips-dev
ENV NODE_ENV=production
WORKDIR /opt/
COPY --from=build /opt/node_modules ./node_modules
WORKDIR /opt/app
COPY --from=build /opt/app ./
ENV PATH /opt/node_modules/.bin:$PATH

RUN chown -R node:node /opt/app
USER node
EXPOSE 1337
CMD ["yarn", "start"]
```

### Building the production container

Building production Docker images can have several options. The following example uses the docker build command to build a production Docker image for a Strapi project. However, it is recommended you review the Docker documentation for more information on building Docker images with more advanced options.

To build a production Docker image for a Strapi project, run the following command:

```
docker build \
  --build-arg NODE_ENV=production \
  # --build-arg STRAPI_URL=https://api.example.com \ # Uncomment to set the Strapi Server URL
  -t mystrapiapp:latest \ # Replace with your image name
  -f Dockerfile.prod .
```

---
## Publishing the container to a registry

After you have built a production Docker image for a Strapi project, you can publish the image to a Docker registry. Ideally for production usage this should be a private registry as your Docker image will contain sensitive information.

Depending on your hosting provider you may need to use a different command to publish your image. It is recommended you review the Docker documentation for more information on publishing Docker images with more advanced options.

Some popular hosting providers are:

* AWS ECR
* Azure Container Registry
* GCP Container Registry
* Digital Ocean Container Registry
* IBM Cloud Container Registry
* GitHub Container Registry
* Gitlab Container Registry
---
# Community tools

Several community tools are available to assist you in deploying Strapi to various cloud providers and setting up Docker in a development or production environment.
## @strapi-community/dockerize

The @strapi-community/dockerize package is a CLI tool that can be used to generate a Dockerfile and docker-compose.yml file for a Strapi project.

To get started run npx @strapi-community/dockerize@latest within an existing Strapi project folder and follow the CLI prompts.

## @strapi-community/deployify

The @strapi-community/deployify package is a CLI tool that can be used to deploy your application to various cloud providers and hosting services. Several of these also support deploying a Strapi project with a Docker container and will call on the @strapi-community/dockerize package to generate the required files if they don't already exist.

---
# Why doesn't Strapi provide official Docker images?

Strapi is a framework that can be used to build many different types of applications. As such, it is not possible to provide a single Docker image that can be used for all use cases.

# Why do we have different Dockerfiles for development and production?

The primary reason for various Docker images is due to the way our Admin panel is built. The Admin panel is built using React and is bundled into the Strapi application during the build process. This means that the Strapi backend is acting as a web server to serve the Admin panel and thus certain environment variables are statically compiled into the built Admin panel.

It is generally considered a best practice with Strapi to build different Docker images for development and production environments. This is because the development environment is not optimized for performance and is not intended to be exposed to the public internet.