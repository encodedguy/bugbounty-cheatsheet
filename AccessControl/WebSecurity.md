# Access Control Vulnerabilities (Notes)
#### Notes are made form Web Security Path of Port Swigger
#### Few points are from my side, hope to see some commits from other researchers in future.
#### Contribute and add more things to this file to help others.

## Security by Obscurity
* When admin panels are hidden by making `/admin-endpoint-12xcbs` unpredictable and non-discoverable even using Content Discovery tools.
* Find such endpoints in javascript files, try bruteforcing javascript files
* Use assetnote wordlists for js, or append `.js` in `Seclists/directory-2.3-medium.txt` wordlist
* Maybe visible in `robots.txt` or `sitemap.txt` (Wayback machine) -- @tip-not-by-me @read-2-hours-ago @tip-author-can-edit-this-line

## Parameter Based Access Control
* When the user is admin or not, is controlled by parameter or cookies in requests
* `/user?role=admin /user?admin=true /user?admin=1 /user?roleid=2 /user?isAdmin=true`
* A tip to find the parameter name, check the parameters present in the response from /user API requests or similar.
* `/user`, `/session`, `/profile`, etc. endpoints reveal details about user and also contains details about your roles.
* Just add the parameter in some `PUT` or `POST` request while updating user information and append the parameter in the request.

## Platform Misconfiguration
* Some platforms disallow access by URLs and HTTP Methods.
* `DENY: POST, /admin/deleteUser, managers`
* You can override such rule using non standard HTTP headers like `X-Original-URL` and `X-Rewrite-URL`.
* This will work if the application allows the URL to be overridden via a request header.
* Like, the back-end application is built on a framework that supports the X-Original-URL header. 
* Change the method to POST, PUT, or HEAD to see responses as different access roles.

## Horizontal Privilege Escalation
* When you are able to access other users of the same privilege that you have.
* You are a manager with `id=1234`, you increment it to `id=1236` and you are able to access other manager.
* So, same role but escalated to different user which you are not authorised to.
* IDs could be GUIDs/UUIDs (GLobally Unique Identifier) which are 32 digits long and could be time-based, MAC address based or randomly made.
* Like this, `id=1970e249-2f9a-462d-9b36-5e944c449d92`
* To find other user's GUID, access other user's posts, comments, public profiles.
* These posts, comments, and profiles must have some notion about other user's id in source code.

## Vertical Privilege Escalation
* Almost same as horizontal escalation, but here the privileges gained are more
* You change the `userid=123` to `userid=1` and `userid=1` is an admin. You gained permission to delete other users (++ privileges)

## IDOR (Insecure Direct Object Reference)
* It is a subclass of access control vulnerabilities
* Most of the programming languages supports OOP and there are objects which define particular things like `Chat`, `Profile`, `Cart`, etc.
* These objects have attributes and methods. Attributes are variables like `chatid`, `profileid`. Methods are like `getProfile()`, `addToCart()`, etc.
* Methods display attributes or change attributes.
* All these methods for a particular object requires access controls, so that methods for UserA cannot be accessed by UserB or anonymous user (worst case).
* You are downloading your information from Instagram, using your email
* Request looks like this `/download-information?username=encodedguy&email=attacker@attacker.com` (Just an example !!)
* Attacker changes this to another username like `rashahackx` and gain access to other user's Instagram information
* Change fileid or numbers while exporting files or while uploading files, see the HTTP history and grab the file number or fileid.
* Change it to some other file id and you may expose sensitive data of other file. (Reported Last Year -- $$$ -- rashahacks.com)


## Access Control Issue in Multi-Step Functions
* User goes to cart --> add address -->  add payment method --> checkout --> ordered
* Developer thought that User will reach checkout function only after completing previous requests.
* Developer missed a check on checkout request
* Attacker made a request to `/checkout` directly before going to `/add-payment method`, and bypassed payment portal requests.

## Referer Header Based Access Control
* The Referer header is generally added to requests by browsers to indicate the page from which a request was initiated. 
* Request for /admin/deleteUser only inspects the Referer header. If the Referer header contains the main /admin URL, then the request is allowed. 
* So, a normal user can forge the Referer Header to /admin or some admin's Referer Header's value and use the admin's functionalities.
