# Redirect Users with Precision Using React Router's useLocation Hook

`useLocation` is a hook provided by the React Router library that allows you to access the current URL location in your application. It returns an object that contains information about the current URL, including the pathname, and search query parameters.

With this information, you can navigate your users to different pages, based on where they came from or where they need to go.

## Why is the hook important?

To put this in context, let's say you're building an invoice application application, and you want to make it easy for users to navigate the app. You might have a button on your homepage that takes users to a page where they can preview all their invoices.

But what happens when they've already started creating an invoice and navigate away from the page or log out? How do you make it easy for them to come back and finish what they started?

This is where `useLocation` comes in. When you use the `location` object, react then helps you keep track of where the user came from and where they need to go next.

For example, you could use the `state` property of the `location` object to store information about the user's previous page, like this:

```javascript
import { useLocation, useNavigate } from 'react-router-dom';

function InvoiceForm() {
  const location = useLocation();
   const navigate = useNavigate();

  function handleNavigateUser(page) {
    navigate(page, { state: { from: location.pathname } });
  }

  // Rest of the component code here
}
```

In this example, the `useLocation` hook is used to get the information about the user's current URL location. Then the `useNavigate` hook provided by React Router is used to navigate the user to a new page. We're also passing in a state object with information about the user's previous page, so that we can redirect them back to it if needed.

This way, `useLocation` can help make navigating a website or app feel more intuitive and seamless. It's like a GPS for digital platforms, guiding users to where they need to go and keeping track of where they've been. And just like a GPS, it can make the journey feel smoother and less confusing.

## UseCase with Google sign in

Let's say your user is trying to sign into their application, but they've been logged out. And you want to redirect them to their intended page after they input their details. Here is how you can implement this:

### Pre-requisites:

1. In-depth understanding of firebase user authentication
    
2. Understanding of React
    
3. Knowledge of React Protected Routes
    

If you are not familiar with protected routes and routing, you can [read this article](https://kodervine.hashnode.dev/how-to-prevent-react-protectedroutes-from-redirecting-to-login-on-page-reload).

### Step 1: Create a Protected Route

Create a protected route that only logged-in users can access. Create a file named `ProtectedRoutes.js` in your src folder and paste in the following code:

```javascript
import React from "react";
import { Outlet, useNavigate, useLocation } from "react-router-dom";

const ProtectedRoutes = () => {
  const location = useLocation();
  return localStorage.getItem("isUserSignedIn") ? (
    <Outlet />
  ) : (
    <Navigate to="signin" replace state={{ from: location }} />
  );
};

export default ProtectedRoutes;
```

In this code, we're using the `useLocation` hook from React Router to get the current URL location. If the user is not signed in, we redirect them to the `signin page` with the `location state` set to the `current location.`

### Step 2: Create Google user authentication Component

Next, create a Sign-In component that allows users to sign in with their Google accounts.

```javascript
// Google Sign in
  const gmailProvider = new GoogleAuthProvider();
  const handleUserSignInWithGoogle = async () => {
    try {
      // Sign in with Google
      const res = await signInWithPopup(auth, gmailProvider);
      const googleUser = res.user;

      // Check if the Google user email is already in use in Firebase
      const q = query(collection(db, "users"));
      const docs = await getDocs(q);
      let existingUser = null;
      docs.forEach((item) => {
        if (item.data().email === googleUser.email) {
          existingUser = item;
        }
      });

      // If the Google user email is already in use, merge the two users
      if (existingUser) {
        const userRef = doc(db, "users", existingUser.id);
        updateDoc(userRef, {
          uid: googleUser.uid,
          name: googleUser.displayName,
          authProvider: "google",
          email: googleUser.email,
        });
        if (location.state?.from) {
          navigate(location.state.from);
        } else {
          handleNavigateUser("create-invoice");
        }

        setTimeout(() => {
          alert("Login successful");
        }, 500);
      } else {
        // If the email is not in use, create a new user
        addDoc(collection(db, "users"), {
          uid: googleUser.uid,
          createdAt: serverTimestamp(),
          name: googleUser.displayName,
          authProvider: "google",
          email: googleUser.email,
          password: "",
          invoiceData: [],
        });
        alert("Account created successfully");
         if (location.state?.from) {
          navigate(location.state.from);
        } else {
          handleNavigateUser("create-invoice");
        }
      }
      // await
    } catch (error) {
      if (error.code == "auth/email-already-in-use") {
        alert("The email address is already in use");
      } else if (error.code == "auth/invalid-email") {
        alert("The email address is not valid.");
      } else if (error.code == "auth/operation-not-allowed") {
        alert("Operation not allowed.");
      } else if (error.code == "auth/weak-password") {
        alert("The password is too weak.");
      }
    }
  };
```

**Code explanation**

1. `const gmailProvider = new GoogleAuthProvider();` initializes a new instance of the GoogleAuthProvider class from the Firebase Auth library. This object is then used to authenticate a user with their Google account.
    
2. `const handleUserSignInWithGoogle = async () => {...}` is an asynchronous function that handles the Google sign-in process. When a user clicks the "Sign in with Google" button, this function is called.
    
3. `const res = await signInWithPopup(auth, gmailProvider);` initiates the Google sign-in process by opening a popup window that prompts the user to sign in with their Google account. If the user successfully signs in, the `res` object contains information about the user, including their email address, name, and unique identifier.
    
4. `const q = query(collection(db, "users"));` creates a query to retrieve all user documents from the Firestore database.
    
5. `const docs = await getDocs(q);` retrieves all documents that match the query created in step 4 and stores them in the `docs` array.
    
6. `let existingUser = null;` initializes a variable called `existingUser` to `null`.
    
7. `docs.forEach((item) => {...})` loops through each document in the `docs` array.
    
8. `if (`[`item.data`](http://item.data)`().email ===` [`googleUser.email`](http://googleUser.email)`) {...}` checks if the email address associated with the current Firestore document matches the email address of the Google user who just signed in.
    
9. `existingUser = item;` if a Firestore document is found that matches the Google user's email address, the `existingUser` variable is set to that document.
    
10. `const userRef = doc(db, "users",` [`existingUser.id`](http://existingUser.id)`);` creates a reference to the Firestore document that matches the Google user's email address.
    
11. `updateDoc(userRef, {...})` updates the Firestore document with the Google user's information, including their unique identifier, name, email address, and authentication provider.
    
12. `if (location.state?.from) {...}` checks if the `from` state variable is set, which indicates that the user was redirected to the sign-in page from another page in the app.
    
13. `navigate(location.state.from);` redirects the user back to the previous page they were on before clicking the "Sign in with Google" button.
    
14. `addDoc(collection(db, "users"), {...})` creates a new Firestore document for the Google user, which includes their unique identifier, name, email address, password, and invoice data.
    
15. `if (location.state?.from) {...}` checks if the `from` state variable is set.
    
16. `navigate(location.state.from);` redirects the user back to the previous page they were on before clicking the "Sign in with Google" button.
    
17. The `catch` block handles any errors that occur during the sign-in process, including email already in use, invalid email, operation not allowed, and weak password. Depending on the type of error, an appropriate alert message is displayed to the user.
    

## Conclusion

And that's it! With just a few lines of code, you can integrate redirection functionality into your React application and provide a smooth and seamless user experience with the `useLocation` hook. By giving you access to the current URL location, it can help you create more intuitive and seamless navigation flows, keeping users on track on the website.