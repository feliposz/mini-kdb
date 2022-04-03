# MariaDB


## Running MariaDB from binaries (portable)

Download binaries from `https://downloads.mariadb.org/mariadb/10.3.12/#file_type=zip&os_group=windows`

Unzip on a directory, like: `C:\Local\Portable\mariadb\`.

Start the server in _portable_ mode:

```
C:\Local\Portable\mariadb\bin\mysqld.exe --no-defaults
```

> TODO: how to set password/authentication?

Connect locally:

```
C:\Local\Portable\mariadb\bin\mysql --no-defaults
```
