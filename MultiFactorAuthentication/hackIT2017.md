## Case 1: Bypassing 2FA in Recurly (Universal OAuth Bug)
* Cause of Vulnerability: Automatic login of users after password change without 2FA
#### Process Flow
* User requires a password change
* User requests a password reset token
* User changes password via the token
* Aplication  lets user log automatically after change
#### Abusive Scenario
* 2FA Enabled
* Attacker has victim's credentials
* Attacker logs in and is faced with a 2fa page
* Attacker requests password reset token
* Attacker changes the password and is directly logged in without 2FA

## Case 2: E-Wallet 2FA Bypass
* Cause of vulnerability: No verification of response on client end
#### Abusive Scenario
* Change response code with successful login response code


## Case 3: Paypal 2FA Bypass
* Cause of vulnerability: Secret question request manipulation
#### Process Flow
* One way for 2FA is text code
* Second way is secret questions
#### Abusive Scenario
* Attacker logs into account
* Attacker selects alternative option to login
* Attacker intercepts incorrect answers
* Attacker intercepts request with Burpsuite
* Attacker removes challenge and response fields
* Attacker grants access

## Case 4: RelateIQ 2FA Bypass
* Cause of Vulnerability: 2FA not enabled on 0Auth
#### Process Flow
* 2FA is enabled
* Login with email and password
* 2FA text code and access granted
#### Abusive Scenario
* Attacker compromises user's facebook credentials
* Attacker clicks on "Login via Facebook"
* Attacker is granted access to victim's account
* Without 2FA prompt
* 95% of webapps do not have 2FA enabled after 0Auth

## Case 5: Bypassing 2FA via voicemail
* Cause of vulnerability: Exploiting voicemail
#### Process Flow
* User logs in
* User requests 2fa code via call
* User gets a call from someone else at the same time
* User's 2fa code is sent to voice mail
#### Abusive Scenario
* Attacker logs into victim's account
* Attacker engages a call with the victim's phone number
* Attacker chooses the 2FA code via phone call option
* As the victim is engaged in the call by the attacker, the calling service will send the 2FA code to the victim's voicemail immediately
#### Exploiting Voicemail
* Obtain a CallerID via VOIP Provider or dedicated spoofing provider
* Enter the Caller ID to display as the victim's mobile number
* If you are using Spoofcard, a number and access code is displayed. Call this number and input the access code
* You will be connected to the victims voicemail service provider's endpoint. In this, input the victim's mobile number and press #
*
*

## Case 6: Bypassing Pattern Lock using ADB
* This option will only work when you have enabled USB Debugging previously on your device and your PC is allowed to connect via ADB. If you meet such requirements, it is ideal to use this method to unlock Samsung lock screen.
#### Abusive Scenario
* Connect your device to the PC using USB cable and open command propt in ADB directory. Type command "adb shell rm /data/system/gesture.key"

