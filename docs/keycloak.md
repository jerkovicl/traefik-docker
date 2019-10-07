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

- Setup OIDC provider using instructions below (fill out $CLIENT_ID and $CLIENT_SECRET env variables after that):

```
// redirect uri --> https://auth.domain.ltd/_oauth
https://geek-cookbook.funkypenguin.co.nz/recipes/keycloak/setup-oidc-provider/
```



### Some additional links

[Refernce for traefik auth](https://geek-cookbook.funkypenguin.co.nz/ha-docker-swarm/traefik-forward-auth/)  
[User restriction](https://github.com/thomseddon/traefik-forward-auth#user-restriction)  
[Docker image](https://hub.docker.com/r/jboss/keycloak/)  
[Keycloak docker recipe guide](https://geek-cookbook.funkypenguin.co.nz/recipes/keycloak/)  
[Keycloak docker info](https://github.com/keycloak/keycloak-containers/blob/master/server/README.md)  
[OpenId configuration url](https://<your-keycloak-url>/realms/master/.well-known/openid-configuration)  
