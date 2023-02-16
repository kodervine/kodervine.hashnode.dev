# Streamlining Code for Improved Functionality and Memory Management

## The Importance of Reusable Functions and Efficient Folder Structuring

As a software developer, it is easy to get caught up in the day-to-day task of adding new features and fixing bugs. However, it is equally important to take a step back and evaluate the efficiency of our code. This is what happened to me recently when I found myself writing the same function, `handleNavigateUser` by calling `useNavigate` hook in 7 different files.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675249406177/a838dafc-2570-42f6-92da-363ab0b430df.png align="center")

It was then that I realized how much unnecessary redundant code I was writing. Sure, 7 files may not be too much. But what happens when it becomes 30, even 50 files?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675250456490/1fd07d07-5a34-43a4-bfa0-7f6e7cca6a2e.png align="center")

Declaring functions multiple times in different files is not a good coding practice. Not only does it take up more memory, but it also makes it harder to maintain and update the code. To fix this, I decided to focus on creating reusable code in my context file.

## Creating reusable code using useContext Hook

`useContext` is a feature in React that provides a way to share data between components in a React application, without having to pass props down through multiple levels of components. The App context is created with the `React.createContext()` method, which creates an object with two properties: Provider and Consumer.

The Provider component can be placed higher up in the component tree, and the Consumer component can be placed wherever the data is needed in the component tree. This allows for the data to be shared with all components that are descendants of the `Provider component`, without having to manually pass the data down through props. [Learn more about this here](https://beta.reactjs.org/reference/react/useContext)

This is particularly useful for values that are required by many components within an application. I realized that I could declare the `handleNavigateUser` function, make it accept an argument as its parameter inside the `context.jsx` file. Then, I can use the same function in all of my components.

However, I had messed up my folder structure. So, I couldn't call the `useNavigate` hook in my `context.jsx` file as the `AppProvider` is not being wrapped as a child component to the `BrowserRouter` in my index.js root file.

### **Bad Practice**

![Diagram of a React DOM setup with AppProvider, ChakraProvider, Router and App components wrapped inside React Strict Mode.](https://cdn.hashnode.com/res/hashnode/image/upload/v1675249766928/e5265bfd-ca79-469f-bf6e-946787627b45.png align="center")

### **Good practice**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675249857625/31116c45-aacd-49b2-a476-c375199288b1.png align="center")

To fix this, I had to wrap the AppProvider underneath the `router` in `index.js`. This change enabled me to call the `handleNavigateUser()` function inside the `context.jsx` instead of declaring it on the children's components. [Learn more on why useNavigate may not be working in your programme in my article here](https://kodervine.hashnode.dev/uncaught-error-usenavigate-may-be-used-only-in-the-context-of-a-router-component)

## Creating reusable code using useContext Hook

To optimize my code, I declared the "useNavigate" function in my context.jsx file. I then used template literals to indicate the string parameter. By doing this, I was able to reuse the function in multiple files and reduce memory usage. The optimized code looked like this:

```javascript
const navigateUser = useNavigate();
  const handleNavigateUser = (link) => {
    navigateUser(`/${link}`);
  };
```

By declaring functions in a centralized location and using template literals, I was able to write cleaner and more efficient code. This not only saved memory but also made my code easier to maintain in the long run. So, as can be seen, I now have the `useNavigate` function in one file.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1675251033824/f8d31bd6-e9a7-44f5-9ff0-e7d0e3944ad0.png align="center")

## Conclusion

In conclusion, taking the time to optimize your code can have a significant impact on the performance of your app. Whether it's through reusing code or improving your folder structure, every small improvement adds up to create a better overall experience for your users.

Happy coding!