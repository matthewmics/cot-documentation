# MySQL

## Overview

MySQL is installed on our EC2 server and is used for managing our databases for both production and testing environments.

    - Production Database: `prod_recruitment`

    - Test Database: `experimental`


## Checking MySQL Version

To confirm the installed version of MySQL, run:

```
mysql --version
```

Output:

```
mysql  Ver 8.0.41-0ubuntu0.20.04.1
```

## Accessing MySQL

To log in to MySQL as the root user, run:

```
mysql -u root -p

```

Enter the password when prompted:

```
password: <password>
```

## Basic MySQL Commands

1. Show all databases

```
SHOW DATABASES;
```

2. Select a database to use

```
USE prod_recruitment;
```

or 

```
USE experimental;
```

3. Show all tables in the selected database

```
show tables
```

## MySQL Backup (mysqldump)

To create a backup of a specific database, use mysqldump:

### 1. Backup Production Database (prod_recruitment)

```
mysqldump -u root -p prod_recruitment > prod_recruitment_backup.sql
```

### 2. Backup Test Database (experimental)

```
mysqldump -u root -p experimental > experimental_backup.sql
```

### 3. Backup All Databases

```
mysqldump -u root -p --all-databases > all_databases_backup.sql
```

### 4. Restore a Database from a Backup

```
mysql -u root -p prod_recruitment < prod_recruitment_backup.sql
```

or 

```
mysql -u root -p experimental < experimental_backup.sql
```

## Conclusion

We have set up MySQL 8.0.41 on our EC2 server to manage our databases efficiently. The production database (`prod_recruitment`) and the test database (`experimental`) are accessible using basic MySQL commands.

For database management:

    - We can list databases, select a database, and view tables using MySQL queries.

    - Backup and restore operations can be performed using `mysqldump`.

    - Secure access is ensured with password authentication.

This documentation serves as a quick reference for managing MySQL on our EC2 server. Let me know if any additional details are needed! 🚀