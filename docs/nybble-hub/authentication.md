# Authentication
Access to Nybble services is secured by Single Sign On, based on Auth0 services.

## Onboarding process
3 easy steps to get your account up and ready:

1. Account creation by Nybble
2. Set your password
3. Enroll a 2nd factor for stronger authentication (Multi Factor Authentication)

After step 1, you will receive 2 mails:

- welcome mail which summarizes the onboarding process
- password reset mail, which gives you a link to set your password

## Multi Factor Authentication (MFA)
To complement password authentication, Nybble has activated Multi Factor Authentification (MFA) using a smartphone as 2nd factor.  
You will need an Authenticator app such as:

- Google Authenticator ([Google Play](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2) / [App Store](https://itunes.apple.com/us/app/google-authenticator/id388497605)).
- Microsoft Authenticator ([Google Play](https://play.google.com/store/apps/details?id=com.azure.authenticator) / [App Store](https://itunes.apple.com/us/app/microsoft-authenticator/id983156458))

Enrollment will be done at 1st login: a QR code will be prompted, you can scan it with one of the listed app.

Finally, after a successful login with your password, you will be prompted for a one-time code, coming from your smartphone.  

## How to reset your password?

If you've lost / forgotten your password, you can reset it from login page:

1. Connect to [Nybble Hub](https://hub.nybble-analytics.io) then try to login as usual
2. On the login page, select "forgot password"<br>
  ![Forgot Password](../img/nybble-hub/auth0-forgot.png){: style="width:400px;"}
3. Provide your mail and the platform will send you a link and full instructions to reset your password.

