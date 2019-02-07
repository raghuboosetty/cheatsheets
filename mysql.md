# Dump and Load MySQL - terminal/shell
```sh
# take dump
mysqldump -u username -p password database_name > database_dump.sql
# with no data/ just schema
mysqldump -u username -p password database_name > database_dump.sql  --no-data

# import dump
mysql -u username -p password database_name < database_dump.sql
#  multi-thread import
mysqlimport -u username -p password database_name < database_dump.sql
```

# Basic Commands - mysql prompt
```sql
USE databse_name;                  -- select db
SELECT * FROM table_name;          -- select all the rows
SELECT COUNT(*) FROM table_name;   -- count records/rows
SELECT * FROM table_name\G         -- display rows in non-cluttered format

SHOW TABLES;                       -- show all the tables
SHOW COLUMNS FROM table_name;      -- display columns
SHOW FULL FIELDS FROM table_name   -- display all fields
SHOW INDEX FROM table_name;        -- indexes in a table
DESCRIBE table_name;               -- schema of a table

-- reset username/password
UPDATE mysql.user SET Password=PASSWORD('newpassword') WHERE User='root';
UPDATE mysql.user SET user='newusername', password=PASSWORD('newpassword') WHERE user='root';
FLUSH PRIVILEGES;

-- create new user with all privileges
GRANT ALL PRIVILEGES ON database.* To 'user'@'hostname' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;

-- e.g:
UPDATE mysql.user SET user='pp_root' WHERE user='root';
GRANT ALL PRIVILEGES ON pops_production.* To 'pp_root'@'localhost' IDENTIFIED BY 'AUVJa7tUL6';
GRANT ALL PRIVILEGES ON alicia.* To 'pp_root'@'localhost' IDENTIFIED BY 'AUVJa7tUL6';


-- non-Windows: will pipe the outut through the less command line tool
pager less -SFX   -- set
nopager           -- reset

-- altering tables
ALTER TABLE table1 RENAME TO table2;                              -- rename a table
ALTER TABLE table_name ADD PRIMARY KEY (id);                      -- add primary key
ALTER TABLE table_name ADD id INT PRIMARY KEY AUTO_INCREMENT;     -- add primary key with auto-increment
ALTER TABLE table_name MODIFY id INT AUTO_INCREMENT PRIMARY KEY;  -- add auto-increment to id
ALTER TABLE table_name CHANGE column1 column2 DATETIME;           -- rename column
-- e.g:
ALTER TABLE jobs RENAME TO jobs_old;
ALTER TABLE raw_listings MODIFY id INT AUTO_INCREMENT PRIMARY KEY;
ALTER TABLE raw_listings CHANGE updated_at processed_at DATETIME;

-- Replicate a table
CREATE TABLE table_name LIKE table_name_old;
-- e.g:
CREATE TABLE raw_listings LIKE raw_listings_old;

-- Insert from table1 to table2
INSERT INTO table2 (column1_table2, column2_table2)
SELECT column1_table1, column2_table1
FROM table1;
-- e.g:
INSERT INTO raw_listings (source_id, url, raw)
SELECT source_id, url, raw
FROM raw_listings_old;

-- Delete
TRUNCATE TABLE table_name;  -- empty
DROP TABLE table_name;      -- destroy
```

# Row Count [All tables] - mysql prompt
```sql
-- default
SHOW TABLE STATUS;

-- select columns to show from above
SELECT table_name,table_rows
FROM information_schema.tables
WHERE table_schema = DATABASE();

-- all
SELECT table_name,Engine,Version,Row_format,table_rows,Avg_row_length,
Data_length,Max_data_length,Index_length,Data_free,Auto_increment,
Create_time,Update_time,Check_time,table_collation,Checksum,
Create_options,table_comment FROM information_schema.tables
WHERE table_schema = DATABASE();

-- directly from schema by using it
use information_schema;
SELECT * FROM statistics;
```

