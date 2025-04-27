# API (Docker)

## Overview

This guide provides instructions on setting up and running the NestJS application in a Dockerized environment. The application uses MySQL as a database and includes a few essential setup steps that must be completed before starting the containers.

By following these instructions, you will be able to:

-   Set up the environment for running the application in Docker containers.

-   Ensure proper configuration for database connection and app settings.

-   Use Docker Compose to manage the application, including the app container and database container.

## Prerequisites

Before you proceed, ensure that the following software is installed on your machine:

-   **Docker**: The application uses Docker for containerization. [Install Docker](https://docs.docker.com/get-docker/)

-   **Docker Compose**: To manage multi-container setups. [Install Docker Compose](https://docs.docker.com/compose/install/)

-   **Node.js**: Ensure that Node.js is available for building the application. [Install Node.js](https://nodejs.org/)

## Initial Setup: Copy Configuration Files

Before running the Docker containers, you need to copy some configuration files to set up the application. Follow these steps:

1. Copy appConfig.example.ts to appConfig.ts
    ```
    cp src/appConfig.example.ts src/appConfig.ts
    ```
2. Copy database.config.example.ts to database.config.ts
    ```
    cp src/db/database.config.example.ts src/db/database.config.ts
    ```
    After copying these files, you can proceed with starting the application using Docker.

## Running Docker

To run the application using Docker, ensure that you're in the **API directory** where the `docker-compose.yml` file is located.

### Step 1: Build the Docker Containers

Run the following command to build the Docker containers:

```bash
docker compose build
```

### Step 2: Start the Docker Containers

Once the build is complete, run the following command to start the containers:

```
docker compose up
```

:::info
When run for the first time, this command will execute database migrations and run the CLI to populate the database.
:::

:::note
You can verify that the API is running by visiting [http://localhost:3000/api](http://localhost:3000/api)
:::

To check the status of the containers, you can use:

```
docker compose ps
```

### Step 3: Check Application Logs

If you want to see the logs of the running application, you can use:

```
docker compose logs -f
```

### Step 4: Stopping the Containers

To stop the containers, run the following command:

```
docker compose down
```

This will stop and remove the containers. If you want to remove the containers along with the volumes (to clear any persistent data), you can use:

```
docker compose down --volumes
```

## Accessing the Application and Database via Docker

Once the containers are running, you can access both the application and the MySQL database through Docker commands.

### Step 1: Access the Application Container

To access the running application container, use the following command:

```bash
docker exec -it nestjs-dev sh
```

:::note
This is where you can run CLI commands, create or run migrations, and install new packages. (It's recommended to run npm install here instead of rebuilding the container, as rebuilding can be time-consuming.)
:::

### Step 2: Access the Database Container

To access the MySQL database container, use the following command:

```
docker exec -it mysql8-dev sh
mysql -u root -padmin
```

or

```
mysql -u root -padmin -P3307
```

## Conclusion

In this guide, we've outlined the steps required to set up, run, and interact with the Dockerized NestJS application and MySQL database. With Docker Compose, we can easily manage the multi-container setup, ensuring a smooth development environment.

### Key Takeaways:

-   **Docker Setup:** We've used Docker and Docker Compose to containerize the application, making it portable and easy to run across different environments.
-   **Accessing Containers:** With `docker exec`, you can easily access the application container to run commands or check logs, and also access the database container for running SQL queries.
-   **Efficient Development:** By using Docker Compose commands like `docker compose build` and `docker compose up`, you can quickly spin up the development environment, allowing you to focus on development without worrying about environment setup.
