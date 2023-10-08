# CAWASP
Certification

## Certification Tools:

```
PS C:\AzAppsec\Tools> dir


    Directory: C:\AzAppsec\Tools


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----         7/1/2022   3:48 AM                AADInternals
d-----         7/1/2022   3:48 AM                Az
d-----         7/1/2022   3:48 AM                AzureAD
d-----         7/1/2022   3:48 AM                CosmosDB
d-----         7/1/2022   3:48 AM                MicroBurst
d-----         7/1/2022   3:48 AM                PowerZure
-a----         7/1/2022  11:25 AM       57475072 azure-cli-2.37.0.msi
-a----         7/1/2022  11:13 AM      244359176 burpsuite_community_windows-x64_v2022_5_2.exe
-a----         7/1/2022  11:24 AM      101020440 StorageExplorer.exe
-a----        6/17/2022   2:42 PM            667 studentx.jsp

```

### Enumerate Azure subdomains (pharmacorp):

```
Import-Module C:\AzAppsec\Tools\MicroBurst\Misc\Invoke-EnumerateAzureSubDomains.ps1
Invoke-EnumerateAzureSubDomains -Base pharmacorp -Verbose
VERBOSE: Found pharmacorp.onmicrosoft.com
VERBOSE: Found pharmacorp.mail.protection.outlook.com
VERBOSE: Found pharmacorp.sharepoint.com
VERBOSE: Found pharmacorp-my.sharepoint.com
VERBOSE: Found pharmacorp-web.sharepoint.com

Subdomain                              Service
---------                              -------
pharmacorp.mail.protection.outlook.com Email
pharmacorp.onmicrosoft.com             Microsoft Hosted Domain
pharmacorp-web.sharepoint.com          SharePoint
pharmacorp-my.sharepoint.com           SharePoint
pharmacorp.sharepoint.com              SharePoint

```
