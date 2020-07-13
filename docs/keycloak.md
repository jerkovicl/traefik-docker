## Keycloak docker setup:

### Suggested to create Keycloak database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE keycloak CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON keycloak.* TO 'keycloak'@'keycloak.traefik_proxy'IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### First run setup

- Initialize admin account:

```
docker exec keycloak keycloak/bin/add-user-keycloak.sh -u <username> -p <password>
```

- Setup OIDC provider using instructions below (fill out $CLIENT_ID (traefik-forward-auth for example) and $CLIENT_SECRET env variables after that):

```
// redirect uri --> https://auth.domain.ltd/_oauth
https://geek-cookbook.funkypenguin.co.nz/recipes/keycloak/setup-oidc-provider/
```

### Fail2Ban Integration

- create log file first `touch /var/log/docker/server.log`
- set permissions and ownership cause docker container user **PUID** is **1000**:

  ```
  sudo chmod 664 /var/log/docker/server.log
  sudo chown $USER:$USER /var/log/docker/server.log
  ```

### Some additional links

[Refernce for traefik auth](https://geek-cookbook.funkypenguin.co.nz/ha-docker-swarm/traefik-forward-auth/)  
[User restriction](https://github.com/thomseddon/traefik-forward-auth#user-restriction)  
[Docker image](https://hub.docker.com/r/jboss/keycloak/)  
[Keycloak docker recipe guide](https://geek-cookbook.funkypenguin.co.nz/recipes/keycloak/)  
[Keycloak docker info](https://github.com/keycloak/keycloak-containers/blob/master/server/README.md)  
[OpenId configuration url](https://<your-keycloak-url>/realms/master/.well-known/openid-configuration)  
[Keycloak Brute Force info](https://github.com/keycloak/keycloak-documentation/blob/master/server_admin/topics/threat/brute-force.adoc)   
[Keycloak Auditing and Events configuration](https://www.keycloak.org/docs/latest/server_admin/#auditing-and-events)
