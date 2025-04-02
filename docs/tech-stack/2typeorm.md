# TypeORM

TypeORM is an Object-Relational Mapping (ORM) library that helps developers interact with databases using TypeScript or JavaScript. It supports various databases like MySQL, PostgreSQL, SQLite, and MongoDB. In this guide, we will integrate TypeORM with NestJS.

## Installation

To use TypeORM in NestJS, install the required dependencies:

```
npm install @nestjs/typeorm typeorm mysql2
```

Replace mysql2 with the database driver for your preferred database (e.g., pg for PostgreSQL, sqlite3 for SQLite, etc.).

## Configuration

Modify the app.module.ts file to configure TypeORM:

```
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from './users/user.entity';
import { UsersModule } from './users/users.module';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'mysql',
      host: 'localhost',
      port: 3306,
      username: 'root',
      password: 'password',
      database: 'test_db',
      entities: [User],
      synchronize: true, // Set to false in production
    }),
    UsersModule,
  ],
})
export class AppModule {}
```

## Creating an Entity

Entities define database table structures. Create a user.entity.ts file:

```
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @Column({ unique: true })
  email: string;
}
```

## Creating a Repository

A repository allows interaction with the database. Update users.module.ts:

```
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from './user.entity';
import { UsersService } from './users.service';
import { UsersController } from './users.controller';

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

## Using the Repository in a Service

Create users.service.ts to handle database operations:

```
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './user.entity';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private usersRepository: Repository<User>,
  ) {}

  async createUser(name: string, email: string): Promise<User> {
    const user = this.usersRepository.create({ name, email });
    return this.usersRepository.save(user);
  }

  async findAll(): Promise<User[]> {
    return this.usersRepository.find();
  }
}
```

## Creating a Controller 

Create users.controller.ts to expose API routes:

```
import { Controller, Get, Post, Body } from '@nestjs/common';
import { UsersService } from './users.service';
import { User } from './user.entity';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Post()
  create(@Body() body: { name: string; email: string }): Promise<User> {
    return this.usersService.createUser(body.name, body.email);
  }

  @Get()
  findAll(): Promise<User[]> {
    return this.usersService.findAll();
  }
}
```

## Database Migrations

To track schema changes, use migrations instead of synchronize: true. First, install the CLI tool:

```
npm install -g typeorm
```

Modify ormconfig.json:

```
{
  "type": "mysql",
  "host": "localhost",
  "port": 3306,
  "username": "root",
  "password": "password",
  "database": "test_db",
  "entities": ["dist/**/*.entity.js"],
  "migrations": ["dist/migrations/*.js"],
  "cli": {
    "migrationsDir": "src/migrations"
  }
}
```

## Generating Migrations

To generate a new migration, run the following command:

```
typeorm migration:generate -n MigrationName
```

This will create a new migration file inside the src/migrations directory. To apply the migration, run:

```
typeorm migration:run
```

To revert the last migration, run:

```
typeorm migration:revert
```

## Conclusion

Integrating TypeORM with NestJS provides a structured approach to handling database operations. This guide covered setting up TypeORM, creating entities, repositories, services, controllers, handling database migrations, and generating migrations. Keep exploring to enhance your NestJS applications!