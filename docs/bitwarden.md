## Bitwarden docker setup:

### Suggested procedure to create Bitwarden database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE bitwarden CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON bitwarden.* TO 'bitwarden'@'bitwarden.traefik_proxy' IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### First run setup

- activate otp
- disable signups
- verify admin token status
