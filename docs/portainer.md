## Portainer docker setup:

### Reset admin password

```
docker stop portainer 
docker pull portainer/helper-reset-password
docker run --rm -v ~/docker/portainer/data:/data portainer/helper-reset-password
docker start portainer 
```