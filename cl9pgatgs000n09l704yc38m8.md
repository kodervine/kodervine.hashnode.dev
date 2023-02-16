# When to use "toggle()" method instead of "add()" and "remove()" methods in Javascript

# Introduction

According to MDN, the `toggle()` method removes an existing token from the list and returns `false`. If the token doesn't exist it's added and the function returns true.

In simpler terms, toggle allows you to alternate between two use cases with just a single line of code.

Before we proceed, this article doesn't cover the basics of javascript. It assumes you have a functional knowledge of javascript.

Attached below are the HTML and CSS markup if you want to follow along

**Html markup**

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script
      src="your font-awesome link"
      crossorigin="anonymous"
    ></script>
    <link rel="stylesheet" href="style.css" />
    <script src="/app.js" defer></script>
    <title>On Toggling</title>
  </head>
  <body>
    <!-- Header -->
    <header class="header">
      <div class="header-name">Using toggling</div>
      <div class="header-portfolio">
        <ul>
          <li><a href="#My_projects">Projects</a></li>
          <li>
            <a href="https://github.com/kodervine" target="_blank">GitHub</a>
          </li>
          <li>
            <a
              href="#"
              target="_blank"
              >LinkedIn</a
            >
          </li>
        </ul>
        <button class="button">
          <a href="mailto:kodervine@gmail.com"
            >Contact Me</a
          >
        </button>
      </div>
      <div class="navbar-menu-icon active">
        <i class="fa-solid fa-bars"></i>
      </div>
      <div class="navbar-delete-icon"><i class="fa-solid fa-x"></i></div>
    </header>
 
    <!-- Mobile navbar -->
    <section class="mobile-navbar-container">
      <ul class="mobile-navbar">
        <li><a href="#My_projects">Projects</a></li></li>
        <li>
          <a href="mailto:kodervine@gmail.com"
            >Contact</a
          >
        </li>
        <li>
          <a href="https://github.com/kodervine" target="_blank">Github</a>
        </li>
        <li>
          <a
              href="#"
              target="_blank"
              >LinkedIn</a
            >
        </li>
      </ul>
    </section>
 
 
    <script src="/app.js"></script>
  </body>
</html>
```

**CSS markup**

```javascript
@import url("https://fonts.googleapis.com/css2?family=Lato:ital,wght@0,300;0,400;0,700;1,300;1,400;1,700&display=swap");
 
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
 
body {
  font-family: "Lato", Helvetica, sans-serif;
  color: var(--primary-black-color);
}
 
/* Header */
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 85px;
  background-color: white;
  box-shadow: 0 5px 5px rgba(221, 218, 218, 0.75);
  position: sticky;
  top: 0vh;
  z-index: 1000;
}
 
.header-name {
  font-size: 20px;
  text-transform: uppercase;
}
 
.header-portfolio {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
}
 
.header ul {
  display: flex;
  gap: 50px;
  font-weight: bold;
  font-size: 16px;
}
 
.header li {
  list-style: none;
}
 
.header li:hover {
  transform: translateY(-1px);
  transition: 0.5s ease;
}
 
.header a {
  text-decoration: none;
  color: var(--primary-black-color);
}
 
.header a:hover {
  color: var(--primary-blue-color);
}
 
.header .button {
  border: 2px solid var(--primary-black-color);
  padding: 10px 20px;
  align-self: center;
  background-color: transparent;
  border-radius: 5px;
  cursor: pointer;
  font-size: 15px;
  font-weight: bold;
}
 
.header .button:hover {
  transform: scale(1.1);
  transition: 1s ease;
}
 
/* Mobile navbar */
.mobile-navbar-container {
  display: none;
}
 
.navbar-menu-icon {
  display: none;
}
 
.navbar-delete-icon {
  display: none;
}
 
/* Media queries */
@media (max-width: 768px) {
  .header {
    padding: 20px 40px 20px 30px;
  }
 
  .header .button {
    display: none;
  }
 
  .header-portfolio {
    display: none;
  }
 
  .mobile-navbar-container.active {
    display: block;
  }
  .mobile-navbar {
    height: 100vh;
    background-color: white;
    color: var(--darkest-blue);
    overflow: hidden;
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding: 0 0 0 2rem;
  }
 
  .mobile-navbar li {
    padding: 10px;
    list-style: none;
    transform: 1s ease-in;
  }
 
  .mobile-navbar a {
    color: var(--darkest-blue);
    text-decoration: none;
  }
 
  .mobile-navbar a:hover {
    border-bottom: 2px solid black;
  }
 
  /* Menu icons */
  .navbar-menu-icon.active {
    display: block;
    cursor: pointer;
  }
 
  .navbar-delete-icon.active {
    display: block;
    cursor: pointer;
  }
}
```

However, you should link your fontawesome kit to the HTML header.

![toggling-font-awesome.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666776346930/x81ZL7GM8.png align="left")

[Go here to get your font-awesome kit](https://fontawesome.com/kits)

### Explaining the code structure

To explain the HTML and CSS markups to be used for the functionality, I created 2 `navs`

One for the large screen, with the class of `header-portfolio` and one for mobile screen, `mobile-navbar-container`

In the css file, the section with the class of `mobile-navbar-container` is set to `hidden` on large screen

While the navbar with the class of `header-portfolio` and the header `button` is set to `hidden` on small screen

However, notice that the css file contains a class of `“active”` added to the `“mobile-navbar-container”`, the `navbar-menu-icon` and the `navbar-delete-icon` on the media query

The `active` class allows the display to be block on the small screen when we handle the functionality

### Working on the Javascript file using add() and remove() methods

Let's say you want to create a responsive menu that hides and displays the navbars on the small screen.

Usually, your function would probably go like this 👇

```javascript
  const mobileNavbarContainer = document.querySelector(
    ".mobile-navbar-container"
  );
 
  const navbarMenuIcon = document.querySelector(".navbar-menu-icon");
  const navbardeleteIcon = document.querySelector(".navbar-delete-icon");
 
  navbarMenuIcon.addEventListener("click", () => {
    mobileNavbarContainer.classList.add("active");
    navbarMenuIcon.classList.remove("active");
    navbardeleteIcon.classList.add("active");
  });
 
  navbardeleteIcon.addEventListener("click", () => {
    mobileNavbarContainer.classList.remove("active");
    navbarMenuIcon.classList.add("active");
    navbardeleteIcon.classList.remove("active");
  });
