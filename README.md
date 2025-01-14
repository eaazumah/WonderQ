# WonderQ

WonderQ is a broker that allows producers to write to it, and consumers to read from it. It runs on a single server and has a single message queue. Whenever a producer writes to WonderQ, a message ID is generated and returned as confirmation. Whenever a consumer polls WonderQ for new messages, it can get those messages. These messages should NOT be available for processing by any other consumer that may be concurrently accessing WonderQ.

A few things to note in the project:

- **[Github Actions Workflows](https://github.com/sidhantpanda/docker-express-typescript-boilerplate/tree/master/.github/workflows)** - Pre-configured Github Actions to run automated builds and publish image to Github Packages
- **[Dockerfile](https://github.com/sidhantpanda/docker-express-typescript-boilerplate/blob/master/Dockerfile)** - Dockerfile to generate docker builds.
- **[docker-compose](https://github.com/sidhantpanda/docker-express-typescript-boilerplate/blob/master/docker-compose.yml)** - Docker compose script to start service in production mode.
- **[OpenAPI 3.0 Spec](https://github.com/sidhantpanda/docker-express-typescript-boilerplate/blob/master/openapi.json)** - A starter template to get started with API documentation using OpenAPI 3.0. This API spec is also available when running the development server at `http://localhost:3000/dev/api-docs`
- **[.env file for configuration](#environment)** - Change server config like app port, mongo url etc
- **[Winston Logger](#logging)** - Uses winston as the logger for the application.
- **ESLINT** - ESLINT is configured for linting.
- **Mocha** - Using Mocha for running test cases

## I. Installation

#### 1. Clone this repo

```
$ git clone git@github.com:eaazumah/wonder-q.git
$ cd wonder-q
```

#### 2. Install dependencies

if yarn is not install

```
$ npm install --global yarn
```

```
$ yarn install
```

## II. Configuration

#### Environment

Copy the content of .env.default to a .env file

```
PORT=3000
NODE_ENV=development
```

## III. Development

### Start dev server

Start the dev server using the compose script at `docker-compose.dev.yml`.

```
$ docker-compose -f docker-compose.dev.yml up --build
```

Running the above commands results in

- 🌏**API Server** running at `http://localhost:3000`
- ⚙️**Swagger UI** at `http://localhost:3000/dev/api-docs`
- 🌏**GRAPH QL Server** running at `http://localhost:3000/graphql`

## IV. Packaging and Deployment

#### 1. Build and run without Docker

```
$ yarn build && yarn start
```

#### 2. Run with docker

```
$ docker build -t wonder-q .
$ docker run -t -i \
      --env NODE_ENV=production \
      -p 3000:3000 \
      wonder-q
```

#### 3. Run with docker-compose

```
$ docker-compose up
```

---

## Environment

To edit environment variables, create a file with name `.env` and copy the contents from `.env.default` to start with.

| Var Name | Type   | Default       | Description                            |
| -------- | ------ | ------------- | -------------------------------------- |
| NODE_ENV | string | `development` | API runtime environment. eg: `staging` |
| PORT     | number | `3000`        | Port to run the API server on          |

## Logging

The application uses [winston](https://github.com/winstonjs/winston) as the default logger. The configuration file is at `src/utils/logger.ts`.

- All logs are saved in `./logs` directory and at `/logs` in the docker container.
- The `docker-compose` file has a volume attached to container to expose host directory to the container for writing logs.
- Console messages are prettified
- Each line in error log file is a stringified JSON.

### Directory Structure

```
+-- .github
|   +-- workflows
|   |   +-- develop.yml
+-- @types
|   +-- deceleration.ts
|   +-- schema.ts
+-- src
|   +-- controllers
|   |   +-- controllers.consumer.ts
|   |   +-- controllers.producer.ts
|   +-- grahpql
|   |   +-- index.ts
|   |   +-- resolvers.ts
|   |   +-- types.ts
|   +-- routes
|   |   +-- index.ts
|   |   +-- routes.consumer.ts
|   |   +-- routes.producer.ts
|   +-- server
|   |   +-- apollo.server.ts
|   |   +-- create.express.app.ts
|   |   +-- subscription.server.ts
|   +-- services
|   |   +-- services.queue.ts
|   +-- utils
|   |   +-- log-response-time.ts
|   |   +-- logger.ts
|   +-- app.ts
+-- .env.default
+-- .eslintrc.json
+-- .gitignore
+-- .mocharc.json
+-- codegen.yml
+-- commitlint.config.cjs
+-- docker-compose.dev.yml
+-- docker-compose.yml
+-- Dockerfile
+-- LICENSE
+-- nodemon.json
+-- openapi.json
+-- package.json
+-- README.md
+-- tsconfig.build.json
+-- tsconfig.json
+-- yarn.json
```

## III. Contribution

### Git Workflow

Contribution to this project must follow the
**[GitFlow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow#:~:text=Gitflow%20is%20a%20legacy%20Git,software%20development%20and%20DevOps%20practices.)**
workflow

### Commits

Commits messages must follow **[Conventional Commits Spec](https://www.conventionalcommits.org/en/v1.0.0/)**

Use yarn commit to commit messages

```
$ yarn commit
