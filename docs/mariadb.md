## Mariadb docker setup:

### Suggested procedure to create new databases

```
// Replace <these values>
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE <database name> CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON <database name>.* TO '<user>'@'<container>.traefik_proxy' IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### Create a custom.cnf

- Start the container to create the initial files, then stop the container and remove all but the custom.cnf.
`rm -R $USERDIR/docker/mariadb/databases`
`rm -R $USERDIR/docker/mariadb/log`
- Add the following to $USERDIR/docker/mariadb/custom.cnf under the [mysqld] section:
```
character_set_server=utf8mb4
collation_server=utf8mb4_unicode_ci
innodb_file_format=Barracuda
```
- Some resources for why these commands are chosen:
  > utf8mb4 is the most universal and up to date character set allowing for emojis among other benefits
  > utf8mb4_unicode_ci is the "standard" database type while general_ci is a simplified version which tried to improve speed before modern computing. I believe there is little to no benefit to use the simpler version.
  [Nextcloud info](https://docs.nextcloud.com/server/16/admin_manual/configuration_database/mysql_4byte_support.html)
  
### Check your database variables, for example

```
// For a complete list of available options: docker run -it --rm mariadb --verbose --help
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
SHOW VARIABLES LIKE 'character_set_server';
SHOW VARIABLES LIKE '%server%';
SHOW VARIABLES LIKE 'innodb%';
exit
```

### Delete the initial default databases and secure MySQL
// Accept all options except for 'Disallow root login remotely'. Answer no due to docker networking

`docker exec -it mariadb /usr/bin/mysql_secure_installation`
