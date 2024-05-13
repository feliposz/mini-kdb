---
title: PostgreSQL
---

## Links

[Documentation](https://www.postgresql.org/docs/10/index.html)

## Features

- ACID compliant
- Data types: numeric, string, boolean, datetime, arrays, json, - key-value, etc
- Stored procedures: PL/PGSQL, Python, Perl, etc
- Data Integrity: PK, FK, UK, NN, etc
- Fault tolerant
- Full Text Search
- ...

## Getting started

Test new PG installation on linux:

```
sudo -u postgres psql -c "select ':-D' ok"
```

Create user and database for local usage:

```
sudo -u postgres createuser -s $(whoami)
sudo -u postgres createdb $(whoami)
```

Connect as admin user:

```
psql -U postgres
```

Create databases:

```
CREATE DATABASE database_name;
```


Connect as regular user to own database:

```
psql
```

## psql console commands

Command | Description
---|---
`\l` | List databases
`\c NAME` | Connect to database
`\conninfo` | Connection information
`\dt` | List tables
`\d NAME` | Describe table, etc
`\p` | Show current query buffer
`\r` | Clear current query buffer
`\q` | Quit `psql`


Load a CSV file into a table:

```
copy TABLENAME from '/home/user/SOURCE.csv' delimiter ',' csv header;
```

Copy a table structure but not its rows:

```
create table salaries2 as select * from salaries where false;
```


## Dump

Dump database script:

```
pg_dump --format=p --create --host=HOSTNAME --port=5432 --schema-only --file=DBNAME.dump --username=postgres DBNAME
```

## Command line interface

Set environment variables from project config:

```
. config/development.env
```

Connect to DB using env vars:

```
psql -h $DB_HOST -U $DB_USER -d $DB_DATABASE -p $DB_PORT
```

Restore a dump (with create database):

```
psql -h 192.168.99.100 -p 5432 -U postgres < ./db/15-02-19.dump
```

Check config file location:

```
psql -U postgres -c 'SHOW config_file'
```



## Queries

### Date functions

```
SELECT date_trunc('day', 'now'::timestamp)
```

### Generate a series dynamically

Numbers:

```
SELECT hour_series FROM generate_series(0, 23) hour_series
```

Step:

```
SELECT odd_numbers FROM generate_series(1, 100, 2) odd_numbers
```

Timestamp series:

```
SELECT period_series
FROM generate_series('2019-02-11 05:00:00'::timestamp, 
                     '2019-02-12 04:59:59'::timestamp, 
                     '1 hour'::interval) period_series
```


## Postgress on docker

Prerequisites:

- Install docker

Pull image:

`docker pull postgres`

Start container:

`docker run --name some-postgres -p 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword -d postgres`

Parameters:
```
--name CONTAINER_NAME
-p GUEST_PORT:HOST_PORT
-e ENV_VAR=VALUE
-d    (run detached)
```

Run shell on container:

`docker exec -it some-postgres /bin/bash`

Parameters:
```
-it interactive terminal
```

> Note: Use `//bin//bash` or just `bash` when running from Git Bash on Windows

Connect to db as admin:

`runuser -l postgres -c psql`

Create user:

```
create database hellodb;
create user hellouser with password 'hellouser';
grant all privileges on database hellodb to hellouser;
```

Connect to db as user:

`psql postgres://hellouser:hellouser@localhost/hellodb`

Create tables:

```
create table hellotable (name text);
insert into hellotable values ('Hello World');
```

## Running postgres from binaries (portable)

Download binaries from `https://www.enterprisedb.com/download-postgresql-binaries`

Unzip on a directory, like: `C:\Local\Portable\pgsql\` and CD into this dir.

```
cd C:\Local\Portable\pgsql\
```

Create two new subdirs: 

```
mkdir data
mkdir logs
```

Add binaries to PATH:

```
setx PATH "%PATH%;C:\Local\Portable\pgsql\bin"
```

Initialize DB data with:

```
initdb -U postgres -A password -E utf8 -W -D C:\Local\Portable\pgsql\data
```

Start the server cluster with:

```
pg_ctl -D "C:\Local\Portable\pgsql\data" -l "C:\Local\Portable\pgsql\logs\pgsql.log" start
```

Stop the server cluster with:

```
pg_ctl -D "C:\Local\Portable\pgsql\data" -l "C:\Local\Portable\pgsql\logs\pgsql.log" stop
```

Connect locally:

```
psql -U postgres
```

Create a user and database:

```
psql -v ON_ERROR_STOP=1 -U postgres <<-EOSQL
     CREATE USER myuser WITH PASSWORD 'mypass';
     CREATE DATABASE "mydb";
     GRANT ALL PRIVILEGES ON DATABASE "mydb" TO myuser;     
EOSQL
```

## Export/import a table with CSV

The command `copy` works with files on the *server*, while `\copy` works with files on the *client*.

Export from original DB to CSV using a custom query:

~~~sql
\copy (select id, "storeId", "tabletId", description, "mealId", "isCombo", "isOption", "quantity", "basePrice", "composedPrice", "totalPrice", "orderedAt", "startedAt", "finishedAt", "deliveredAt", "canceledAt", "tabId", "orderId" from app.sales where "storeId" = 80 and "tabId" is not null and "orderId" is not null) to 'C:\temp\data-sales.csv' with (format 'csv', header true);
~~~

Import into target DB from CSV:

~~~sql
\copy app.sales (id, store_id, tablet_id, meal_description, meal_id, is_combo, is_option, quantity, base_price, composed_price, total_price, ordered_at, started_at, finished_at, delivered_at, canceled_at, tab_id, order_id) from 'C:\temp\data-sales.csv' with (format 'csv', header true);
~~~


## JSON/JSONB

Insert data from one table as a JSONB array in another table.

~~~sql
create table test_json (data jsonb);
insert into test_json values ((select to_jsonb(array_agg(p.*)) from "Person" p));
~~~

Extract data from a JSONB column as a recordset.

~~~sql
select x.id, x.name, x."createdAt", x."updatedAt"
from jsonb_to_recordset((
    select data
    from test_json 
    where data @> '[{"id":1}]'
)) as x (id int, name text, "createdAt" timestamp, "updatedAt" timestamp);
~~~


## Problems

### Don't know the name for login user [source](https://stackoverflow.com/questions/60556957/how-to-check-the-username-of-postgresql-on-windows-10#60559035)

Start PostgreSQL in single-user mode:

```
C:\Local\Portable\pgsql\bin\postgres.exe --single -D C:\Local\Portable\pgsql\pgsql\data postgres
```

List users that can login:

~~~sql
SELECT * FROM pg_authid WHERE rolcanlogin;
~~~
