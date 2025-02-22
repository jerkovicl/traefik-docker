## Nextcloud docker setup:

### Suggested procedure to create NextCloud database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE nextcloud CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'nextcloud.traefik_proxy'IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### First run setup

- under Storage & database select MySQL/MariaDB and enter:

```
Database user: your db user (nextcloud)
Database password: your db password (<password>)
Database name: your db name (nextcloud)
localhost: mariadb:3306
```

- If you have header issues in the settings dashboard, fix by editing `$USERDIR/docker/nextcloud/www/nextcloud/config/config.php`

```
// Add these lines within the config array:
'forwarded_for_headers' =>
array (
      0 => 'HTTP_X_FORWARDED_FOR',
      ),
```

- If you have the **bigint** issue:

```
docker exec -it nextcloud sh
sudo -u abc php /config/www/nextcloud/occ db:convert-filecache-bigint
exit
```

- If you have missing indexes warning:  

```
docker exec -it nextcloud sh
sudo -u abc php /config/www/nextcloud/occ db:add-missing-indices
exit
```

- Turn of maintenance mode

```
docker exec -it nextcloud occ maintenance:mode --off
```

### Redis setup (optional)

- If you have a Redis server you can connect to it in `$USERDIR/docker/nextcloud/www/nextcloud/config/config.php`

```
// Add the following lines after " 'memcache.local' => '\\OC\\Memcache\\APCu', "
'memcache.distributed' => '\\OC\\Memcache\\Redis',
'memcache.locking' => '\\OC\\Memcache\\Redis',
'redis' => array(
'host' => 'redis',
'port' => 6379,
),
```

### Updating Nextcloud to new version:

- you need to launch updater using webgui, because updating docker image won't update it. 
- Alternative method via cli: `docker exec -it nextcloud updater.phar`

### Other occ commands info

- [docs](https://docs.nextcloud.com/server/stable/admin_manual/configuration_server/occ_command.html#file-operations-label)
