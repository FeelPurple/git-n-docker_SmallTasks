3. Конфигурирование MariaDB

Разверните два контейнера MariaDB. Инициализируйте базу данных vedita-database. Настройте репликацию master-slave.

<hr>

<ins>Comments</ins>:

- start both nodes by running:

`docker compose -f db_compose.yml up -d`

- enter `mariadb` master node environment:

`docker compose -f db_compose.yml exec db-master-node bash`

- set up replication on master:

`CREATE USER 'replication'@'%' IDENTIFIED BY 'SlaveReplPass2000'`;

`GRANT REPLICATION SLAVE ON \*.\* TO 'replication'@'%'`;

Find *File* and *Position* in the output of below statement:

`SHOW MASTER STATUS\G`; 

Initialize an empty database: 

`CREATE DATABASE `vedita-database``; 

- enter `mariadb` slave node environment:

`docker compose -f db_compose.yml exec db-slave-node bash`

- complete replication setup:

`CHANGE MASTER TO
MASTER_HOST='db-master-node',
MASTER_USER='replication',
MASTER_PASSWORD='SlaveReplPass2000',
MASTER_LOG_FILE='<log file name on master node, e.g. *mysqld-bin.000001*>',
MASTER_LOG_POS='<position in log file on master node, e.g. *329*>';`

`START SLAVE;` 
