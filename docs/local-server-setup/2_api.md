# API

## Overview

Our API is built using NestJS, a progressive Node.js framework that leverages TypeScript and follows a modular architecture. It is designed to be scalable, maintainable, and efficient for handling our recruitment system's backend operations.

## Database Configuration

To configure the database, copy the example configuration file `/api/src/db/database.config.example.ts` to the same directory:

For Linux/macOS:

```
cp api/src/db/database.config.example.ts api/src/db/database.config.ts
```

For Windows (Command Prompt):

```
copy api\src\db\database.config.example.ts api\src\db\database.config.ts
```

For Windows (PowerShell):

```
Copy-Item api\src\db\database.config.example.ts -Destination api\src\db\database.config.ts
```

Here is the database configuration file:

```
import { DataSourceOptions, DataSource } from 'typeorm';

export const DatasourceOptions = {
    type: 'mysql',
    host: 'localhost',
    port: 3306,
    username: 'root',
    password: 'admin',
    database: 'recruitment',
    synchronize: false,
} as DataSourceOptions;

export default new DataSource({
    ...DatasourceOptions,
    entities: ['src/**/*.entity.ts'],
    migrations: ['src/db/migrations/*.ts'],
    synchronize: false,
    migrationsRun: false,
});
```

#### Database Setup

Ensure the credentials match your MySQL setup. You must create the database manually using the MySQL CLI before running the API. Use the following commands:

```
mysql -u root -p
```

Enter your MySQL password when prompted.

```
CREATE DATABASE recruitment;
```

This will create the required database before running the API.

## App Configuration

The application requires an app configuration file to be set up before running the server. This configuration file contains various settings, including API prefixes, host details, authentication credentials, and third-party service keys.

#### 1. Copy the Example Configuration File

Navigate to the /api/src/ directory and copy the example configuration file:

```
# linux
cp /api/src/appConfig.example.ts /api/src/appConfig.ts

# windows
copy /api/src/appConfig.example.ts /api/src/appConfig.ts
```

#### 2. Configuration File Details

The appConfig.example.ts file looks like this:

```
const port = 3001;
const host = `http://localhost:${port}/api`;
const cotHost = 'http://localhost:5173/community';

export const appConfig = {
    prefix: 'api',
    host: host,
    port: port,

    websocketPath: undefined,

    uploadDir: './upload',
    pythonCli: 'python',
    imageMagickCli: 'magick',
    jwtSecret: '88e2b25495e0e3b7bc573d3bb682385005d7592d',

    google: {
        googleClientId: '',
        googleClientSecret: '',
        googleCallbackUrl: host + '',
        googleRedirectUrl: cotHost + '',
        geminiKey: '',
    },

    quickbase: {
        qbAuthorization: '',
        qbHost: '',
        qbRequisitionTable: '',
        qbJobDetailsTable: '',
        qbJobsRequestTable: '',
        qbApplicantTable: '',
        qbApplicantInfoTable: '',
    },

    referralForm: {
        url: '',
        authorization: {
            username: '',
            password: '',
        },
    },

    mail: {
        transport: {
            host: 'smtp.gmail.com',
            port: 465,
            secure: true,
            auth: {
                user: 'youremail@gmail.com',
                pass: '',
            },
        },
        defaults: {
            from: 'No reply <youremail@gmail.com>',
        },
    },
};
```

#### 3. Configure Required Settings

    - `port` – Defines the port for the API server.

    - `host` – The base URL for the API.

    - `cotHost` – The base URL for the community feature.

    - `jwtSecret` – A secret key used for authentication (replace with a secure value).

    - `google` – Google OAuth credentials (fill in if using Google authentication).

    - `quickbase` – API credentials for QuickBase integration (if applicable).

    - `referralForm` – Credentials for referral form authentication.

    - `mail` – SMTP settings for email notifications (replace with actual credentials).

#### 4. Ensure Proper Configuration

    - Update API keys, authentication credentials, and database settings to match your environment.

    - If running the server in production, make sure sensitive values like jwtSecret and SMTP credentials are not exposed in public repositories.

    - If using Google OAuth, populate the googleClientId, googleClientSecret, and other fields accordingly.

:::info
Google OAuth is no longer in use. It was previously implemented for applicant login, but its development has been put on hold.
:::

## Running the API

Once you're in the /api directory, follow these steps to get everything set up:

1. Run npm install to install all necessary dependencies.

2. Next, run the migrations with the following command:

```
npm run orm:run
```

This will apply the database migrations.

3. For more commands, you can refer to the package.json file. Some common commands are:

-   Create a migration:

```
npm run orm:create
```

-   Revert a migration

```
npm run orm:revert
```

-   Generate a migration:

```
npm run orm:generate
```

4. Build the project before running the CLI:

```
npm run build
```

:::warning
You need to run this command every time you create or update a CLI to ensure that you're running the latest code.
:::

5. Now, you can run the desired CLI command. For example, to populate required data, run:

```
npm run cli:run -- command=populate-required-data
```

6. For more details, you can refer to the api/src/cli.ts file.

7. Once everything is set up, run the development server:

```
npm run start:dev
```

:::info
This will run the server in watch mode, automatically rebuilding whenever a file is modified.
::::
