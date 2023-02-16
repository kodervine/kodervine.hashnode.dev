# Creating Dynamic PDF Downloads in React: A Step-by-Step Guide Using useState, useRef, and jsPDF library

## Introduction

In this article, we will explore how to use the jsPDF library to add a download feature to a React project. Specifically, you will learn how to download sections of a page dynamically from an array of elements. I will guide you through the process of setting up a new React app, creating the necessary files, and using dummy data to test the implementation.

By the end of this article, you will have a solid understanding of how to use jsPDF in a React project to provide a dynamic download feature.

## Prerequisites

1. Knowledge of JavaScript and React
    
2. Basic understanding of React hooks
    

## Setting up the Project

But if you are following along with this article, the first step is to make sure that you have `Node.js` and `npm (Node Package Manager)` installed on your computer.

To see if you already have Node.js and npm installed and check the installed version, run the following commands separately on your terminal.

```javascript
node -v
npm -v
```

It should display the current versions of either on the terminal.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674480296260/b8fdc85e-3b0a-48be-9f5f-b80d7a9e61e9.png align="center")

If you don't have them installed, you can download them from the official Node.js [website](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). Once you have Node.js and npm installed, you can use the following command to create a new React app:

```javascript
npx create-react-app my-app
```

This command will create a new folder called "`my-app`" in the current directory and install all the necessary dependencies for a React app.

Once the installation is complete, you can navigate to the "`my-app`" folder using the "`cd my-app`" command on your terminal.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674480075519/b6a00960-2107-4fb9-b502-c287ecde7b1c.png align="center")

Then, you can start the development server using the following command:

```javascript
npm start
```

This will start the development server and open your app in a web browser. You should now see a "Welcome to React" message on the localhost.

You can clear the templates on the `src` folder. Your folder tree should be similar to this:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674480822596/485851ef-3143-4e71-8394-ab9a4418808c.png align="center")

The files needed for this project are: `App.js`, `data.js`

The App.js will be our parent directory

While the `data.js` has the dummy data we'd used for this implementation. You can copy the data below.

```javascript
const data = [
  {
    id: 1,
    title: "buttermilk pancakes",
    category: "breakfast",
    price: 15.99,
    desc: `I'm baby woke mlkshk wolf bitters live-edge blue bottle, hammock freegan copper mug whatever cold-pressed `,
  },
  {
    id: 2,
    title: "diner double",
    category: "lunch",
    price: 13.99,
    desc: `vaporware iPhone mumblecore selvage raw denim slow-carb leggings gochujang helvetica man braid jianbing. Marfa thundercats `,
  },
  {
    id: 3,
    title: "godzilla milkshake",
    category: "shakes",
    price: 6.99,
    desc: `ombucha chillwave fanny pack 3 wolf moon street art photo booth before they sold out organic viral.`,
  },
  {
    id: 4,
    title: "country delight",
    category: "breakfast",
    price: 20.99,
    desc: `Shabby chic keffiyeh neutra snackwave pork belly shoreditch. Prism austin mlkshk truffaut, `,
  },
  {
    id: 5,
    title: "egg attack",
    category: "lunch",
    price: 22.99,
    desc: `franzen vegan pabst bicycle rights kickstarter pinterest meditation farm-to-table 90's pop-up `,
  },
  {
    id: 6,
    title: "oreo dream",
    category: "shakes",
    price: 18.99,
    desc: `Portland chicharrones ethical edison bulb, palo santo craft beer chia heirloom iPhone everyday`,
  },
  {
    id: 7,
    title: "bacon overflow",
    category: "breakfast",
    price: 8.99,
    desc: `carry jianbing normcore freegan. Viral single-origin coffee live-edge, pork belly cloud bread iceland put a bird `,
  },
  {
    id: 8,
    title: "american classic",
    category: "lunch",
    price: 12.99,
    desc: `on it tumblr kickstarter thundercats migas everyday carry squid palo santo leggings. Food truck truffaut  `,
  },
  {
    id: 9,
    title: "quarantine buddy",
    category: "shakes",
    price: 16.99,

    desc: `skateboard fam synth authentic semiotics. Live-edge lyft af, edison bulb yuccie crucifix microdosing.`,
  },
];
export default data;

