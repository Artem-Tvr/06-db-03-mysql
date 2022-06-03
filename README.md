# Домашнее задание к занятию "6.3. MySQL"

## Задача 1

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

Изучите [бэкап БД](https://github.com/netology-code/virt-homeworks/tree/master/06-db-03-mysql/test_data) и восстановитесь из него.

Перейдите в управляющую консоль `mysql` внутри контейнера.

Используя команду `\h` получите список управляющих команд.

Найдите команду для выдачи статуса БД и **приведите в ответе** из ее вывода версию сервера БД.

Подключитесь к восстановленной БД и получите список таблиц из этой БД.

**Приведите в ответе** количество записей с `price` > 300.

В следующих заданиях мы будем продолжать работу с данным контейнером.

*Ответ:*

1. Запускаем контейнер с MySQL

```

root@cadcixztkv:~/hw-mysql# docker-compose up
Creating network "hw-mysql_default" with the default driver
Creating db_devops ... done
Attaching to db_devops
db_devops | 2022-05-31 15:14:04+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.29-1debian10 started.
db_devops | 2022-05-31 15:14:05+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
db_devops | 2022-05-31 15:14:05+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.29-1debian10 started.
db_devops | 2022-05-31T15:14:05.782732Z 0 [System] [MY-010116] [Server] /usr/sbin/mysqld (mysqld 8.0.29) starting as process 1
db_devops | 2022-05-31T15:14:05.811069Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
db_devops | 2022-05-31T15:14:06.267745Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
db_devops | 2022-05-31T15:14:06.658953Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
db_devops | 2022-05-31T15:14:06.660703Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
db_devops | 2022-05-31T15:14:06.663518Z 0 [Warning] [MY-011810] [Server] Insecure configuration for --pid-file: Location '/var/run/mysqld' in the path is accessible to all OS users. Consider choosing a different directory.
db_devops | 2022-05-31T15:14:06.705219Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /var/run/mysqld/mysqlx.sock
db_devops | 2022-05-31T15:14:06.706707Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.29'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.

```

2. Подключаемся к контейнеру, создаем пустую базу test_db и в нее восстанавливаем дамп.

```
root@cadcixztkv:~# docker exec -ti ba870e1cec81 bash
root@ba870e1cec81:/# ls -la
total 80
drwxr-xr-x   1 root root 4096 May 31 15:14 .
drwxr-xr-x   1 root root 4096 May 31 15:14 ..
-rwxr-xr-x   1 root root    0 May 31 15:14 .dockerenv
drwxr-xr-x   2 root root 4096 May 27 00:00 bin
drwxr-xr-x   2 root root 4096 Mar 19 13:44 boot
drwxr-xr-x   5 root root  360 May 31 17:28 dev
drwxr-xr-x   2 root root 4096 May 28 05:33 docker-entrypoint-initdb.d
lrwxrwxrwx   1 root root   34 May 28 05:33 entrypoint.sh -> usr/local/bin/docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 May 31 15:14 etc
drwxr-xr-x   2 root root 4096 Mar 19 13:44 home
drwxr-xr-x   1 root root 4096 May 28 05:33 lib
drwxr-xr-x   2 root root 4096 May 27 00:00 lib64
drwxr-xr-x   2 root root 4096 May 27 00:00 media
drwxr-xr-x   2 root root 4096 May 27 00:00 mnt
drwxr-xr-x   2 root root 4096 May 27 00:00 opt
dr-xr-xr-x 175 root root    0 May 31 17:28 proc
drwx------   1 root root 4096 May 28 05:33 root
drwxr-xr-x   1 root root 4096 May 28 05:33 run
drwxr-xr-x   2 root root 4096 May 27 00:00 sbin
drwxr-xr-x   2 root root 4096 May 27 00:00 srv
dr-xr-xr-x  13 root root    0 May 31 17:28 sys
drwxrwxrwt   1 root root 4096 May 31 17:28 tmp
drwxr-xr-x   1 root root 4096 May 27 00:00 usr
drwxr-xr-x   1 root root 4096 May 27 00:00 var

root@ba870e1cec81:/var/lib/mysql# mysql -u root -p test_db < /var/lib/mysql/test_dump.sql
Enter password:


```


```
mysql> use test_db;
Database changed
mysql> show tables;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)

mysql> \s
--------------
mysql  Ver 8.0.29-0ubuntu0.20.04.3 for Linux on x86_64 ((Ubuntu))

Connection id:          277
Current database:       test_db
Current user:           root@172.19.0.1
SSL:                    Cipher in use is TLS_AES_256_GCM_SHA384
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         8.0.29 MySQL Community Server - GPL
Protocol version:       10
Connection:             172.17.0.1 via TCP/IP
Server characterset:    utf8mb4
Db     characterset:    utf8mb4
Client characterset:    utf8mb4
Conn.  characterset:    utf8mb4
TCP port:               3306
Binary data as:         Hexadecimal
Uptime:                 2 days 23 hours 49 min 54 sec

Threads: 2  Questions: 2364  Slow queries: 0  Opens: 6111  Flush tables: 3  Open tables: 538  Queries per second avg: 0.009
--------------

```

Server version:         8.0.29 MySQL Community Server - GPL

```
mysql> select count(*) from orders where price > 300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)

```

## Задача 2

Создайте пользователя test в БД c паролем test-pass, используя:

* плагин авторизации mysql_native_password
* срок истечения пароля - 180 дней
* количество попыток авторизации - 3
* максимальное количество запросов в час - 100
* аттрибуты пользователя:
  * Фамилия "Pretty"
  * Имя "James"

Предоставьте привелегии пользователю `test` на операции SELECT базы `test_db`.

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю `test` и  **приведите в ответе к задаче** .

*Ответ:*

```
mysql> CREATE USER 'test'@'localhost' IDENTIFIED BY 'test-pass';
Query OK, 0 rows affected (0.04 sec)

mysql> ALTER USER 'test'@'localhost'
    -> IDENTIFIED BY 'test-pass'
    -> WITH
    -> MAX_QUERIES_PER_HOUR 100
    -> PASSWORD EXPIRE INTERVAL 180 DAY
    -> FAILED_LOGIN_ATTEMPTS 3 PASSWORD_LOCK_TIME 2;
Query OK, 0 rows affected (0.01 sec)

mysql> GRANT Select ON test_db.orders TO 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.01 sec)

mysql> ALTER USER 'test'@'localhost' ATTRIBUTE '{"fname":"James", "lname":"Pretty"}';
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER='test';
+------+-----------+---------------------------------------+
| USER | HOST      | ATTRIBUTE                             |
+------+-----------+---------------------------------------+
| test | localhost | {"fname": "James", "lname": "Pretty"} |
+------+-----------+---------------------------------------+
1 row in set (0.00 sec)

```


## Задача 3

Установите профилирование `SET profiling = 1`. Изучите вывод профилирования команд `SHOW PROFILES;`.

Исследуйте, какой `engine` используется в таблице БД `test_db` и  **приведите в ответе** .

Измените `engine` и  **приведите время выполнения и запрос на изменения из профайлера в ответе** :

* на `MyISAM`
* на `InnoDB`

*Ответ:*


```
mysql> SELECT TABLE_NAME,ENGINE,ROW_FORMAT,TABLE_ROWS,DATA_LENGTH,INDEX_LENGTH FROM information_schema.TABLES WHERE table_name = 'orders' and  TABLE_SCHEMA = 'test_db' ORDER BY ENGINE asc;
+------------+--------+------------+------------+-------------+--------------+
| TABLE_NAME | ENGINE | ROW_FORMAT | TABLE_ROWS | DATA_LENGTH | INDEX_LENGTH |
+------------+--------+------------+------------+-------------+--------------+
| orders     | InnoDB | Dynamic    |          5 |       16384 |            0 |
+------------+--------+------------+------------+-------------+--------------+
1 row in set (0.02 sec)

mysql> ALTER TABLE orders ENGINE = MyISAM;
Query OK, 5 rows affected (0.06 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> ALTER TABLE orders ENGINE = InnoDB;
Query OK, 5 rows affected (0.02 sec)
Records: 5  Duplicates: 0  Warnings: 0

mysql> show profiles;
+----------+------------+------------------------------------+
| Query_ID | Duration   | Query                              |
+----------+------------+------------------------------------+
|        1 | 0.00013225 | show prifiles                      |
|        2 | 0.06195100 | ALTER TABLE orders ENGINE = MyISAM |
|        3 | 0.02270325 | ALTER TABLE orders ENGINE = InnoDB |
+----------+------------+------------------------------------+
3 rows in set, 1 warning (0.00 sec)


```


## Задача 4

Изучите файл `my.cnf` в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

* Скорость IO важнее сохранности данных
* Нужна компрессия таблиц для экономии места на диске
* Размер буффера с незакомиченными транзакциями 1 Мб
* Буффер кеширования 30% от ОЗУ
* Размер файла логов операций 100 Мб

Приведите в ответе измененный файл `my.cnf`.

*Ответ:*

```
root@ba870e1cec81:/etc/mysql# cat my.cnf
# Copyright (c) 2017, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html

[mysqld]
pid-file        = /var/run/mysqld/mysqld.pid
socket          = /var/run/mysqld/mysqld.sock
datadir         = /var/lib/mysql
secure-file-priv= NULL

#Set IO Speed
# 0 - скорость
# 1 - сохранность
# 2 - универсальный параметр
innodb_flush_log_at_trx_commit = 0 

#Set compression
# Barracuda - формат файла с сжатием
innodb_file_format=Barracuda

#Set buffer
innodb_log_buffer_size	= 1M

#Set Cache size
key_buffer_size = 640М

#Set log size
max_binlog_size	= 100M

# Custom config should go here
!includedir /etc/mysql/conf.d/

```
