# AUSCERT 2018: How to Bypass Authentication and Authorization

## Case 1: Misuse of Auth Token
* Valid Credential --> Login Status = Success and Auth Token Returned
* Wrong Credential --> Login Status = Failed and Blank Auth Token Returned

#### Attacking Scenario
* Attacker submits valid credential from valid account
* Intercepts the reponse of login request, copies the auth token present in the response and then drops the response
* Sends the login request with Target user ID + wrong password
* Intercept the response, change Login Status = Success and Copy the Auth Token copied earlier from attacker's account

**Attacker gets access to Victim's Account**

## Case 2: Cookie Manipulation
* Successful Login --> App sets session cookie --> contains UserID

#### Attacking Scenario
* Attacker logs in to the app
* Using a cookie editor, replaces the UserID value present in the session cookie or any other cookie with that of victim's UserID and refreshes the page

**Attacker gets access to Victim's Account**

## Case 3: Session Invalidation
* Multi-Login Allowed and Session Not Expired After Password Reset (> 3 months)

#### Attacking Scenario
* Login Into Multiple Devices
* Change the password using password reset on one device
* Password is only changed on this device and other devices are still logged in
* If other device's password don't get changed then if the device gets compromised, attacker gets lifetime access of victim's account

**Session should be invalidated in other devices if password is changed on one device**

## Case 4: Account Takeover by Forgot Password Functionality
* Forgot password gives option to select recovery option (OTP/email reset link)
* Part of the email and number are masked with asterisk (number/email)

#### Attacking Scenario
* Attacker provides victim's UserID in forgot password page
* Select any recovery option and using proxy tool modify the OTP/email option value in request
* Just change that email or number, OTP will be provided to attacker's device but password will be changed of victim
* Generally present where masking on number/email is there

**Attacker receives recovery OTP/email and compromises the victim's account**

## Case 5: Authentication Bypass in Mobile App
* On successful login, user session (sessionID) was stored in local /AppData folder (sqlite db)

#### Attacking Scenario
* After analyzing the sqlite db, session id in the stored session was triple base64 encoded or something else
* Attacker encodes the victim's username thrices or some other encoding present and pushed in the device's /AppData folder
* Opens the app in device
* Server thinks that user is logged in with victim's account as victim's session is present

**Attacker gets logged in to victim's account**
**Such sessions should not be stored on local folders and session values should be changed for each request**

## Case 6: Boolean Based Privileges
* Application has only two privileges: admin and normal
* Keeps track of privilege by boolean parameter (isAdmin=true/false)
* Admins have additional content which are unavailable for normal user

#### Attacking Scenario
* Attacker who is Normal user logs in with valid credential
* In response body, make the Boolean value as true

**Normal user gets admin privilege**

## Case 7: Role Based Privilege
* Application has more than two privileges
* Keeps track of privilege by integer numbers (0, 1, 2) or (role=manager/admin/user)
* Roles stores in cookie

#### Attacking Scenario
* Attacker logs in as low privilege user
* Edits the cookie and changes the role value

**Attacker escalates the privileges**

## Case 8: GUI Based/Client Side Privilege Escalation
* Based on different roles, the application disables/hides privileged features from low privilege users
* Disabling/ Hiding HTML Elements on client side

#### Attacking Scenario
* Inspect that element, remove rules enforced like hidden, disabled
* Bypass GUI Based/Client Side Privilege Enforcement

**Attacker escalates the privilege and perform disabled/hidden operations**

## Case 9: Design FLAW - user sends spam mail to anyone
* Normal User can send mails only to team members who are in recepient list
* Only admins can send mails to different teams

#### Attacking Scenario
* Using proxy tool, user can modify recipient address to any non-team member's address
* Also, can change sender's mail ID.

**Attacker can send mail to anyone on behalf of someone else**

## Case 10: Deleting Project by IDOR
* A normal user who is not a owner can only view a project
* A user can be normal user for one project and admin for others
* No delete option for normal user

#### Attacking Scenario
* Normal user views the project and copies the ProjectID from URL
* Normal user himself creates a project for which he/she becomes owner
* Now for his own project, delete option is available
* Clicks delete and change the ProjectID with that of previous project

**Normal User deleted a project for which he/she is not a owner but a normal member**

## Remediation - Authentication Bypass
* Mapping of Auth Token and UserID
* No guessable UserID in cookie
* Proper Session Invalidation (with multiple logins)
* Strong password reset functionality
* Response body should not contain debugging error messages
* AppData contains sensitive information

## Remediation - Authorization Bypass
* Map user role with session ID
* No GUI Based/ Client Side Privilege Implementation
* Proper Access Control Mechanism
* No IDOR
* Server Side Validation/ Business Logic Validation