// source - https://github.com/john-smilga/react-projects/blob/master/05-menu/setup/src/data.js
```

### Installing the jsPDF library

In addition to installing a React app, you need to install the jsPDF library. This library allows you to create PDF documents and add content to them. You can install the library by running the following command:

```javascript
npm install jspdf
```

Once the installation is complete, you can import the library into your `App.js` and start using it.

```javascript
import jsPDF from 'jspdf';
```

With React and jsPDF installed, you will have all the necessary tools to start building your application. The jsPDF library is to help you to export the data in pdf format.

### **Implementing jspdf download feature**

In the App.js file, this is what the app should look like:

```javascript
import { useState, useRef } from "react";
import data from "./data";
import jsPDF from "jspdf";
import "./App.css";

function App() {
    return <div className="App">Use Ref</div>
}

export default App;
```

The first step is to create an array that will contain all our dummy data to be rendered dynamically, in this case, the "`allData`" array as seen below.

This array is then passed to the "`map`" function which is used to loop through each element of the array and create a new component for each one.

```javascript
function App() {
  const [allData, setAllData] = useState(data);
  return (
    <div className="App">
      {allData.map((items) => {
        const { id, title, category, price, img, desc } = items;
        return (
          <div
            key={id}
            style={{ border: "2px solid blue", marginBottom: "10px" }}
          >
            <h2>
              {title}: {category}
            </h2>
            <p>${price}</p>
            <p>{desc}</p>
            <button>download menu</button>
          </div>
        );
      })}
    </div>
  );
}

export default App;
```

**Your webpage should look similar to this:**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674481966523/ee821639-a539-429b-bfc9-f084b6aa0a37.png align="center")

## Implementing useRef for each element from the array

When working with React, the use of "`refs`" is important as it helps to track and manipulate specific elements in the DOM (Document Object Model).

A "ref" is an object that can be used to store a reference to a specific element in the DOM. The "`useRef`" hook provided by React allows us to create refs and store them in the component state. [Learn more about the useRef hook here.](https://beta.reactjs.org/reference/react/useRef)

The `ref` hook has a property called `current`, which can be used to store a reference to an element in the DOM.

```javascript

  const EachDownloadRef = useRef([]);
  EachDownloadRef.current = [];
```

In the code snippet provided, we can see the use of refs in action. The first line creates a ref called "`EachDownloadRef`" and assigns it an empty array. The next line sets the "`current`" property of the ref to an empty array. This is done to ensure that the array is empty every time the function implementing the ref is called.

```javascript
 const handlePrint = (element, index) => {
    if (element && !EachDownloadRef.current.includes(element)) {
      EachDownloadRef.current.push(element);
    }
    const content = EachDownloadRef.current[index];
    const doc = new jsPDF("p", "pt", [800, 800]);
    doc.setFontSize(12);
    doc.html(content, {
      callback: function (doc) {
        doc.save("document");
      },
      x: 20,
      y: 20,
      width: 800,
      windowWidth: 800,
      margin: -20,
    });
  };
