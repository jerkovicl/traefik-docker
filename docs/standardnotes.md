## StandardNotes docker setup:

### Suggested to create StandardNotes database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE boards CHARACTER SET = utf32 COLLATE = utf32_general_ci ;
GRANT ALL PRIVILEGES ON standardnotes.* TO 'std_notes'@'%' IDENTIFIED BY 'BeowulfB6';
FLUSH PRIVILEGES;
exit
```

### Suggested to create Focalboard database using Postgres

```
CREATE DATABASE boards;
CREATE USER boardsuser WITH PASSWORD 'boardsuser-password';
```

### First run setup

- Check if server is working by going to url: `https://notes.domain.ltd/healthcheck`
- Activate extensions with url: `notes-ext.domain.ltd`

### Some additional links

[Docker setup guide](https://www.blackvoid.club/standard-notes-docker-self-hosted-alternative/amp/)
[Docker Traefik setup guide](https://ae3.ch/selfhosted-standard-notes-with-docker-and-traefik)
[Another selfhost guide](https://theselfhostingblog.com/posts/how-to-completely-self-host-standard-notes/)
[Docker compose example](https://pastebin.com/LFNwgscB)
[Selfhost extensions repo](https://github.com/iganeshk/standardnotes-extensions)