# Table/Schema size [KB, MB, GB] - mysql prompt
```sql
-- Database Size in MB
SELECT table_schema AS "Databases",
ROUND(SUM(data_length + index_length) / POWER(1024,2), 3) AS "Size (MB)"
FROM information_schema.TABLES
GROUP BY table_schema;

-- Schema's size in GB per database
SELECT table_schema AS "Databases",
ROUND(SUM(data_length + index_length) / POWER(1024,3), 3) AS "Size (GB)"
FROM information_schema.TABLES
GROUP BY table_schema;

-- Table's size in MB table format
SELECT table_name AS "Table",
ROUND(((data_length + index_length) / POWER(1024, 2), 2) "Size (MB)"
FROM information_schema.TABLES
WHERE table_schema = "$DB_NAME"
AND table_name = "$TABLE_NAME";

-------------
-- Example --
-------------

SELECT (data_length+index_length)/power(1024,3) tablesize_gb FROM information_schema.tables WHERE table_schema='localbiz_staging';

SELECT table_name AS "Table",
round(((data_length + index_length) / power(1024, 2)), 2) "Size (MB)"
FROM information_schema.TABLES
WHERE table_schema = "cape_development"
AND table_name = "reservations";
```
### My detailed blog post about the table sizes:
https://raghuror.wordpress.com/2015/10/02/mysql-table-or-schema-size-stats-or-row-counts/

# Granting/Revoking Permissions [ALL] - mysql prompt
```sql
-- Granting
GRANT ALL PRIVILEGES ON *.* TO 'root'@'54.212.95.101' IDENTIFIED BY 'dd84R96b8yJ1zs5K' WITH GRANT OPTION;

-- Revoking
revoke usage on *.* from 'user'@'ec2-54-212-95-101.us-west-2.compute.amazonaws.com';
revoke usage on *.* from 'user'@'54.212.95.101';
revoke usage on *.* from 'root'@'54.212.95.101';
```

# Slow/General Query Logs
```sql
-- check if performance schema is on - run from mysql prompt
SHOW VARIABLES LIKE 'performance_schema';

-- Enable SlowQueryLog - run from mysql prompt
set GLOBAL slow_query_log = 'ON';
set GLOBAL log_queries_not_using_indexes = 'ON';
set GLOBAL slow_query_log_file ='/var/log/mysql/slow-query.log';
set GLOBAL long_query_time = '10'; -- in seconds
show variables like '%slow%';
```
```sh
# Check SlowQuery - run from terminal
mysqldumpslow /var/log/mysql/slow-query.log
```
```sql
-- General Query and Checking - run from mysql prompt
SET global general_log = 1;
```
```sh
# Dump slow queries to logs - run from terminal
mysqldumpslow -a -s r -t 5 /var/log/mysql/slow-query.log
mysqldumpslow -a -s c -t 5 /var/log/mysql/slow-query.log
```

## Flush logs
```sql
-- run from terminal
sudo mv /var/lib/mysql/ip-10-197-143-124-slow.log /var/lib/mysql/ip-10-197-143-124-slow-old.log
```
```sh
#  run from mysql prompt
FLUSH LOGS;
```

## References
* http://toastergremlin.com/?p=276
* https://rtcamp.com/tutorials/mysql/slow-query-log/


# Permissions - Shell/Prompt access
```sh
chown -R root:mysql /usr/local/var/mysql
chmod -R 755 /usr/local/var/mysql

chown -R mysql:mysql /usr/local/var/mysql
chmod -R 755 /usr/local/var/mysql

/usr/local/bin/mysql_install_db --basedir=/usr/local
/usr/local/Cellar/mysql/5.6.10/bin/mysql_install_db --basedir=/usr/local/Cellar/mysql/5.6.10
```


