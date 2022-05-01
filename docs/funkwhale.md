## Funkwhale docker setup:

### First run setup

- create admin account:

```
docker exec -it funkwhale manage createsuperuser
```

- create library on this page `https://domain.ltd/content/libraries` and save library id to fill $LIBRARY_ID env variable in container

- Official docs [here](https://docs.funkwhale.audio/installation/docker.html#docker-mono-container)

### Import music

- to import music, Set the $LIBRARY_ID environment variable (or replace it inside of the command) with your library ID, then run the command below:

```
// For file structures similar to ./Artist/Album/Track.mp3
docker exec -it funkwhale manage import_files $LIBRARY_ID "/music/" --in-place --async --recursive
```
