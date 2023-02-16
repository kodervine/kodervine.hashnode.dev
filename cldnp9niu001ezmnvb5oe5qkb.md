# FirebaseError: auth/admin-restricted-operation

## Problem

I was working on firebase authentication that creates a new user with email and password and came across this error message -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675088786601/ede772cf-bd32-4d4d-b6f3-53bc91b1914c.png align="center")

## Solution

It might be tempting to use an anonymous sign up authentication for your prospective users to access the platform as this [anonymous sign-in](https://firebase.google.com/docs/auth/web/anonymous-auth) enables users to access Firebase resources without having to register for an account, sign in using a conventional email address and password, or use a third-party provider. The user is given a valid authentication token that may be used to access Firebase resources when they sign in anonymously, and Firebase creates a temporary anonymous user ID that is persistent between sessions.

Although anonymous authentication may work temporarily, the issue will arise later when they can't log in with the email and password used for sign-up, because anonymous authentication doesn't require email and password.

**The root cause** for this error is not sending the email and password correctly, which is why email/password authentication is only allowed for administrators. The solution is to verify the `sign-in data`, not enable anonymous authentication.

So, what you need to do is check your function handling the signup.

In my case, this is what happened:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675089165637/b598ae6c-757e-4158-a998-90f261c746cd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675089176880/f01aeaed-e6f1-4779-942e-ada534a47909.png align="center")

My `handleSignUpFormSubmit` was trying to access the `userSignUpForm` state value. However, it was not properly calling the `key` in the object properly.

So, instead of `userSignUpForm.signupEmail`, I was calling `userSignUpForm.email.`

Here is the updated version that got rid of this bug for me in the `handleSignUpFormSubmit` function:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675089392569/87e880da-f702-46c9-81b0-813bbd0fec4b.png align="center")

## Conclusion

I hope that the information provided has helped resolve the "auth/admin-restricted-operation" error in your Firebase project. By carefully checking your authentication data and permissions, you can ensure that your app is functioning smoothly and securely.