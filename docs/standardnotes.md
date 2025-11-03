## StandardNotes docker setup:

### Suggested to create StandardNotes database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE boards CHARACTER SET = utf32 COLLATE = utf32_general_ci ;
GRANT ALL PRIVILEGES ON standardnotes.* TO 'std_notes'@'%' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
exit
```

### First run setup

- Check if server is working by going to url: `https://notes.domain.ltd/healthcheck`
- Activate extensions with url: `https://notes-ext.domain.ltd`
- Activate pro plan executing sql statements on db (replace email with yours):
  ```
  INSERT INTO user_roles (role_uuid , user_uuid) VALUES ((SELECT uuid FROM roles WHERE name="PRO_USER" ORDER BY version DESC limit 1) ,(SELECT uuid FROM users WHERE email="dmyil@test.com")) ON DUPLICATE KEY UPDATE role_uuid = VALUES(role_uuid);

  
  INSERT INTO user_subscriptions SET uuid=UUID(), plan_name="PRO_PLAN", ends_at=8640000000000000, created_at=0, updated_at=0, user_uuid=(SELECT uuid FROM users WHERE email="email@test.com"), subscription_id=1, subscription_type="regular";
 ```  


### Some additional links

[Official Docker setup guide](https://docs.standardnotes.com/self-hosting/getting-started)
[Docker setup guide](https://www.blackvoid.club/standard-notes-docker-self-hosted-alternative/amp/)  
[Docker Traefik setup guide](https://ae3.ch/selfhosted-standard-notes-with-docker-and-traefik)  
[Another selfhost guide](https://theselfhostingblog.com/posts/how-to-completely-self-host-standard-notes/)  
[Docker compose example](https://pastebin.com/LFNwgscB)  
[Selfhost extensions repo](https://github.com/iganeshk/standardnotes-extensions)  
[Awesome List - collection of editors and themes](https://github.com/jonhadfield/awesome-standard-notes)
[Activate pro plan guide](https://docs.standardnotes.com/self-hosting/subscriptions/)
