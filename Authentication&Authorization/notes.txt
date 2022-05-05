## Case 1: Misuse of Auth Token
* Valid Credential --> Login Status = Success and Auth Token Returned
* Wrong Credential --> Login Status = Failed and Blank Auth Token Returned

#### Attacking Scenario
* Attacker submits valid credential from valid account
* Intercepts the reponse of login request, copies the auth token present in the response and then drops the response
* Sends the login request with Target user ID + wrong password
* Intercept the response, change Login Status = Success and Copy the Auth Token copied earlier from attacker's account

**Attacker gets access to Victim's Account**

