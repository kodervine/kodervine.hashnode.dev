# Accessing Firestore Data and Ensuring State Update in useEffect: A Step by Step Guide

## Introduction

React hooks are powerful for managing state in React components, but it comes with a few challenges. One common issue that developers face when using React hooks is updating the state in one component and accessing it in another. In this tutorial, we'll explore a common scenario where this issue arises and show you how to resolve it.

If you're working with **Firebase** in React and using the **onAuthStateChanged** function to fetch the current user data, you might encounter the error of `undefined` state. This error can occur when you try to access the user data outside the `useEffect` hook, which is the primary way to handle state changes in React.

**Here's an example of a sample code that can cause the error:**

```javascript
const [currentUser, setCurrentUser] = useState("");

useEffect(() => {
  onAuthStateChanged(auth, (user) => {
    if (user) {
      console.log("note: user is signed in");
      setCurrentUser(user);
      console.log(user.uid);
    } else {
      console.log("note: user is signed out");
    }
  });
}, []);
```

In this example, there are two state variables: `currentUser` and `userUpdated`. The `currentUser` variable is used to store the current user who is signed in, and `userUpdated` is a boolean value that determines if the user information has been updated.

The `useEffect` hook is used to listen to changes in the `auth` variable. When the `auth` variable changes, the `onAuthStateChanged` function is called, which takes the current user as an argument. If the user is signed in, we log a message and set the `currentUser` and `userUpdated` state variables.

**Now, let's take a look at a second piece of code that accesses the currentUser state:**

```javascript
const [allInvoiceData, setAllInvoiceData] = useState([]);
const fetchInvoiceData = async () => {
    try {
      const q = query(collection(db, "users"));
      const querySnapshot = await getDocs(q);
      querySnapshot.forEach((document) => {
        const userInfoInFirebase = document.data();
        if (userInfoInFirebase.uid == currentUser.uid) {
          const newInvoiceData = document.data().invoiceData;
          setAllInvoiceData(newInvoiceData);
        }
      });
    } catch (e) {
      console.log(e);
    }
  };
```

This code defines an asynchronous function `fetchInvoiceData` that retrieves data from a Firebase database. The function performs a query on the `users` collection and retrieves a `querySnapshot` that contains the data for all the documents in the collection.

For each document, the function compares the `uid` property of the `userInfoInFirebase` object with the `currentUser.uid` value. If they match, the `newInvoiceData` is set as the new value of the `allInvoiceData` state and the `newInvoiceData` is logged to the console.

## Problem with the codes

The problem with the code is that `currentUser` is always `undefined` when the `fetchInvoiceData` function is called, even though the `currentUser` state is updated in the `useEffect` hook. This is because the `useEffect` hook is only executed once in the `onAuthChanged` useEffect, when the component is first mounted.

As a result, the `currentUser` state is only updated once and remains unchanged for subsequent renderings and this is because the dependency array is empty. So, when you try to access the `currentUser` outside the `useEffect`, it still shows an empty useState.

## Solution

To resolve this issue, a dependency array has to be added to the `useEffect` hook to ensure that it re-runs whenever the `currentUser` state changes.

```javascript
const [currentUser, setCurrentUser] = useState("");
const [userUpdated, setUserUpdated] = useState(false);

useEffect(() => {
  onAuthStateChanged(auth, (user) => {
    if (user) {
      console.log("note: user is signed in");
      setCurrentUser(user);
      setUserUpdated(true);
    } else {
      console.log("note: user is signed out");
    }
  });
}, [auth, currentUser]);
```

By adding `currentUser` as a dependency to the `useEffect` hook, we ensure that the hook re-runs whenever the `currentUser` state changes. This ensures that the updated user information is accessible outside of the hook.

**Finally, here's an example of how you can use the updated** `currentUser` **to fetch the data from Firestore:**

```javascript
// Ensure user exists
const [currentUser, setCurrentUser] = useState("");
  const [userUpdated, setUserUpdated] = useState(false);
  useEffect(() => {
    onAuthStateChanged(auth, (user) => {
      if (user) {
        setCurrentUser(user);
        setUserUpdated(true);
      } else {
        console.log("note: user is signed out");
        setUserUpdated(false);
      }
    });
  }, [auth, currentUser]);

// fetch data from firestore
const [allInvoiceData, setAllInvoiceData] = useState([]);
  const fetchInvoiceData = async () => {
    if (userUpdated) {
      try {
        const q = query(collection(db, "users"));
        const querySnapshot = await getDocs(q);
        querySnapshot.forEach((document) => {
          const userInfoInFirebase = document.data();
          if (userInfoInFirebase.uid == currentUser.uid) {
            const newInvoiceData = document.data().invoiceData;
            setAllInvoiceData(newInvoiceData);
          }
        });
      } catch (e) {
        console.log(e);
      }
    }
  };

  useEffect(() => {
    if (currentUser) {
      fetchInvoiceData();
    }
  }, [currentUser]);
```

The `fetchInvoiceData` function queries the "users" collection of the Firestore database and retrieves the documents within it. For each document, it checks if the `uid` of the `userInfoInFirebase` matches the `uid` of the `currentUser`. If it does, it sets the `newInvoiceData` to the data retrieved from `invoiceData` in the document, and updates the `allInvoiceData` state with `setAllInvoiceData(newInvoiceData)`.

Finally, the `useEffect` hook is used to trigger `fetchInvoiceData` only when the `currentUser` state changes. The dependency array `[currentUser]` makes sure that the effect is only run when the `currentUser` value changes, and not on every render.

This code is used to fetch and store user invoice data from Firestore, but only after the user has been authenticated and their data has been updated in the `currentUser` state.

## Conclusion

In conclusion, the `useEffect` hook is a powerful tool for adding side effects to your React components. When fetching data from Firestore, it's important to correctly manage state and dependencies to avoid errors. By using a boolean value to check if the state has been updated before running the dependencies, you can ensure that your data is correctly fetched and processed.