3. Конфигурирование MariaDB

Разверните два контейнера MariaDB. Инициализируйте базу данных vedita-database. Настройте репликацию master-slave.

<hr>

<ins>Comments</ins>:

- start both nodes by running:

`docker compose -f db_compose.yml up -d`

- enter `mariadb` master node environment:

`docker compose -f db_compose.yml exec db-master-node bash`

- enter `mariadb` slave node environment:

`docker compose -f db_compose.yml exec db-slave-node bash`
