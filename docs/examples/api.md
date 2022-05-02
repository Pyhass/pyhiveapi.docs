---
title: API
sidebar: example
---
# API Examples

Below are examples on how to use the library independently with the API.

## Log in - Username and Password with MFA (If Required)

Below is an example how to log in to Hive with 2FA if needed
and get a session token.

```Python
import pyhiveapi as Hive

tokens = {}
hive_auth = Hive.Auth(<Hive Username>", "<Hive Password>")
authData = hive_auth.login()

if authData.get("ChallengeName") == "SMS_MFA":
    code = input("Enter your 2FA code: ")
    authData = hive_auth.sms_2fa(code, tokens)


device_data = hive_auth.getDeviceData()
print(device_data)

if "AuthenticationResult" in authData:
    session = authData["AuthenticationResult"]
    tokens.update({"token": session["IdToken"]})
    tokens.update({"refreshToken": session["RefreshToken"]})
    tokens.update({"accessToken": session["AccessToken"]})

```


## Log in - Using Device Authentication

Below is an example how to log in to Hive with 2FA if needed
and get a session token.

```Python
import pyhiveapi as Hive

tokens = {}
hive_auth = Hive.Auth(<Hive Username>", "<Hive Password>", "Hive Device Group Key>", "<Hive Device Key>", "<Hive Device Password>")
authData = hive_auth.deviceLogin()

if "AuthenticationResult" in authData:
    session = authData["AuthenticationResult"]
    tokens.update({"token": session["IdToken"]})
    tokens.update({"refreshToken": session["RefreshToken"]})
    tokens.update({"accessToken": session["AccessToken"]})
```



## Refresh Tokens

Below is an example how to refresh your session tokens
after they have expired

```Python
import pyhiveapi as Hive

tokens = {}
hive_auth = Hive.Auth(<Hive Username>", "<Hive Password>", "Hive Device Group Key>", "<Hive Device Key>", "<Hive Device Password>")
authData = hive_auth.deviceLogin()
newTokens = hive_auth.refreshToken(tokens['AuthenticationResult']['RefreshToken'])

if "AuthenticationResult" in newTokens:
    session = newTokens["AuthenticationResult"]
    tokens.update({"token": session["IdToken"]})
    tokens.update({"refreshToken": session["RefreshToken"]})
    tokens.update({"accessToken": session["AccessToken"]})
```

## Get Hive Data - Using Tokens

Below is an example how to data from the Hive platform
using the session token acquired from login.

```Python
api = Hive.HiveApi()
data = api.getAllData(tokens["token"])
```