```

### Explanation of code

The function "`handlePrint`" takes two arguments, "`element`" and "`index`", and is used to handle the downloading of specific sections in the DOM. The first thing the function does is to check if the "`element`" argument is present and that it is not already in the "`EachDownloadRef.current`" array. If these conditions are met, the element is pushed into the array.

The function then creates a new variable called "`content`" and assigns it the value of the element at the "`index`" position in the "`EachDownloadRef.current`" array. This is to ensure that the correct element is being printed.

Next, a new instance of the `jsPDF library` is created and assigned to the "doc" variable. This library allows us to create PDF documents and add content to them. The `doc.setFontSize` method is then used to set the font size of the `document to 12`.

The `doc.html` method is then used to add the "`content`" variable to the document. This method takes an object with several options as an argument. The "`callback`" option is used to specify a function that will be called once the content has been added to the document. In this case, the function saves the document with the name "`document`".

The "`x`" and "`y`" options are used to specify the coordinates of the content on the document. The "`width`" and "`windowWidth`" options are used to set the width of the content. The "`margin`" option is used to specify the margin around the content.

### Integrating jsPDF into JSX Syntax

```javascript
 return (
    <div className="App">
      {allData.map((items, index) => {
        const { id, title, category, price, img, desc } = items;
        return (
          <div
            key={id}
            style={{ border: "2px solid blue", marginBottom: "10px" }}
            ref={handlePrint}
          >
            <h2>
              {title}: {category}
            </h2>
            <p>${price}</p>
            <p>{desc}</p>
            <button
              onClick={() => {
                handlePrint(items, index);
              }}
            >
              download menu
            </button>
          </div>
        );
      })}
    </div>
  );
```

### Explanation of Generating PDFs code

The above code is using React JSX syntax to render a list of items in a div element with the class "App". Inside this div, the code is using the `map` method to iterate through an array of `allData` and for each item in the array, it is destructuring the item to get the values of `id`, `title`, `category`, `price`, `img`, and `desc`.

For each item, it is returning a new `div` element with a `key` prop set to the item's `id` and a `ref` prop that is passed to a function `handlePrint`.

Inside this `div`, it's rendering the `title`, `category`, `price`, and `desc` of the item, and a button with an `onClick` event handler that calls a function `handlePrint` and passes the current item being iterated as **items** and its **index** as arguments.

The button text is `download menu.`When the button is clicked, it triggers the `onClick` event that is associated with the button and calls the `handlePrint` function, passing the current item and its index as arguments.

The `handlePrint` function uses the passed item and its index to create a pdf document of the menu item and save it on the local machine with the name 'document'.

**Your App.js file should end up like this for the download to work properly -**

```javascript
import { useState, useRef } from "react";
import data from "./data";
import jsPDF from "jspdf";
import "./App.css";

function App() {
  const [allData, setAllData] = useState(data);
  const EachDownloadRef = useRef([]);
  EachDownloadRef.current = [];
  const handlePrint = (element, index) => {
    if (element && !EachDownloadRef.current.includes(element)) {
      EachDownloadRef.current.push(element);
    }
    const content = EachDownloadRef.current[index];
    const doc = new jsPDF("p", "pt", [800, 800]);
    doc.setFontSize(12);
    doc.html(content, {
      callback: function (doc) {
        doc.save("document");
      },
      x: 20,
      y: 20,
      width: 800,
      windowWidth: 800,
      margin: -20,
    });
  };

  return (
    <div className="App">
      {allData.map((items, index) => {
        const { id, title, category, price, img, desc } = items;
        return (
          <div
            key={id}
            style={{ border: "2px solid blue", marginBottom: "10px" }}
            ref={handlePrint}
          >
            <h2>
              {title}: {category}
            </h2>
            <p>${price}</p>
            <p>{desc}</p>
            <button
              onClick={() => {
                handlePrint(items, index);
              }}
            >
              download menu
            </button>
          </div>
        );
      })}
    </div>
  );
}

export default App;
```

### Final results

<iframe src="https://share.descript.com/embed/tLbgfgDAjzY" width="400" height="360"></iframe>

## Conclusion

In conclusion, this article has provided a detailed explanation of how to generate and download PDFs in a React application using the jsPDF library and JSX syntax. The article has covered the use of the `useState` and `useRef` hooks to manage the data.

Also, this article covered how to use the jsPDF library to create and save PDFs on the local machine. By following the steps outlined in this article, you can easily implement dynamic PDF generation and download functionality in your different React projects.