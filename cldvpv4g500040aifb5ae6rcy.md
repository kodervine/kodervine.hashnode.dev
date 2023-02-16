# How to prevent React ProtectedRoutes from Redirecting to Login on Page Reload

In standardized applications, it is important to ensure that your users' information is not accessible to anyone but your users.

One common way to handle this is through the use of protected routes, which only allow access to specific pages of an application when the user is authenticated. This is to prevent unauthorized access to certain pages in an application. If you don't know how to set up protected Routes, you can read [this post.](https://www.linkedin.com/posts/chinenye-anikwenze_react-reactrouter-webdevelopment-activity-7027219055845490688-QlAH/?utm_source=share&utm_medium=member_android)

## Problem

I encountered an issue where my user authentication state was being lost upon page reload or manual URL typing. The reason for this was that the initial value of my `user` authentication state was set to `false`.

```javascript
 const [currentUser, setCurrentUser] = useState("");
  const [userUpdated, setUserUpdated] = useState(false);
  useEffect(() => {
    onAuthStateChanged(auth, (user) => {
      if (user) {
        setCurrentUser(user);
        setUserUpdated(true);
      } else {
       setUserUpdated(false)
      }
    });
  }, [auth, currentUser]);
```

So, whenever the app re-renders, the userUpdated value is set to false. Which in turn, evaluates to false on the ProtectedRoutes component like so:

```javascript
import React from "react";

import { Outlet, Navigate } from "react-router-dom";
import { useGlobalContext } from "../context/AppContext";

const ProtectedRoutes = () => {
  const { userUpdated } = useGlobalContext();
  return userUpdated ? <Outlet /> : <Navigate to="signin" />;
};

export default ProtectedRoutes;
```

This meant that the protected routes were not being rendered even if the user was logged in.

**To understand this issue better, let's use an analogy.**

Imagine you are playing a video game where you have to unlock different levels by earning points. You have reached a certain level and saved your progress, but every time you reload the game, you have to start from the beginning.

This is similar to the issue I faced with my React application and the boolean value.

## Solution 1

I set the initial value of my user authentication state to true and then updated it to false only if the onAuthStateChanged method returns a falsy value, indicating that the user is not logged in.

```javascript
 const [currentUser, setCurrentUser] = useState("");
  const [userUpdated, setUserUpdated] = useState(true);
  useEffect(() => {
    onAuthStateChanged(auth, (user) => {
      if (user) {
        setCurrentUser(user);
        setUserUpdated(true);
      } else {
        setUserUpdated(false);
      }
    });
  }, [auth, currentUser]);
```

This allowed me to preserve the user's authentication state even after a page reloads or manual URL typing, ensuring that the protected routes are rendered correctly, just like your saved progress in the video game remains intact.

## Solution 2

The first solution has a minor bug to it.

It allows an unregistered user access to the ProtectedRoutes for a split second before redirecting to the signin page.

A more robust solution is to store the user authentication state in local storage. This way, even if the user closes the browser, the authentication information will persist.

To implement this solution, I added a line of code to store the authentication state in local storage every time the user logs in. Similarly, when the user logs out, I remove the authentication information from local storage.

Here's an example of how to use local storage with user authentication in React:

```javascript
 const [currentUser, setCurrentUser] = useState("");
  const [userUpdated, setUserUpdated] = useState(false);
  useEffect(() => {
    onAuthStateChanged(auth, (user) => {
      if (user) {
        setCurrentUser(user);
        setUserUpdated(true);
        localStorage.setItem("isUserSignedIn", true);
      } else {
        setUserUpdated(false);
        localStorage.removeItem("isUserSignedIn");
      }
    });
  }, [auth, currentUser]);
```

So, this is what the updated Protected Routes will look like:

```javascript
import React from "react";
import { Outlet, Navigate } from "react-router-dom";

const ProtectedRoutes = () => {
  return localStorage.getItem("isUserSignedIn") ? (
    <Outlet />
  ) : (
    <Navigate to="signin" />
  );
};

export default ProtectedRoutes;
```

## Conclusion

With these two solutions in mind, you'll never have to worry about losing user authentication information in your web application again. Whether you choose the first solution or the second, the important thing is that you're able to preserve the user's authentication state even after a page reloads or manual URL typing.