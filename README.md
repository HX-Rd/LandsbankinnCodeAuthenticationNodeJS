LandsbankinnCodeAuthentication
==========

This repo is a modified version of [Gatekeeper](https://github.com/prose/gatekeeper) so all credit goes to them for creating this.   
However, landsbankins authentication server works a little differently, so the code is modified to accommodate this change.    

## API

```
GET http://localhost:9999/authenticate/TEMPORARY_CODE
```

## OAuth Steps

1. Redirect users to request Landsbankinn access.

   ```
   GET https://authsandbox.landsbankinn.is/as/authorization.oauth2
   ```

2. Landsbankinn redirects back to your site including a temporary code you need for the next step.

   You can grab it like so:

   ```js
   var code = window.location.href.match(/\?code=(.*)/)[1];
   ```

3. Request the actual token using your instance of Gatekeeper, which knows your `client_secret`.

   ```js
   $.getJSON('http://localhost:9999/authenticate?code='+code+ '&redirect_uri=' + redirecturl, function(data) {
     console.log(data.token);
   });
   ```

## Setup your LandsbankinnCodeAuthenticationNodeJS

1. Clone it

    ```
    git clone git@github.com:hx-rd/LandsbankinnCodeAuthenticationNodeJS.git
    ```

2. Install Dependencies

    ```
    cd LandsbankinnCodeAuthenticationNodeJS && npm install
    ```

3. Adjust config.json

   ```json
   {
     "oauth_client_id": "LANDSBANKINN_APPLICATION_CLIENT_ID",
     "oauth_client_secret": "LANDSBANKINN_APPLICATION_CLIENT_SECRET",
     "oauth_host": "authsandbox.landsbankinn.is",
     "oauth_port": 443,
     "oauth_path": "/as/token.oauth2",
     "oauth_method": "POST",
     "port": 9999
   }
   ```

   You can also set environment variables to override the settings if you don't want Git to track your adjusted config.json file. Just use UPPER_CASE keys.

4. Serve it

   ```
   $ node index.js
   ```

## Deploy on Azure

### Azure Button

Use the button below to instantly setup your own Gatekeeper instance on Azure.

[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)

### Azure manually

1. Create a new Azure site

   ```
   azure site create SITE_NAME --git
   ```

2. Provide OAUTH_CLIENT_ID and OAUTH_CLIENT_SECRET:

   ```
   azure site appsetting add OAUTH_CLIENT_ID=XXXX
   azure site appsetting add OAUTH_CLIENT_SECRET=YYYY
   ```

3. Push changes to Azure

   ```
   git push azure master
   ```
