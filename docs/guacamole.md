## Guacamole docker setup:

### Suggested to create NextCloud database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE guacamole CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON guacamole.* TO 'guac'@'guacamole.traefik_proxy' IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### First run setup

* run script from [here](https://raw.githubusercontent.com/jerkovicl/traefik-docker/master/scripts/init_guacamole_db.sql) to seed db

### Fail2ban integration

> jail.d/guacamole.conf