# Remove MySQL : Mac and Ubuntu
## Mac
```sh
sudo rm /usr/local/mysql
sudo rm -rf /usr/local/mysql*

sudo rm /usr/local/var/mysql
sudo rm -rf /usr/local/var/mysql/*

sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
edit /etc/hostconfig and remove the line MYSQLCOM=-YES-
rm -rf ~/Library/PreferencePanes/My*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /private/var/db/receipts/*mysql*
```
## Ubuntu [Detailed]
```sh
sudo apt-get purge mysql-server mysql-common
sudo apt-get purge mysql-client
sudo apt-get autoremove mysql-server mysql-client mysql-common
sudo apt-get autoclean mysql-server mysql-client mysql-common

sudo apt-get autoremove

sudo rm -rf /etc/mysql/
# References:
# http://www.cyberciti.biz/faq/uninstall-mysql-ubuntu-linux-command/

```
### Purging
```sh
sudo dpkg --purge mysql-client-core-5.5 -- or alternative version
sudo dpkg --purge mysql-client
sudo dpkg --purge mysql-server-core-5.5 -- or alternative version
sudo dpkg --purge mysql-common

# this will list the mysql versions that it has, you can select and remove those
sudo dpkg --purge mysql -- followed by two tabs

sudo apt-get --purge remove mysql-server
sudo apt-get --purge remove mysql-client
sudo apt-get --purge remove mysql-common
sudo apt-get autoremove
sudo apt-get autoclean

sudo rm -rf /etc/mysql

# checking
which mysql     -- should not return anything
mysql --version -- should ask if we want to install it

# just few updates not related to mysql
sudo dpkg --configure -a
sudo apt-get update
```
# InnoDB: Resolving Errors
```sql
-- Lock wait timeout exceeded; try restarting transaction:
-- check the following in it and the query_id
------------------
-- TRANSACTIONS --
------------------
SHOW ENGINE INNODB STATUS\G
SHOW VARIABLES LIKE 'innodb_lock_wait_timeout'; -- innodb_lock_wait_timeout defaults to 50 seconds

-- check the query_id in the process_list
SELECT * FROM information_schema.PROCESSLIST;
SHOW FULL PROCESSLIST \G;
SHOW PROCESSLIST;

-----------------------------------
-- Possible reasons for Deadlock --
-----------------------------------

-- in your query (misplaced char, cartesian product, ...)
-- very numerous records to edit
-- complex joins or tests (MD5, substrings, LIKE %...%, etc.)
-- data structure problem
-- foreign key model (chain/loop locking)
-- misindexed data

UNLOCK TABLES;
FLUSH TABLES WITH READ LOCK;

KILL ID;

innodb_locks_unsafe_for_binlog = 1
```

## Mysql2::Error: Error on delete of './cape_development//db.opt' (Errcode: 13 - Permission denied):
```sql
show variables where Variable_name ='datadir'; -- Where is MySql table data stored(execute following in mysql prompt)
ls -la /usr/local/var/mysql                    -- execute following in console/terminal
sudo chown -R reonios /usr/local/var/mysql     -- check the user from above (third column name is user) and that user should be added below instead of reonios

-- references
-- http://dba.stackexchange.com/a/42664
```

## ERROR! The server quit without updating PID file (/usr/local/var/mysql/MacBook-Pro.local.pid).
* Go to given path i.e<br/>
``` $ cd /usr/local/var/mysql/ ```
* find out the PID from "Activiy Monitor"<br/>
  - Force quit or kill it(NOTE: you can find additional information by clicking 'info' icon)
  or
  - create a pid file like in given above and add the pid to that file and then stop mysql
* remove all the `.err` files(make sure of the below command, I took backup not rm)<br/>
```$ rm *.err /usr/local/var/mysql/ ```
* start mysql<br/>
``` $ mysql.server start ```

Optional step: If you've created `etc/my.cnf` file then take a backup of that file or remove it and then try to restart. It should work!


## An error occurred while installing mysql2 (0.3.21), and Bundler cannot continue.
``` bundle config --local build.mysql2 "--with-ldflags=-L/usr/local/opt/openssl/lib --with-cppflags=-I/usr/local/opt/openssl/include" ```


# Config file [my.cnf]
```sh
brew info mysql   # location of mysql and other details
mysql --help      # find out all options

#  my.cnf priority order
/etc/my.cnf /etc/mysql/my.cnf /usr/local/etc/my.cnf ~/.my.cnf

# default my.cnf is in '/usr/local/Cellar/mysql/5.6.25/support-files'
# This file need to be copied to one of the above locations
ls $(brew --prefix mysql)/support-files/my-*                               # to view default cnf file
sudo cp $(brew --prefix mysql)/support-files/my-default.cnf /etc/my.cnf    # copy default cnf to etc/

# add server_id for master/slave error
# 1. copy default-my.cnf to /etc/my.cnf
# 2. enable server_id with 'server_id = 2’
# 3. save and import the dump
# 4. remove my.cnf
```

# Miscellaneous Info
## ERRORS
* Timestamp, Thread, Type, Details
* InnoDB:, ERROR: the age of the last checkpoint is 9434879,
* InnoDB: which exceeds the log group capacity 9433498.
* InnoDB: If you are using big BLOB or TEXT rows, you must set the
* InnoDB: combined size of log files at least 10 times bigger than the
* InnoDB: largest such row.


## Jobsin Monthly Stats
```sql
SELECT MONTH(created_at), COUNT(created_at)
FROM raw_listings
WHERE created_at >= NOW() - INTERVAL 1 YEAR
GROUP BY MONTH(created_at);
-- MONTHNAME instead of MONTH gives name of month
```
## Server start/stop - [Mac]
```sh
mysql.server start
mysql.server stop
mysql.server restart
```