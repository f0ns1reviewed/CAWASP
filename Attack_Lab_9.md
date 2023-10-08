# Attack Lab 9

```
-Find and exploit the File Upload vulnerability and execute OS Level command on the contact application (https://contactpharmacorp.azurewebsites.net/)
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
