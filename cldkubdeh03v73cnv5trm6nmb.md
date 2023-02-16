# Uncaught Error: useNavigate() may be used only in the context of a <Router> component

## Problem

I was trying to implement the `useNavigate()` hook in my React application, but I kept getting an error on my console.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674824831290/c17320e3-48e1-4f8c-92d9-5097877c99ce.png align="center")

After a bit of digging, I discovered that the issue was with the way I had set up my router. Specifically, the `BrowserRouter` component was not properly configured in my `App.jsx` file.

Routing is a crucial aspect of any React application, as it allows users to navigate through different pages without having to refresh the entire page. And one of the most popular libraries for routing in React is `React Router`. React Router provides a component called `useNavigate` that allows developers to navigate to different routes programmatically.

So, the problem was that I was declaring the `useNavigate()` hook in the same file that I was wrapping the `BrowserRouter`

## Solution

The solution to this issue is to move the `BrowserRouter` component from the `App.jsx` file to the `index.js` file.

Here's an example of how to move the `BrowserRouter` to the `index.js` file:

```javascript
// index.js
import React from 'react';
import { render } from 'react-dom';
import { BrowserRouter} from 'react-router-dom';
import App from './App.jsx';


ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
        <BrowserRouter>
          <App />
        </BrowserRouter>
  </React.StrictMode>
);
```

By moving the `BrowserRouter` to the `index.js` file, it will wrap around the entire `App.jsx` component, providing access to the router for all child components.

Now that you have moved the `BrowserRouter` to the `index.js`, you should also remove the BrowserRouter from the `App.jsx`

So, your App.jsx should be similar to this:

```javascript
import React from 'react';
import { useNavigate } from 'react-router-dom';

function App() {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/new-route');
  };

  return (
    <div>
      <button onClick={handleClick}>Go to new route</button>
    </div>
  );
}

export default App;
```

## Conclusion

If you encountered this issue where `useNavigate` is not working in the `App.jsx` file, moving the `BrowserRouter` to the `index.js` is the solution. By wrapping the `App.jsx` component with the `BrowserRouter`, the `useNavigate` hook can access the router as a child component and properly navigate to different routes in your application