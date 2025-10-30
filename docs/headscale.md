## Headscale docker setup:

### First run setup

You must configure an API key in order to access and manage your headscale server. 

You can create those using docker exec:

````bash
# create an api key
docker exec -it headscale headscale apikeys create
````

### Server Management 

#### Register Tailscale Exit Node with Headscale

Execute these commands once Headscale has been deployed:  

``` bash
sudo docker exec -it headscale headscale users create exit-node
sudo docker exec -it headscale headscale users list
```

List of users will be displayed showing their "ID" number:  

``` bash
ID | Name | Username  | Email | Created            
1  |      | exit-node |       | 2025-05-17 23:30:00
```

Create a PreAuthKey for "exit-node" with following command:  

``` bash
sudo docker exec -it headscale headscale --user 1 preauthkeys create
```

Output will display as:

``` bash
2025-05-18T09:46:34+10:00 TRC expiration has been set expiration=3600000
4f9e5c04a019273ef6356b3f4c173b2a896749e7364993f5
```

Add the authkey to `TAILSCALE_AUTH_KEY` in the `.env` file.  

Restart the Tailscale container:  

``` bash
sudo docker compose restart tailscale
```

Check Tailscale exit node has connected and registered with Headscale:  

``` bash
sudo docker exec -it headscale headscale nodes list
```

Check to see if the Tailscale exit node has registered the local / home subnet addresses with the Headscale server:  

``` bash
sudo docker exec -it headscale headscale nodes list-routes
```

List of routes for each host will be displayed showing their "ID" number:  

``` bash
ID | Hostname  | Approved | Available                                       | Serving (Primary)
1  | exit-node |          | 0.0.0.0/0, 192.168.1.0/24, 172.28.10.0/24, ::/0 |  
```

Enable IP routing out of the Tailscale exit node with the following command:  

``` bash
sudo docker exec -it headscale headscale nodes approve-routes --identifier 1 --routes "0.0.0.0/0,192.168.1.0/24,172.28.10.0/24,::/0"
sudo docker exec -it headscale headscale nodes list-routes
```

The IP routes will now be enabled and look like this:  

``` bash
ID | Hostname  | Approved                                        | Available                                       | Serving (Primary)  
1  | exit-node | 0.0.0.0/0, 192.168.1.0/24, 172.28.10.0/24, ::/0 | 0.0.0.0/0, 192.168.1.0/24, 172.28.10.0/24, ::/0 | 192.168.1.0/24, 172.28.10.0/24, 0.0.0.0/0, ::/0
```

### Register Mobile Tailscale Application with Headscale

You can now download the official Tailscale application, and when prompted to login, select a custom URL.  

Enter your home Headscale URL: [https://headscale.example.com](https://headscale.example.com)  

When you select connect, it will ask if you want to go to the URL, select Yes, then it will show a connection string like  

``` bash
headscale nodes register --user USERNAME --key 64LErdY2YcnMdNLNYc6wJJzE
```

We need to first create a user account, then register the Tailscale node against that account:  

``` bash
sudo docker exec -it headscale headscale users create alice
sudo docker exec -it headscale headscale nodes register --user alice --key 64LErdY2YcnMdNLNYc6wJJzE
```

The Tailscale will now automatically connect with the Headscale server, which can be checked with commands:  

``` bash
sudo docker exec -it headscale headscale users list
sudo docker exec -it headscale headscale nodes list
sudo docker exec -it headscale headscale nodes list-routes
```

You can now go to the Tailscale application on your phone, and select `Exit Node` --> `exit-node` and turn on `Allow Local Network Access`.  

You can also go into the Tailscale application settings on your phone, and turn on `VPN On Demand`, so you always have remote access when away from home.  

### WebUI Managed with Headplane

Headplane is a WebUI control for Headscale and is accessible at [https://headplane.example.com/admin/](https://headplane.example.com/admin/)    NOTE: "/" is needed at the end.  

You can generate an API key to connect Headplane to Headscale with:  

``` bash
sudo docker exec -it headscale headscale apikeys create --expiration 999d
```

The API Key can now be used in the Headplane portal:  

``` bash
xRYtN-G.frqhgHAC3jqLMbBqVTTRwAs2lWxSTeHr
```

The API Key can be stored in the Headplane configuration so its always used without prompting:

``` bash
vi headplane/config.yaml
```

Update this section:  

``` bash
  headscale_api_key: "xRYtN-G.frqhgHAC3jqLMbBqVTTRwAs2lWxSTeHr"
```

### Debugging and troubleshooting

Headscale and Tailscale provide debug and introspection capabilities that can be helpful when things don't work as
expected. This page explains some debugging techniques to help pinpoint problems.

Please also have a look at [Tailscale's Troubleshooting guide](https://tailscale.com/kb/1023/troubleshooting). It offers
a many tips and suggestions to troubleshoot common issues.

The Tailscale client itself offers many commands to introspect its state as well as the state of the network:

- [Check local network conditions](https://tailscale.com/kb/1080/cli#netcheck): `tailscale netcheck`
- [Get the client status](https://tailscale.com/kb/1080/cli#status): `tailscale status --json`
- [Get DNS status](https://tailscale.com/kb/1080/cli#dns): `tailscale dns status --all`
- Client logs: `tailscale debug daemon-logs`
- Client netmap: `tailscale debug netmap`
- Test DERP connection: `tailscale debug derp headscale`
- And many more, see: `tailscale debug --help`

### References

- [Headscale Docs](https://headscale.net/running-headscale-container/)
- [Headscale Github](https://github.com/juanfont/headscale)
- [Headscale debugging](https://headscale.net/stable/ref/debug/#tailscale)
- [Headplane Docs](https://headplane.net/configuration)
- [Headplane Github](https://github.com/tale/headplane/)
- [Headscale/Headplane Docker Example](https://github.com/geekau/mediastack)
- [Headscale/Headplane Docker Example 2](https://github.com/madaha668/self-hosting-ts/blob/main/WAN/README.md)
- [Headscale/Headplane Docker Example 3](https://www.lucasjanin.com/2025/01/03/headscale-tailscale-ios-installation/)
- [Keycloak OIDC info](https://stackoverflow.com/questions/28658735/what-are-keycloaks-oauth2-openid-connect-endpoints/30449500#30449500)
