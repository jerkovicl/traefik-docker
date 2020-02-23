## Guacamole docker setup:

### Suggested to create Guacamole database using MariaDB

```
docker exec -it mariadb mysql -uroot -p$MYSQL_ROOT_PASSWORD
CREATE DATABASE guacamole CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
GRANT ALL PRIVILEGES ON guacamole.* TO 'guac'@'guacamole.traefik_proxy' IDENTIFIED BY '<password>';
FLUSH PRIVILEGES;
exit
```

### First run setup

* run script from [here](https://raw.githubusercontent.com/jerkovicl/traefik-docker/master/scripts/init_guacamole_db.sql) to seed db

### Fail2ban integration

* Referenced from [here](https://github.com/crazy-max/docker-fail2ban/tree/master/examples/jails)

> jail.d/guacamole.conf
```
[DEFAULT]
banaction = cloudflare

[guacamole-auth]
enabled = true
logpath = /var/log/guacamole/guacd.log
port = http,https

bantime = -1
maxretry = 5
```
> filter.d/guacamole-auth.conf
```
[Definition]
failregex = \bAuthentication attempt from \[<HOST>(?:,.*)?\] for user ".*" failed\.
ignoreregex =
```
> action.d/cloudflare.conf
```

# IMPORTANT
#
# Please set jail.local's permission to 640 because it contains your CF API key.
#
# This action depends on curl.
# Sample file here also: https://fossies.org/linux/misc/fail2ban-0.11.1.tar.gz:t/fail2ban-0.11.1/config/action.d/cloudflare.conf
# Referenced from https://technicalramblings.com/blog/cloudflare-fail2ban-integration-with-automated-set_real_ip_from-in-nginx/
#
# To get your CloudFlare API Key: https://www.cloudflare.com/a/account/my-account
#
# CloudFlare API error codes: https://www.cloudflare.com/docs/host-api.html#s4.2

#
# Author: Gilbn from https://technicalramblings.com
# Adapted Source: https://github.com/fail2ban/fail2ban/blob/master/config/action.d/cloudflare.conf and https://guides.wp-bullet.com/integrate-fail2ban-cloudflare-api-v4-guide/
#
# To get your Cloudflare API key: https://dash.cloudflare.com/profile use the Global API Key
#

[Definition]

# Option:  actionstart
# Notes.:  command executed once at the start of Fail2Ban.
# Values:  CMD
#
actionstart =

# Option:  actionstop
# Notes.:  command executed once at the end of Fail2Ban
# Values:  CMD
#
actionstop =

# Option:  actioncheck
# Notes.:  command executed once before each actionban command
# Values:  CMD
#
actioncheck =

# Option:  actionban
# Notes.:  command executed when banning an IP. Take care that the
#          command is executed with Fail2Ban user rights.
# Tags:      IP address
#            number of failures
#            unix timestamp of the ban time
# Values:  CMD

actionban = curl -s -X POST "https://api.cloudflare.com/client/v4/user/firewall/access_rules/rules" \
            -H "X-Auth-Email: <cfuser>" \
            -H "X-Auth-Key: <cftoken>" \
            -H "Content-Type: application/json" \
            --data '{"mode":"block","configuration":{"target":"ip","value":"<ip>"},"notes":"Fail2ban <name>"}'

# Option:  actionunban
# Notes.:  command executed when unbanning an IP. Take care that the
#          command is executed with Fail2Ban user rights.
# Tags:      IP address
#            number of failures
#            unix timestamp of the ban time
# Values:  CMD
#

actionunban = curl -s -X DELETE "https://api.cloudflare.com/client/v4/user/firewall/access_rules/rules/$( \
              curl -s -X GET "https://api.cloudflare.com/client/v4/user/firewall/access_rules/rules?mode=block&configuration_target=ip&configuration_value=<ip>&page=1&per_page=1&match=all" \
             -H "X-Auth-Email: <cfuser>" \
             -H "X-Auth-Key: <cftoken>" \
             -H "Content-Type: application/json" | awk -F"[,:}]" '{for(i=1;i<=NF;i++){if($i~/'id'\042/){print $(i+1);}}}' | tr -d '"' | sed -e 's/^[ \t]*//' | head -n 1)" \
             -H "X-Auth-Email: <cfuser>" \
             -H "X-Auth-Key: <cftoken>" \
             -H "Content-Type: application/json"

[Init]

# If you like to use this action with mailing whois lines, you could use the composite action
# action_cf_mwl predefined in jail.conf, just define in your jail:
#
# action = %(action_cf_mwl)s
# Name of the jail in your jail.local file. default = [jail name]
# name = default

# Option: cfuser
# Notes.: Replaces <cfuser> in actionban and actionunban with cfuser value below
# Values: Your CloudFlare user/ email account

cfuser = user@mail.com

# Option: cftoken (Global API Key)
# Notes.: Replaces <cftoken> in actionban and actionunban with cftoken value below
# Values: Your CloudFlare API key 
cftoken = 
```

> config/guacamole/logback.xml
```
<configuration>
        <!-- Appender for debugging -->
        <appender name="GUAC-DEBUG" class="ch.qos.logback.core.ConsoleAppender">
                <encoder>
                        <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
                </encoder>
        </appender>
        <!-- Appender for debugging in a file-->
        <appender name="GUAC-DEBUG_FILE" class="ch.qos.logback.core.FileAppender">
                <file>/usr/local/tomcat/logs/guacd.log</file>
                <encoder>
                        <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
                </encoder>
        </appender>
        <!-- Log at DEBUG level -->
        <root level="debug">
                <appender-ref ref="GUAC-DEBUG"/>
                <appender-ref ref="GUAC-DEBUG_FILE"/>
        </root>
</configuration>
```

* Some fail2ban commands:
    - Enter fail2ban mode: 
    ```
    docker exec -it fail2ban /bin/sh
    fail2ban-client
    ```
    - Check the status of the jail: `fail2ban-client status guacamole-auth`
    - Check regex filter: `fail2ban-regex /var/log/guacamole/guacd.log /data/filter.d/guacamole-auth.conf`
    - Unban with: `fail2ban-client set guacamole-auth unbanip x.x.x.x`
    - Check iptables: `iptables -L <jail> --line-numbers`
    - Other commands [here](https://www.fail2ban.org/wiki/index.php/Commands)
