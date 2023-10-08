# Attack Lab 1

## Enumeration

## Azure subdomains enumeration:

Edit permutations dictionary file:

```
PS C:\Users\studentuser107> type C:\AzAppsec\Tools\MicroBurst\Misc\permutations.txt
$root
$web
access-logs
accounting
api
assets
azure
azure-logs
chef
client
conf
confidential
config
configuration

...
```
After include resources and contact words, execute enumeration script.
```
Import-Module C:\AzAppsec\Tools\MicroBurst\Misc\Invoke-EnumerateAzureSubDomains.ps1
Invoke-EnumerateAzureSubDomains -Base pharmacorp -Verbose
VERBOSE: Found resourcespharmacorp.scm.azurewebsites.net
VERBOSE: Found contactpharmacorp.scm.azurewebsites.net
VERBOSE: Found pharmacorp.onmicrosoft.com
VERBOSE: Found pharmacorp.mail.protection.outlook.com
VERBOSE: Found resourcespharmacorp.azurewebsites.net
VERBOSE: Found contactpharmacorp.azurewebsites.net
VERBOSE: Found pharmacorp.sharepoint.com
VERBOSE: Found pharmacorp-my.sharepoint.com
VERBOSE: Found pharmacorp-web.sharepoint.com

Subdomain                                 Service
---------                                 -------
contactpharmacorp.azurewebsites.net       App Services
resourcespharmacorp.azurewebsites.net     App Services
resourcespharmacorp.scm.azurewebsites.net App Services - Management
contactpharmacorp.scm.azurewebsites.net   App Services - Management
pharmacorp.mail.protection.outlook.com    Email
pharmacorp.onmicrosoft.com                Microsoft Hosted Domain
pharmacorp-web.sharepoint.com             SharePoint
pharmacorp-my.sharepoint.com              SharePoint
pharmacorp.sharepoint.com                 SharePoint

```
