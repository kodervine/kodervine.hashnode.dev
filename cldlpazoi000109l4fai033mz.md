# Fixing the Duplication Issue in User Creation with Firebase Authentication

## Problem

I encountered an issue with Firebase where multiple instances of the same user are being created on `signup`

```javascript
 const handleCreateUserWithEmailAndPassword = async (
    email,
    password,
    name = "username"
  ) => {
    try {
      const res = await createUserWithEmailAndPassword(auth, email, password);
      const user = res.user;
      // confirm if users email already exists in the collection but not properly working yet
      const q = query(collection(db, "users"));
      const docs = await getDocs(q);
      docs.forEach((items) => {
        if (items.data().email == user.email) {
          alert("Email already exists, log in instead");
          handleNavigateUser(signin);
          // return;
        } else {
          addDoc(collection(db, "users"), {
            uid: user.uid,
            createdAt: serverTimestamp(),
            name,
            authProvider: "local",
            email,
            password,
            invoiceData: [],
          });
          console.log("user created");
          handleNavigateUser("create-invoice");
        }
      });
    } catch (error) {
      if (error.code == "auth/email-already-in-use") {
        alert("The email address is already in use, log in instead");
        handleNavigateUser("signin");
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

## Code explanation

The `handleCreateUserWithEmailAndPassword` function creates a user with the given email and password using the `createUserWithEmailAndPassword` function. It then confirms if the user's email already exists in the Firestore `users` collection by querying all documents from that collection and comparing each document's email field with the newly created user's email.

If the email already exists, it alerts the user and navigates them to the `signin` page, otherwise it adds a new document to the `users` collection with the specified fields and navigates the user to the `create-invoice` page.

If there is an error while creating the user, the error codes `auth/email-already-in-use`, `auth/invalid-email`, `auth/operation-not-allowed`, and `auth/weak-password` are checked and an appropriate alert message is shown to the user.

## Reason for the bug

What happens in the above code is that I was creating multiple records in Firebase for the same user by using a `forEach` method that runs through all the documents in the `users` collection. This loop will create a new document in the collection each time it runs, even if a document for that user already exists.

## Solution

So, the solution is to get rid of the `query` and `forEach` method like so:

```javascript
 const handleCreateUserWithEmailAndPassword = async (
    email,
    password,
    name = "username"
  ) => {
    try {
      const res = await createUserWithEmailAndPassword(auth, email, password);
      const user = res.user;
      await addDoc(collection(db, "users"), {
        uid: user.uid,
        createdAt: serverTimestamp(),
        name,
        authProvider: "local",
        email,
        password,
        invoiceData: [],
      });
      console.log("user created");
      handleNavigateUser("create-invoice");
    } catch (error) {
      if (error.code == "auth/email-already-in-use") {
        alert("The email address is already in use, log in instead");
        handleNavigateUser("signin");
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

To avoid creating multiple instances of the same user, the previous version of the function checked the existence of the user's email in the "`users`" collection before creating a new document.

However, this caused multiple instances of the same user to be created in the Firebase database for each loop. This function solves this issue by directly adding the new document after creating the user, without checking the email existence beforehand as there is already a Firebase inbuilt error code for an existing authenticated email.

## Conclusion

In conclusion, the solution to the issue of duplication of users in the Firebase database was achieved by updating the create user function. By eliminating the `forEach` method and directly adding the user data to the Firebase database using the `addDoc` function, only a single instance of the user was created.

I hope this solution was helpful to you. And if you're interested in receiving similar updates, you can follow me on [hashnode via this link.](https://hashnode.com/@kodervine)