```

### Explaining the add() and remove() methods

To quickly explain the code above -

I declared the `mobileNavbarContainer` variable which becomes visible on the small screen.

The `mobileNavbarContainer` is what contains the navbar list items for the mobile screen Add the picture showing the console

![toggling-mobile-container.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666775274549/0vUKhxcH9.png align="left")

In the video below, Notice that when I click on the `menu` icon, it gets hidden and the `“X”` shows up in its place.

<iframe src="https://share.descript.com/embed/IK5KRJ3tPRf" width="320" height="300"></iframe>

So, I accessed the font awesome for both the `menu bar` icon and `delete icon` and saved to different variables

```javascript
const navbarMenuIcon = document.querySelector(".navbar-menu-icon");
const navbardeleteIcon = document.querySelector(".navbar-delete-icon");
```

Then added an `event listener` to each of the icons.

When the `navbarMenuIcon` is clicked,

* We add the class `active` to the `mobileNavbarContainer`
    
* Then we remove the class of `active` to the `navbarMenuIcon`
    
* Finally, we add the class of `active` to the `navbardeleteIcon`
    

When it’s the `navbardeleteIcon` being clicked, we do the reverse for the `add` and `remove` methods.

```javascript
navbarMenuIcon.addEventListener("click", () => {
    mobileNavbarContainer.classList.add("active");
    navbarMenuIcon.classList.remove("active");
    navbardeleteIcon.classList.add("active");
  });
 
  navbardeleteIcon.addEventListener("click", () => {
    mobileNavbarContainer.classList.remove("active");
    navbarMenuIcon.classList.add("active");
    navbardeleteIcon.classList.remove("active");
  });
```

[View code pen demo](https://codepen.io/kodervine/pen/PoaooEY)

Now, if this is just a single usage, it is not a big deal.

But suppose you have different codes that need this kind of logic, your code becomes unnecessarily repetitive

Y*our code will end up being full of* `.add()` and `.remove()` methods

Now, while this still gets the job done, we have other methods in Javascript to optimize our code while minimizing the lines of code used.

\*\*That's where the toggle() method in Javascript comes in. \*\*

[You can check out mdn documentation here](https://developer.mozilla.org/en-US/docs/Web/API/DOMTokenList/toggle)

## Toggle() method in practice

Now, let us rework on the previous code sample but using `toggle()` method instead

**Remember that we have 2 icons that we want to add event listeners to.**

The `menuNavbarIcon` and the `deleteNavbarIcon`

So our target is this…

When we click on the **menu bar** icon for the menu `(menuNavbarIcon)`, we want to hide the **menu bar** icon itself and display the **“X”** `(deleteNavbarIcon)`

### STEP 1 - Selecting the icon variables

```javascript
const navbarMenuIcon = document.querySelector(".navbar-menu-icon");
const navbardeleteIcon = document.querySelector(".navbar-delete-icon");
```

### STEP 2 - Add Event Listeners to the icons

Then, we add the `click` event listener type to the icon, you create a function

Previously, our function would have gone thus

```javascript
/*iconPlaceHolder.addEventListener('click', function ( ) {

   *The add and remove classlist goes here*

}*/
```

However, we simply need to declare the eventListeners for both icons

```javascript
navbarMenuIcon.addEventListener("click", toggleMenu);
navbardeleteIcon.addEventListener("click", toggleMenu);
```

### STEP 3 - Create function that handles the toggle() method

However, we are not going to manually declare the function for each of `icons’` event listeners

Rather, we will create a `function` that will handle the `toggle()` for both use cases

So, let's just create the `toggleMenu() function`

```javascript
function toggleMenu() {
    mobileNavbarContainer.classList.toggle("active");
    navbarMenuIcon.classList.toggle("active");
    navbardeleteIcon.classList.toggle("active");
  }
```

So, instead of adding and removing `classLists` for each click, we just toggle between the active classes under the toggleMenu() function.

Then we called the function inside the eventListener for both the `navbarMenuIcon` and `navbardeleteIcon`

Your javascript code should look like this with the toggle() method -

```javascript
  const mobileNavbarContainer = document.querySelector(
    ".mobile-navbar-container"
  );

  // declaring the variable for the event listeners
  const navbarMenuIcon = document.querySelector(".navbar-menu-icon");
  const navbardeleteIcon = document.querySelector(".navbar-delete-icon");

  // The icons event listenrs for onclick
  navbarMenuIcon.addEventListener("click", toggleMenu);
  navbardeleteIcon.addEventListener("click", toggleMenu);

  function toggleMenu() {
    mobileNavbarContainer.classList.toggle("active");
    navbarMenuIcon.classList.toggle("active");
    navbardeleteIcon.classList.toggle("active");
  }
```

[See link to live demo](https://codepen.io/kodervine/pen/jOKOOey)

And that's it folks.

Though this method is largely for syntactic sugar, it's still a great option to add to your code

[Learn more about toggle and toggle event listeners here](https://developer.mozilla.org/en-US/docs/Web/API/HTMLDetailsElement/toggle_event)