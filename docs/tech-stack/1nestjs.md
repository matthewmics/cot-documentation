# NestJS

NestJS is a powerful framework for building efficient, scalable Node.js server-side applications. It is built with TypeScript and provides an out-of-the-box application architecture that helps to create maintainable applications.

## Introduction

NestJS is a progressive Node.js framework for building server-side applications. It uses TypeScript by default, and it is heavily inspired by Angular, so if you are familiar with Angular, you will feel right at home.

## Installation

To get started with NestJS, you need to have Node.js installed. If you haven't installed it yet, you can download it from [nodejs.org](https://nodejs.org/).

Once Node.js is installed, you can install the NestJS CLI globally:

```bash
npm install -g @nestjs/cli
```

## Create a New Project

Once the CLI is installed, you can create a new NestJS project:

```
nest new project-name
```

Replace project-name with the name you want for your project. The CLI will prompt you to select the package manager you prefer (npm or yarn). Choose one and continue.

## Install Dependencies

After creating the project, navigate to your project directory:

```
cd project-name
```

Now, install the dependencies using the package manager (npm or yarn). If you chose npm:

```
npm install
```

## Run the Application

To start your application, you can use the following command:

```
npm run start:dev
```

This will start the application on http://localhost:3000/ by default.

You should see something like the following in the terminal:

```
[Nest] 12345   - 2025-04-01 11:00:00   [NestFactory] Starting Nest application...
[Nest] 12345   - 2025-04-01 11:00:00   [InstanceLoader] AppModule dependencies initialized +10ms
[Nest] 12345   - 2025-04-01 11:00:00   [NestApplication] Nest application is successfully started +10ms
```

Now, open your browser and navigate to http://localhost:3000/. You should see a message like:

```
"Hello World!"
```

This means your NestJS application is running successfully.

## Core Concepts

NestJS follows a modular architecture, where the application is divided into modules. Below are some key concepts in NestJS.

### Modules

Modules are the fundamental building blocks in NestJS. Each module is a class annotated with the @Module() decorator. A module organizes related components such as controllers, services, and providers.

```
import { Module } from '@nestjs/common';

@Module({
  imports: [],
  controllers: [],
  providers: [],
})
export class AppModule {}
```

### Controllers

Controllers handle incoming requests and return responses to the client. They are responsible for defining the application's routes.

```
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll() {
    return 'This action returns all cats';
  }
}
```

### Providers

Providers are used to handle business logic. They can be services, repositories, or any other class that can be injected into other components via dependency injection.

```
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  getHello(): string {
    return 'Hello from Cats Service';
  }
}
```

### Services

Services are providers that hold the business logic. They can be injected into controllers to share common functionality.

```
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  findAll() {
    return ['Maine Coon', 'Persian', 'Siamese'];
  }
}
```

### Pipes

Pipes are used to transform input data and validate it. They are often used for validating request parameters, body, or query parameters.

```
import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ParseIntPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return parseInt(value, 10);
  }
}
```

### Middleware

Middleware functions are executed before the route handler. They can be used to modify the request object, perform logging, or check authentication.

```
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

### Interceptors

Interceptors are used to modify the behavior of method calls, like transforming the result returned by a handler or adding additional logic.

```
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');

    return next
      .handle()
      .pipe(
        tap(() => console.log('After...'))
      );
  }
}
```

## Creating a Simple Application

After installing NestJS and setting up your first project, let's create a simple application with a single route.

1. After installing NestJS and setting up your first project, let's create a simple application with a single route.

```
nest generate controller cats
```

2. Add a route inside cats.controller.ts:

```
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll() {
    return ['Maine Coon', 'Persian', 'Siamese'];
  }
}
```

3. Run the application

```
npm run start
```

Now, if you visit http://localhost:3000/cats, you should see a list of cat breeds returned by the server.

## Conclusion

NestJS is an excellent framework for building efficient, scalable server-side applications. This documentation covers the basics of setting up a NestJS project, including core concepts like modules, controllers, providers, services, and other important features. Keep exploring NestJS to leverage its full potential!

```
This file now includes the full `Installation` section, along with other sections for NestJS basics.
```
