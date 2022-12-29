## Plex docker setup:

### Setup Network access

* You need to set two custom access urls in __Plex Server Settings -> Network__ settings page:  
`http://<server_lan_ip>:32400/,https://<plex.yourdomainname>:443/`

* You can actually turn off __Remote Access__ at this point.

* If you have a __PlexPass__ you can set __Network -> LAN Networks__ as appropriate (probably just 192.168.0.0/16 for example)

### Fix subtitles encoding

- install recode utility `sudo apt-get install recode`

- in dir

```
sudo recode cp1250.. ./*.srt
```

- recursivly in dir

```
sudo find . -type f -name "*.txt" -exec recode cp1250.. {} +
```