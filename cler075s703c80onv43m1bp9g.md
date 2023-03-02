---
title: Boost Your React App's Performance with Lazy Loading and Suspense
datePublished: Thu Mar 02 2023 11:08:39 GMT+0000 (Coordinated Universal Time)
cuid: cler075s703c80onv43m1bp9g
slug: boost-your-react-apps-performance-with-lazy-loading-and-suspense
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1677581657990/88ac8f02-126d-4ebe-a814-285411811c02.png
tags: javascript, web-development, reactjs, hashnode, frontend-development

---

## Introduction.

Imagine you're at a pizza restaurant and you're famished. You order your favorite pizza, and the waiter tells you that it'll be ready in just a few minutes. 

Now, you're really hungry and you can't wait to eat, right? But the chef making the pizza is taking their sweet time and it's making you hangry. 

That's how your users feel when your app is slow to load.

Now let's take it a step further. After more minutes, the waiter brings you not one, but ten piping hot pizzas! 

That's great, right? Especially if it's on the house. You're super hungry after all. But hold on, you only asked for one. So what do you do with the other nine? Do you eat them all at once and risk getting a stomach ache? Or do you save them for later and enjoy them when you're ready?

Well, the same thing happens in your React app. When you first load your app, it loads all the components and data at once. This is like getting all those pizzas at once. It's great if you need everything right away, but if not, it can slow down your app and make it less efficient.

This is where `lazyloading` comes in.

## What is lazy Loading?

With lazyloading, you can choose which components and data to load only when they're needed. It's like getting one pizza at a time, instead of all ten at once. 

Instead of loading the entire page and all its components at once, it loads only what's needed at the moment. This way, your app doesn't waste resources loading things that the user might not even see or interact with.

So how do you implement lazyloading in your React app? It's super simple. You can use the `React.lazy()` function and wrap your component in a Suspense component. 

### Code example for using React.lazy() function  

Here's an example:

```javascript
import React, { lazy, Suspense } from 'react';
const MyComponent = lazy(() => import('./MyComponent'));


function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <MyComponent />
      </Suspense>
    </div>

  );

}
```

See how easy that was? Now your app will only load MyComponent when it's needed, making your app faster and more efficient than ever before.

## Suspense

In addition to lazy loading, React provides another feature called `Suspense` to help optimize the performance of our applications. 

As can be seen in the above example, `MyComponent` was wrapped in a Suspense component.

Basically, Suspense ensures that the data loads before showing a component, so react doesn't end up showing a blank screen that makes users wonder if the web app is even working or whether they have network issues.

Think of it like ordering the pizza which the waiter promised a few minutes. You place the order and then go about your day, like checking your phone or chatting with friends. 

When the pizza is finally ready, you get a notification and then head back to the spot to pick it up. That's how Suspense works too. Our React components keep showing specified content while waiting for other parts of the app to load in the background.

This way, users don't get fed up and bounce from the website if they encounter slow loading times. 

So, when you combine lazy loading and Suspense in your React apps, users have a smooth experience, like waiting for that delicious pizza without getting angry and hangry.

### Code example

So, here's an example of how you can use Suspense and React Router to load components lazily and handle code splitting:

```javascript
import React, { lazy, Suspense } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';


const Home = lazy(() => import('./components/Home'));
const About = lazy(() => import('./components/About'));
const Contact = lazy(() => import('./components/Contact'));
const NotFound = lazy(() => import('./components/NotFound'));


function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route exact path="/" component={Home} />
          <Route path="/about" component={About} />
          <Route path="/contact" component={Contact} />
        </Routes>
      </Suspense>
    </Router>
  );
}


export default App;
```

### Code explanation

In this example, we first import the `lazy` and `Suspense` components from React. Then, we use lazy for each of the components that we want to load with the lazy function.

Each of these components only loads when it's needed, so we don't have to load everything all at once and affect app performance.

Then, the suspense component sets a `fallback` that shows while the lazy components are loading. In this case, we just put up a basic `"Loading..."` message.

Then inside the `Routes` component, we define each of our routes using the `Route` component from `React Router`.

***Note that you need to wrap the Routes in the Suspense wrapper, and not wrap the suspense directly with Route. That's because the Routes only accept Route as its child components.***

## Conclusion 

So there you have it. Lazyloading is like getting one pizza at a time, instead of all ten at once. While Suspense is like going about our day while waiting for the pizza. By using lazy loading and Suspense in our React applications, we can ensure that users have a smooth and enjoyable experience, much like waiting for a delicious pizza to be ready without getting frustrated

By using lazy and Suspense, we can make sure that our components only load when they're actually needed, which makes our React app way faster and smoother. Happy coding 

## Additional Resources

[https://beta.reactjs.org/reference/react/lazy](https://beta.reactjs.org/reference/react/lazy)

[https://www.youtube.com/watch?v=-4fyyyQjsz8](https://www.youtube.com/watch?v=-4fyyyQjsz8)