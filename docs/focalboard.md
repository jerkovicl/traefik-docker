## Focalboard docker setup:

### Suggested to create Focalboard database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE boards CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON boards.* TO 'focalboard'@'focalboard.traefik_proxy' IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### Suggested to create Focalboard database using Postgres

```
CREATE DATABASE boards;
CREATE USER boardsuser WITH PASSWORD 'boardsuser-password';
```

### First run setup

- Setup config.json for mysql

```
{
    "serverRoot": "http://localhost:8000",
    "port": 8000,
    "dbtype": "mysql",
    "dbconfig": "focalboard:password@tcp(mariadb:3306)/boards",
    "postgres_dbconfig": "dbname=boards sslmode=disable",
    "useSSL": false,
    "webpath": "./pack",
    "filespath": "./files",
    "telemetry": true,
    "prometheus_address": ":9092",
    "session_expire_time": 2592000,
    "session_refresh_time": 18000,
    "localOnly": false,
    "enableLocalMode": true,
    "localModeSocketLocation": "/var/tmp/focalboard_local.socket"
}
```

- or for postgres:

```
{
    "serverRoot": "http://localhost:8000",
    "port": 8000,
    "dbtype": "postgres",
    "dbconfig": "postgres://boardsuser:boardsuser-password@focalboard-db/boards?sslmode=disable&connect_timeout=10",
    "postgres_dbconfig": "dbname=boards sslmode=disable",
    "useSSL": false,
    "webpath": "./pack",
    "filespath": "./files",
    "telemetry": true,
    "prometheus_address": ":9092",
    "session_expire_time": 2592000,
    "session_refresh_time": 18000,
    "localOnly": false,
    "enableLocalMode": true,
    "localModeSocketLocation": "/var/tmp/focalboard_local.socket"
}
```

### Fix for failed migrations

```
update schema_migrations set version = version - 1, dirty = 0;
```

### Some additional links

[Linux setup guide](https://www.focalboard.com/download/personal-edition/ubuntu/)  
