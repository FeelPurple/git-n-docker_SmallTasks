3. Конфигурирование MariaDB

Разверните два контейнера MariaDB. Инициализируйте базу данных vedita-database. Настройте репликацию master-slave.

<hr>

<ins>Comments</ins>:

- start both nodes by running:

`docker compose -f db_compose.yml up -d`

- enter mariadb master node environment:

`docker compose -f db_compose.yml exec db-master-node bash`

- set up replication on master:

`MariaDB [(none)]> CREATE USER 'replication'@'%' IDENTIFIED BY 'SlaveReplPass2000';`

`MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO 'replication'@'%';`

Find *File* and *Position* properties in the output of below statement:

`MariaDB [(none)]> SHOW MASTER STATUS\G;` 

Initialize an empty database: 

`MariaDB [(none)]> CREATE DATABASE `` `vedita-database` ``;` 

- enter mariadb slave node environment:

`docker compose -f db_compose.yml exec db-slave-node bash`

- complete replication setup:

<code>MariaDB [(none)]> CHANGE MASTER TO <br/>
MASTER_HOST='db-master-node',<br/>
MASTER_USER='replication',<br/>
MASTER_PASSWORD='SlaveReplPass2000',<br/>
MASTER_LOG_FILE='<log file name on master node, e.g. <em>mysqld-bin.000001</em>',<br/>
MASTER_LOG_POS='<position in log file on master node, e.g. <em>329</em>,';<code>

`MariaDB [(none)]> START SLAVE;`
