## How to create Trakt.tv API app

> * Create a new app https://trakt.tv/oauth/applications/new
> * Name it and set `urn:ietf:wg:oauth:2.0:oob` as the redirect uri.  Also select both permissions (I'm not sure which one is actually relevant)
> * Get your client_id, and go [here](https://trakt.docs.apiary.io/#reference/authentication-devices/device-code/generate-new-device-codes), then either click on "Switch to Console" or "Try". Select body and replace the string here with your own client_id, send the query with "Call Resource"
> * Go to https://trakt.tv/activate and use the `user_code` from the 200 response from the previous query
> * Go to https://trakt.docs.apiary.io/#reference/authentication-devices/get-token/poll-for-the-access_token?console=1 and use the `device_code` from the previous query alongside the `client_id` and `client_secret` from your app to finally fetch the `access_token`
