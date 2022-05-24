## File Upload and Its Main Headers
* Filename
* Content Length
* Content Type
* Magic Number
* File Content

## Scenario 1: Blacklisting
#### Process Flow
* Attacker tries to upload .php, .php2, .php3, .phtml, etc.
* Server have function preg_match (php code) to match php, php2, php3 and phtml, etc.
* But, blacklist check is not case-sensitive. What about upper case?

#### Attacker Flow
* Attacker uploads .PHP, .PHP2, .PHTML
* It works and shell uploads successfully.
* Devs don't know every possible extension that can execute php code. Like, pht extension can also execute php code.
* This is useful if case sensitivity is also checked. Try with .pht extension.

## Scenario 2: IIS-Windows
#### Prcess Flow
* Attacker tries to upload .asp, .aspx code for shell upload
* Server blacklists .asp, .aspx, etc.
* Try uppercase .ASP, .ASPX
* For IIS <=7.5, .asp and .cer extensions can bypass file upload protection. As, .cer and .asp is linked to asp.dll

## Scenario 3: Bypass all malisc. extensions
* There is an extenion .eml which is used in Mozilla Thunderbird for emails.
* This can be used to trigger Stored XSS. If uploaded file is saved on app.
* Only works on Internet Explorer.
* Reference: https://github.com/gogs/gogs/issues/5397

## Scenario 4: Whitelisting (Validating Filename only)
#### Process Flow:
* Server whitelists and validates only those files which have .jpg, .png, .mp3, .pdf, etc.
* But not checking if it ends with .jpg, .png, etc.
#### Attacker Flow:
* Upload file: payload.jpg.php
* This bypasses the check as it contains .jpg but it ends with .php so it will execute php code.

## Scenario 5: Null Byte Injection
* Null charater is a control character with the value zero.
* PHP treats null bytes as terminator like C does.
* Filename: shell.php%001.jpg
* Filename: shell.php\x00.jpg
* Both files should satisfy the file upload page because the file ends with .jpg
* But, the file will be treated as .php due to termination after Null Byte.
* Tip: rename the file as shell.phpD.jpg, upload it and then replace the hex representation of D with 00 in Burpsuite.

## Scenario 6: If application allows upload of .svg images
* SVG images are XML data
* Using XML, lot of bugs can be achieved
* Like, stored XSS, XXE, etc.
* Check any POC on stored XSS using SVG file upload

## Scenario 7: Allowing Video Uploads?
* Due to a SSRF vulnerability in ffmpef library
* It is possible to create a video file that when uploaded to application that supports video files like youtube, flickr, etc.
* You will be able to read files from that server when you try to watch the video
* Exploit Tool: https://github.com/neex/ffmpeg-avi-m3u-xbin

## Scenario 8: Directory Tranversal
* Suppose the logo of the webapp is at /app/home-logo.jpg
* Uploaded images are at the folder /app/uploads/image1.jpg
* All checks are there so, attacker cannot upload shell
* Upload file with name: ../home-logo.jpg
* This is a valid file valid jpg file, with valid content.
* But due to directory traversal, server may upload the file at /app/ directory
* And, boom main webapp logo got updated.

## Scneario 9: Validating the file content and missing the file name
* https://secgeek.net/bookfresh-vulnerability

## Scneario 10: Image Tragic Attack
* SVG Images are just XML attack.
* Image magick, an image processing library is vulnerable to SSRF and RCE
* Source: https://4lemon.ru/2017-01-17_facebook_imagetragick_remote_code_execution.html

## Scenario 11: Exploiting old IIS Servers
* IIS in its earlier versions < 7.0 had an issue handling the uploaded files
* An attacker can bypass the file upload pages using filename as
* Filename: shell.aspx;1.jpg

## Scenario 12: DOS Attack
* Web app that don't validate the file size of uploaded file are vulnerable to DOS attack
* An attacker can upload many large files that will exhaust the server hosting space
* 100 files - 20MB each

## Scenario 13: Magic Numbers and RCE via Zip files
#### Process Flow
* Devs validate the file-content starts with Magic Number and the file content is set to image/gif.
#### Attacker Flow
* Upload shell.php but setting the content type to image/gif
* And start the file content with GIF89a.
* Shell succesfully uploaded!

## Scenario 14: Out of Band SQL Injection via filename
* If the devs trust the filenames and pass it directly to the Database.
* This will allow attackers to execute Out of Band SQLi
* Does not work on Php and mysql

## Techniques for better handling of file upload pages
* Always use a sandbox domain to store uploaded files.
* Use CDN servers as it only allows cacheable resources and disable executable files such as php
* Rename the uploaded files to some random filename, remove the file extension and then append your allowed file extension
* Mark all files as downloadable not executable (Content-Deposition)
* Validate file size
