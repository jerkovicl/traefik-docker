# Cloudflare setup

Rather than repost the excellent instructions on how to initially set up Cloudflare as your DNS provider, here is the link to their page: https://support.cloudflare.com/hc/en-us/articles/201720164-Step-2-Create-a-Cloudflare-account-and-add-a-website

Note that if you have multiple sites you do NOT need a separate account for each. You can add multiple sites, each with a different IP, to the same Cloudflare account. They will all share the same API - which makes it easier to manage - but each has their own IPs, A Record(s), CNAMEs, Page Rules, etc.

**Failure to configure CF correctly will result in cert errors or too many redirect errors.**
**Once you applied this changes, make sure you clear your browser cache and purge the CF cache!**

## DNS Setup

* 1 A record that is mydomain.com and points to your IP, enable orange cloud.
* for each app, add a CNAME, use the appname for the Name and @ for the value, orange cloud on
* To hide the actual IP from the public, everything must have the "orange cloud" enabled.
* You need to have 1 A record listing the top level domain to the actual IP of your domain (i.e. mydomain.com)
  * **DO NOT USE WILDCARDS** They do not work for free accounts! If you have one, remove it! You have to create a separate listing for each sub-domain (i.e. portainer.mydomain.com)
![A record for TLD](https://imgur.com/n74WBlS.png)
* Use CNAMEs for the sub domains (i.e. portainer.mydomain.com) that are an alias of the TLD you listed for your A record.

| Type  | Name         | Value           | TTL       | Status |
|-------|--------------|-----------------|-----------|--------|
| A     | mydomain.com | 111.111.111.111 | Automatic | Orange ☁️ |
| CNAME | plex         | @               | Automatic | Orange ☁️ |
| CNAME | portainer    | @               | Automatic | Orange ☁️ |
| CNAME | radarr       | @               | Automatic | Orange ☁️ |
| CNAME | sonarr       | @               | Automatic | Orange ☁️ |
| CNAME | nzbget       | @               | Automatic | Orange ☁️ |
| CNAME | sabnzbd      | @               | Automatic | Orange ☁️ |


* Add CNames for the rest of the apps that you are using, use the appname as listed in PG as the Name.

| Type  | Name         | Value           | TTL       | Status |
|-------|--------------|-----------------|-----------|--------|
| CNAME | _appname_    | @               | Automatic | Orange ☁️ |

## Crypto Settings

| Setting Name                          | Value                                   |
|---------------------------------------|-----------------------------------------|
| SSL                                   | Full (strict)                           |
| Always Use HTTPS                      | 🟩 On                                      |
| HTTP Strict Transport Security (HSTS) | 🟩 On, Include Subdomains: On, Preload: On |
| Authenticated Origin Pulls            | 🟩 On                                      |
| Minimum TLS Version                   | TLS 1.2                                 |
| Opportunistic Encryption              | 🟩 On                                      |
| Onion Routing                         | 🟥 Off                                      |
| TLS 1.3                               | Enabled +0RTT                           |
| Automatic HTTPS Rewrites              | 🟩 On                                      |
| Disable Universal SSL                 | Keep Universal SSL On (do nothing)      |

**Once you applied this changes, make sure you clear your browser cache and purge the CF cache!**

## Caching

| Setting Name             | Value                    |
|--------------------------|--------------------------|
| Caching Level            | Standard                 |
| Browser Cache Expiration | Respect Existing Headers |
| Always Online            | Off                      |
| Development Mode         | Off                      |

## Page Rules

**This step is very important** Failure to setup this page rule will result in CF terminating your account!
Note: You are limited to 3 page rules for free.

![Plex page rule for Cloudflare](https://imgur.com/6u6GHnE.png)

| Url                         | Cache Level |
|-----------------------------|-------------|
| https://plex.mydomain.com/* | Bypass      |
| https://emby.mydomain.com/* | Bypass      |
| https://jellyfin.mydomain.com/* | Bypass      |

Alternatively, you can bypass the CF cache for everything using:

| Url                         | Cache Level |
|-----------------------------|-------------|
| https://\*.mydomain.com/* | Bypass      |


## 3A. Cloudflare as Content Delivery Network (CDN) for Plex

1. Go to plex web
1. Go to settings
1. Go to Network
1. Enable Advanced Settings

| Plex Network Setting          | Value                         |
|-------------------------------|-------------------------------|
| LAN Networks                  | 172.17.0.0/16,172.18.0.0/16   |
| Treat WAN IP As LAN Bandwidth | Checked                       |
| Custom server access URLs     | https://plex.mydomain.com:443 |

* You must have `https://` and `:443`, just like it's listed above.

### Plex Remote access

Disable "Remote Access", Everything will still connect, including all the apps.
* **Note:** You will see red ! next to remote access. Learn to ignore this, this is normal and expected. Everything will still connect just fine if you followed all of the configuration to a T.

**Once you applied this changes, make sure you clear your browser cache and purge the CF cache!**  
**Credit to PGBlitz for this guide**

### Some additional links

* https://www.smarthomebeginner.com/cloudflare-settings-for-traefik-docker/
