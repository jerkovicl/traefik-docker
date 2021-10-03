## Focalboard docker setup:

### Suggested to create Focalboard database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE boards CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON keycloak.* TO 'focalboard'@'focalboard.traefik_proxy'IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### First run setup

- Setup config.json for mysql

```
{
    "serverRoot": "http://localhost:8000",
    "port": 8000,
    "dbtype": "mysql",
    "dbconfig": "djlujo:BeowulfB6@tcp(127.0.0.1:3306)/boards",
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

### Some additional links

[Linux setup guide](https://www.focalboard.com/download/personal-edition/ubuntu/)  
