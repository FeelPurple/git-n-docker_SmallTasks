3. Конфигурирование MariaDB

Разверните два контейнера MariaDB. Инициализируйте базу данных vedita-database. Настройте репликацию master-slave.

<hr>

<ins>Comments</ins>:

- start both nodes by running:

<code>docker compose -f db_compose.yml up -d</code>

- enter mariadb master node environment:

<code>docker compose -f db_compose.yml exec db-master-node bash</code>

- set up replication on master:

<code>MariaDB [(none)]> CREATE USER 'replication'@'%' IDENTIFIED BY 'SlaveReplPass2000';</code>

<code>MariaDB [(none)]> GRANT REPLICATION SLAVE ON *.* TO 'replication'@'%';</code>

Find *File* and *Position* properties in the output of below statement:

<code>MariaDB [(none)]> SHOW MASTER STATUS\G;</code> 

Initialize an empty database:

<code>MariaDB [(none)]> CREATE DATABASE `vedita-database`;</code> 

- enter mariadb slave node environment:

<code>docker compose -f db_compose.yml exec db-slave-node bash</code>

- complete replication setup:

<code>MariaDB [(none)]> CHANGE MASTER TO <br/>
MASTER_HOST='db-master-node',<br/>
MASTER_USER='replication',<br/>
MASTER_PASSWORD='SlaveReplPass2000',<br/>
MASTER_LOG_FILE='<log file name on master node, e.g. <em>mysqld-bin.000001</em>',<br/>
MASTER_LOG_POS='<position in log file on master node, e.g. <em>329</em>,';<code>

<code>MariaDB [(none)]> START SLAVE;</code>
