## Plex docker setup:

### Setup Network access

* You need to set two custom access urls in __Plex Server Settings -> Network_ settings page:  
`http://<server_lan_ip>:32400/,https://<plex.yourdomainname>:443/`

* You can actually turn off __Remote Access__ at this point.

* If you have a __PlexPass__ you can set __Network -> LAN Networks__ as appropriate (probably just 192.168.0.0/16 for example)
