---
title: "Build a Responsive Layout with React and Mantine UI"
datePublished: Mon Mar 20 2023 08:57:39 GMT+0000 (Coordinated Universal Time)
cuid: clfglg0qn0034jfnvfxj0b02b
slug: build-a-responsive-layout-with-react-and-mantine-ui
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679054546465/27735f17-9fa0-4b63-887e-141812b49dbb.png
tags: programming-blogs, programming, reactjs, mantine-ui

---

Mantine is a React-based UI library that offers a variety of ready-to-use components to create responsive and modern web applications. One of the most useful components in Mantine is AppShell, which provides a standard layout for web apps that include a header, a sidebar, and a content area. The AppShell component is highly customizable, and it can be adjusted to fit any design or layout needs.

This article will dive into how to utilize the in-built AppShell component in order to create a responsive layout for a web page. The focus will be mainly on the header, the sidebar and content area. To build this, basic CSS properties, and the Mantine UI media query component will also be implemented as well.

## Pre-requisites

1. Knowledge of react
    
2. Understanding of styled components
    
3. Basic knowledge of Mantine ui
    

### **Setting up the AppShell Component**

To get started, let's create an AppShell component with a header, sidebar, and content area. We will use Mantine's `AppShell` component to create the basic layout structure, and then we will add the header and sidebar components.

```plaintext
import React from 'react';
import { AppShell, Aside } from '@mantine/core';

export default function MyApp() {
  return (
    <AppShell
      navbarOffsetBreakpoint="sm"
      asideOffsetBreakpoint="sm"
      header={<div>Header Component</div>}
      aside={<Aside>Aside Component</Aside>}
    >
      <div>Content Area Component</div>
    </AppShell>
  );
}
```

In the code above, the header, navbar and sidebar components were passed onto the `AppShell` component as props. Also, a `Content Area` component is passed as a child component on the AppShell to display the main content of the app.

### **Making the Navbar Responsive**

The Navbar component is typically displayed at the top of the page and contains the main navigation links for the app. In a responsive design, we want to hide the `Navbar` component on small screens and display a `menu icon` that toggles the display of the sidebar.

We can achieve this using Mantine's `MediaQuery` component. The `MediaQuery` component renders its children only if the screen size matches the specified condition. We can use this component to render the `Navbar` component only if the screen size is larger than a certain breakpoint.

```javascript
import React from 'react';
import { AppShell, Aside, MediaQuery } from '@mantine/core';
import { Navbar } from './components';

export default function MyApp() {
  const [isVisible, setIsVisible] = React.useState(false);
  const handleToggle = () => {
    setIsVisible(!isVisible);
  };

  return (
    <AppShell
      navbarOffsetBreakpoint="sm"
      asideOffsetBreakpoint="sm"
      header={
        <MediaQuery largerThan="sm" styles={{ display: 'none' }}>
          <Navbar handleToggle={handleToggle} />
        </MediaQuery>
      }
     
    >
      <div>Content Area Component</div>
    </AppShell>
  );
}
```

In the code above, we used the `MediaQuery` component to hide the Navbar component on small screens by setting its display to `none` when the screen size is larger than the `sm` breakpoint. We also rendered the Navbar component inside an `Aside` component that is hidden on large screens by setting its width to `0`.

We used the `isVisible` state and handleToggle function to toggle the display of the Sidebar component when the screen size is smaller than the sm breakpoint. We passed the handleToggle function to the `NavbarMenu` component so that it can be triggered when the menu icon is clicked.

## Making the Sidebar Responsive

The `Sidebar` component is typically displayed on the left or right side of the page and contains additional navigation links or other widgets. In a responsive design, we want to hide the `Sidebar` component on small screens and display it as a menu that can be toggled on and off.

We can achieve this by using the `isVisible` state to toggle the display of the Sidebar component when the screen size is smaller than the `sm` breakpoint, as we did with the Navbar component. We can also adjust the width of the Sidebar component to make it take up the full height of the screen.

```javascript
import React from 'react'; 
import { AppShell, Aside, MediaQuery } from '@mantine/core'; 
import { Sidebar } from './components';

export default function MyApp() { 
    const [isVisible, setIsVisible] = React.useState(false); 
const handleToggle = () => { setIsVisible(!isVisible); };

return (
    <AppShell 
        navbarOffsetBreakpoint="sm" 
        asideOffsetBreakpoint="sm" 
        aside={ 
            <MediaQuery smallerThan="sm" styles={{ display: isSidebarVisible ? 'block' : 'none' }} > 
                <Aside width={{ sm: '100%', lg: 0 }}> 
                    <Sidebar handleToggle={handleToggle} isVisible={isVisible} /> 
                </Aside> 
            </MediaQuery> } > 
    <div>Content Area Component</div> 
</AppShell> ); 
} 
```

In the code above, we used the `isVisible` state to toggle the display of the `Sidebar` component when the screen size is smaller than the `sm` breakpoint. We also adjusted the width of the `Aside component` to make the `Sidebar` take up the full height of the screen by setting the width to '100%'.

## Conclusion

In this article, the Mantine UI library was used to create a responsive layout using the Mantine UI library. It's inbuilt components like the AppShell was used to define the overall layout of the application including the Header and Sidebar components. Also, the MediaQuery component was used to conditional render specific components depending on the screen size at any given time.

With these tools, we can create a responsive layout that looks great on all screen sizes, from mobile devices to desktop computers. By following the principles of responsive design, we can create a user-friendly experience that adapts to the needs of our users.

## Additional Resources

[https://mantine.dev/core/app-shell/](https://mantine.dev/core/app-shell/)