# Guest Accounts

Every frontend user can shop without registering. Later, in the marketplace solution, merchants also can sell without registering. This article discusses technical aspects and necessary limitations for such users.

{{ toc }}

## Guest Accounts

User will be identified using a non-expiring `GUEST` cookie. After the cookie is installed on the user's machine, with every request we'll know it is her.

Installing such a cookie automatically is bad for user's privacy. It also violates GDPR. GDPR requires to receive user's consent before installing such a cookie. Consent should be documented and stored, can be revoked by user later.

## Entering Private Pages

Initially user browses as "not logged in" and as such, can only visit public pages. All the functionality on these pages should work without an account. For example user's cart, language or other preferences should be stored in user's session. 

Private pages such as checkout and customer area are restricted. When a "not logged in" user hits a private page, she is redirected to the Login page that also have "Or" submenu with "Register", "Login with Facebook" and "Login as guest".

The latter action opens a `GET /account/guest` page explaining that the account will only be accessible from the device the user is currently using, that is will be automatically be deleted after one year of not using the service and that the account may also become inaccessible if the user clears the browser's cookies. User can delete the guest account or transform it into a full-fledged registered account later any time. 

After user presses "Agree and proceed", the `GUEST` cookie is installed and the authentication advice on the server uses it to access the user's account record.

Like after a normal login, user's cart and preferences from the current session are merged into the account's cart and preferences.

## Dealing With No Contact Information

Guest users may provide no contact information (email, phone or other). It is user's choice. However, when the system should normally notify the user about new order or something else and there is no contact information, the system should show a warning to the user that she won't receive an email as no email address is specified. User can also opt out from the further similar warnings in the future and enable them later in her account settings.

For certain actions, such as contact form, leaving a contact email or other information may be required. The system should ensure such information is entered. 

BTW, anytime a user provides an email, it should go through the email verification workflow.
 
## Logout

When logging out from the guest account (which doesn't happen automatically), we update the `logged_out` flag in the customer account record. 
 


