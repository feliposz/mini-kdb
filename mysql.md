---
title: MySQL
---

# Service

Restart service (on Ubuntu):

```
sudo /etc/init.d/mysql restart
```

# Dump

Restore an entire dump:

```
mysql -u root -proot DATABASE_NAME < DATABASE_NAME.dump
```

Extract single table from dump script:

```
sed -n -e '/DROP TABLE.*`example_table`/,/UNLOCK TABLES/p' original_dump_file.sql > example_table.sql
```

Restore single table:

```
mysql -u root -proot DATABASE_NAME < example_table.sql
```

Fix missing rows (in child?) with some dummy data from another table (parent?):

~~~sql
insert into example_table
    (parent_id, some_id, name, status, created_at, updated_at, changed_at)
select id, 1, 'Temp', 'Published', created_at, updated_at, updated_at
from example_parent
where status = 'Published'
and created_at > '2019-02-01'
and id not in ( select parent_id from example_table );
~~~

# Enable remote connection

Example: for accessing a DB running in Vagrant.

Enable remote connection for a given IP:

~~~sql
grant all on *.* to root@'192.168.33.1' identified by 'root';
~~~

In file `/etc/mysql/my.cnf` comment localhost and bind to all interfaces:

```
#bind-address=127.0.0.1
bind-address=0.0.0.0
```

# Analysis

Check indexes with low cardinality:

~~~sql
SELECT
    i.database_name,
    i.table_name,
    i.index_name,
    s.cardinality,
    ROUND(i.stat_value * @@innodb_page_size / 1024 / 1024, 2) size_in_mb,
    t.table_rows,
    i.last_update
FROM mysql.innodb_index_stats i
JOIN information_schema.statistics s
    ON i.database_name = s.table_schema
    AND i.table_name = s.table_name
    AND i.index_name = s.index_name
JOIN information_schema.tables t
	ON t.table_schema = s.table_schema
    AND t.table_name = s.table_name
WHERE i.database_name = 'dashboard'
AND s.index_name != 'PRIMARY'
AND i.stat_name = 'size'
AND s.cardinality < 10
ORDER BY size_in_mb DESC;
~~~

# Problems

Error when trying to login:

```
MySQL fails on: mysql "ERROR 1524 (HY000): Plugin 'auth_socket' is not loaded"
```

Solution:

~~~sql
use mysql;
update user set authentication_string=PASSWORD("YOUR_PASSWORD") where User='YOUR_USERNAME';
update user set plugin="mysql_native_password" where User='YOUR_USERNAME';
flush privileges;
~~~

