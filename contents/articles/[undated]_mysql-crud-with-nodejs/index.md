---
title: MySQL CRUD With NodeJS
author: sad301
date: 2015-09-03
template: article.jade
---

Have you ever wanted to access [MySQL][2] database from [NodeJS][1] app ? let's talk about it here. CRUD stands for Create, Retrieve, Update, and Delete. They were basic activities we usually do in MySQL database. Like _creating_ new record using `INSERT` command, _retrieving_ records using `SELECT`, _updating_ record using `UPDATE`, or _deleting_ record using `DELETE`.

Before we start, we need to create a table for our example. Write these lines in an `.sql` file, let's name it `contact.sql`.

```sql
drop database if exists test;
create database test;
use test;

create table contact (
	id int not null auto_increment,
	name varchar(32) not null,
	address text,
	phone varchar(15),
	email varchar(15),
	website varchar(15),
	primary key (id)
);
```

And then, import the `.sql` file into MySQL using this command :

```
mysql -u [your username] -p < [sql file].sql
```

Before we begin coding our app, we need to install `mysql` package using the `npm` tool. For the reference you might want to visit the [github page][3].

```
npm install mysql
```

In your `.js` file, let's name it `mysqlApp.js`, declare to import `mysql` :

```javascript
var mysql = require('mysql');
```

And then, we need to create a connection to the database using `createConnection()` method. This method will require several options

## Create Record

Now, let's do stuff. First, we want to create a new record in the `contact` table, using MySQL `INSERT` command. Looking at the table structure, i bet you already know the SQL command :

```sql
INSERT INTO `contact`(`name`) VALUES ('John');
```

[1]: http://nodejs.org "NodeJS"
[2]: http://www.mysql.com "MySQL"
[3]: https://github.com/felixge/node-mysql "node-mysql"