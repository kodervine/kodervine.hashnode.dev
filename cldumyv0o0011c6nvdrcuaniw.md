# Preventing Duplicate User Information with Firebase Google Sign-in

## Introduction

Implementing user authentication can be complex, especially when integrating multiple sign-in methods like Google. You may encounter challenges such as handling the flow between creating a new account and logging in, or merging existing users with different sign-in methods.

In this article, I will walk you through code snippet that solves these problems, and we'll also explain how it works so you can better understand how to create a robust authentication process for your own app.

## Pre-requisites

1. Knowledge of React
    
2. Basic understanding of Firebase
    

## Problem

So, I was trying to implement Google Signin authentication with the code below:

```javascript
import { auth } from "../firebase-config";
import { GoogleAuthProvider, signInWithPopup} from "firebase/auth";
import { collection, addDoc, getDocs, doc, query, serverTimestamp} from "firebase/firestore";

const gmailProvider = new GoogleAuthProvider();
  const handleUserSignInWithGoogle = async () => {
    try {
      const res = await signInWithPopup(auth, gmailProvider);
      const user = res.user;
      const q = query(collection(db, "users"));
      const docs = await getDocs(q);
      docs.forEach((items) => {
        console.log(items);
        if (items.data().email == user.email) {
          alert("Email already exists, log in instead");
          handleNavigateUser("signin");
          // return;
        } else {
          addDoc(collection(db, "users"), {
            uid: user.uid,
            createdAt: serverTimestamp(),
            name: user.displayName,
            authProvider: "google",
            email: user.email,
            password: "",
            invoiceData: [],
          });
          alert("Account created successfully");
          handleNavigateUser("create-invoice");
        }
      });
  
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

### Code explanation

This code is a function that allows a user to sign in with Google using Firebase authentication. The function is triggered when the user clicks a button or takes some other action to initiate the sign-in process.

This code demonstrates the logic behind a Google signin functionality using Firebase as a backend. The GoogleAuthProvider is instantiated to create a new Google authentication provider instance, which is used to authenticate users through their Google accounts.

The `handleUserSignInWithGoogle` function serves as main handler for the Google sign-in functionality. When the function is called, it performs the following steps:

1. Call the `signInWithPopup` method with the `auth` and `gmailProvider` as its arguments to sign in the user through their Google account.
    
2. After the user signs in, the user object is stored in the `user` variable.
    
3. The function then queries the Firebase Firestore database for a collection of users, which is stored in the `docs` variable.
    
4. The `docs` variable is looped over using the `forEach` method. For each document in the `docs` array, the email address is compared to the email address of the user who has just signed in. If the email address already exists, the function alerts the user that the email address is already in use and navigates the user to the login form instead of creating a new account.
    
5. If the email address does not exist in the `docs` array, the function adds a new document to the Firestore collection of users with the user's information with the `addDoc` method from firestore.
    
6. The user is then alerted that the account was created successfully and navigated to the invoice creation form.
    
7. If an error occurs during the sign-up process, the function checks the error code and alerts the user with a corresponding error message.
    

## Fixing the Signin duplication

```javascript
import { auth } from "../firebase-config";
import { GoogleAuthProvider, signInWithPopup} from "firebase/auth";
import { collection, addDoc, getDocs, doc, updateDocs, query, serverTimestamp} from "firebase/firestore";

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
        alert("Login successful");
        handleNavigateUser("create-invoice");
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
        handleNavigateUser("create-invoice");
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

### Code explanation

1. First, the function creates a new instance of the `GoogleAuthProvider` class, which allows the user to sign in with their Google credentials. Then the function uses the `signInWithPopup` method from Firebase authentication to sign in the user.
    
2. After the user has been signed in, the function retrieves all the documents from the `users` collection in Cloud Firestore.
    
3. It then loops through each document, checking if any of them have the same email address as the signed-in Google user.
    
4. If a document with the same email address is found, the function updates the corresponding Firestore document with the new user information with the `updateDoc()` method from firebase.
    
5. If no document with the same email address is found, the function creates a new document in the `users` collection with the information of the signed-in Google user.
    

## Solution

Compared to the first code, this code checks if the signed-in Google user email address already exists in the Firestore database before creating a new document. If the email address is already in use, the function updates the corresponding Firestore document with the new user information with the `updateDoc` functionality, rather than creating a new document and overwriting the existing data. This way, the data in the Firestore database is preserved and not overridden.

Also, note that not all the user fields are being updated in the `updateDoc` function such as the `invoiceData` array. This is to ensure that the existing data in the users field do not get overridden by the updateDoc when they are signing in with Google.

## Conclusion

I hope this code has been helpful to you and provides a good solution to integrating Google SignIn with Firebase. Follow me [here on Hashnode](https://hashnode.com/@kodervine) for more helpful articles and tips on coding.