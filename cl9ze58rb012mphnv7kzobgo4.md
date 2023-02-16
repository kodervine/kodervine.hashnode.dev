# Practical use case of the JavaScript substring() method

# Understanding Substring() method

One of the most frequently used JavaScript methods is the `substring()` method.

In this article, we will look at how the method works.

Also, you will follow along on the practical use case of how to use it in your project.

Substring() helps you extract values from a given string at the beginning and end of a string.

You can also pass negative or positive indexes as long as they exist within the string, or provided that there is no null character.

There are different ways to use this method

### 1\. When two parameters are passed

`substring(start, end)`

* `start` The starting point for accessing the data (the first character is at the position 0).
    
* `end` The result is a newly extracted string.
    

***Note that the original string is not altered at any time during the manipulation.***

Let's look at the example of application using two parameters:

```javascript
"Hello World!".substring(6, 12); 

// "World!"
```

![substring 2 parameter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667140256224/BJ9ZvK3y7.png align="left")

It also works with negative parameters.

***When the first parameter is*** `negative`, it automatically starts from the first index

![negative first parameter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667140891243/WviPALPhj.png align="left")

***When both parameters are*** `negative`, it returns an empty string

![2 neagtive parameters.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667140989536/w6tu_UtAy.png align="left")

### 2\. Use with a single parameter

If only one number is passed into the method, it returns from the index of the parameter passed to the method

```javascript
"Hello World!".substring(6); 

// "World!"
```

![substring 1 parameter.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667140387643/CFLNGOkwR.png align="left")

### 3\. Other Use cases

If start index exceeds the end, the method switches the two parameters. And this works both when the end index is a positive or negative interger

```javascript
"Hello World!".substring(12, 6); 

 // "World!"
```

***Positive last index***

![negative substring.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667140379960/xhfbjH3Xk.png align="left")

***Negative last index***

![last index negative.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667141205940/rv6ZvkAbS.png align="left")

[Read more about substring method here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring)

## Substring() in practice

Now that we've covered the theoretical aspect, let's try this in real practice.

Suppose you have an article where you want to toggle between the full length of the article and a preview, this is a perfect use case of the JavaScript substring method.

For instance, this sample below -

<iframe src="https://share.descript.com/embed/1FPMbp1xD8Y" width="320" height="200"></iframe>

If you want to follow along with this mini-project, I have attached the HTML, and CSS markup to copy -

**Html markup**

```javascript
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="stylesheet" href="style.css" />

    <title>On Substring</title>
  </head>
  <body>
    <section>
      <h3>Using substring in javascript</h3>
      <p id="full-text">
        Substring method is very useful in web development. It helps you extract
        values from a given string at the beginning and end of a string. The
        substring method in JavaScript also helps you to extract values from a
        given string at any index where the concatenation may be possible and
        can return the value of that index. You can also pass negative or
        positive indexes as long as they exist within the string, or provided
        that there is no null character. This makes it a very flexible method to
        use in your projects. In this article, we will look at how you can make
        use of the substring method in JavaScript. Also, you will follow along
        on some examples and practical use cases of how to make use of this
        functionality. The substring() method returns the part of the string
        between the start and end indexes, or to the end of the string.
      </p>
      <button id="toggle-btn">show full text</button>
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
  background-color: azure;
  color: black;
  margin: auto;
  background-color: #f5f1f5;
}

h3 {
  text-align: center;
  padding-bottom: 10px;
}

#toggle-btn {
  padding: 10px 15px;
  margin: 5px;
  font-weight: 600;
  border: none;
  background-color: rgb(100, 2, 100);
  color: white;
  cursor: pointer;
  border-radius: 5px;
  opacity: 0.8;
  transition: 0.5s ease;
  display: inline;
}

#toggle-btn:hover {
  opacity: 1;
}
```

**Javascript practice**

For JavaScript, here is what we can do.

Now, note that the data for this project could be gotten from the database, or from an input.

However, we will be working with the data that we manually created.

### Step 1. Creating the pseudocode for the logic

To get to the main logic, our goal is this...

1. When our window loads, we want the paragraph to be automatically reduced.
    
2. Then, when one clicks the `button`, it manipulates the paragraph, and changes the `innerText`
    

So the pseudocode that should work perfectly is this 👇

* When window loads, show the paragraph in `substring()` state.
    
* Then, when the button is clicked to show the full paragraph, change the text on the button to `"show Less Text"` and display the full paragraph
    
* Toggle between full paragraph and reduced paragraph
    

With the pseudocode above, it means we'll be working with `window.onload` event, using the `click eventListener` and checking `conditionals.`

In practice, here is how we create the logic.

### Step 2. Define variables to be used

First, let's define the two main variables that we will be manipulating.

```javascript
const fullParagraph = document.getElementById("full-text");
const expandBtn = document.querySelector("#toggle-btn");
```

So there is the `fullParagraph` variable which represents the paragraph

Then there is the `expandBtn` which is the button that we'll be toggling with.

### Step 3. Store the value of the fullParagraph

The next in line is to get access to the content of the `fullParagraph` variable and store it in a variable.

So, I created a new array variable and stored the original Text.

```javascript
const fullTextArray = [fullParagraph.innerText];
```

### Step 4. Using the substring() method on values

Now, let's work on the substring.

So I saved the substring of the fullParagraph to a new variable, reduced paragraph

```javascript
const reducedParagraph = `${fullParagraph.innerText.substring(0, 100)}...`;
```

### Step 5. Using event listener to manipulate the value

When our window loads, we want the text to be automatically reduced.

```javascript
window.onload = () => {
  fullParagraph.innerText = reducedParagraph;
};
```

Then, when one clicks the `expandBtn`, it checks if the `innerText` of the button equals `show less text`.

If so, it sets the innerText of the `fullParagraph` to the `reducedParagraph`

Else, it sets the innerText of the `fullParagraph` to the `fullTextArray`

As seen below 👇

```javascript
expandBtn.addEventListener("click", () => {
  if (expandBtn.innerText === "show less text") {
    fullParagraph.innerText = reducedParagraph;
    expandBtn.innerText = "show full text";
  } else {
    fullParagraph.innerText = fullTextArray;
    expandBtn.innerText = "show less text";
  }
});
```

Your code should end up looking like this 👇

```javascript
// Define global variable
const fullParagraph = document.getElementById("full-text");
const expandBtn = document.querySelector("#toggle-btn");

// Saved the initial value of the paragraph to be stored as an array
const fullTextArray = [fullParagraph.innerText];

// Stored the substring of the paragraph to a variable to be reused
const reducedParagraph = `${fullParagraph.innerText.substring(0, 100)}...`;

// When the window loads, set the paragraph to be the value of the reduced paragraph
window.onload = () => {
  fullParagraph.innerText = reducedParagraph;
};

// Add click event listeners
expandBtn.addEventListener("click", () => {
  if (expandBtn.innerText === "show less text") {
    fullParagraph.innerText = reducedParagraph;
    expandBtn.innerText = "show full text";
  } else {
    fullParagraph.innerText = fullTextArray;
    expandBtn.innerText = "show less text";
  }
});
```

[View live demo here -](https://codepen.io/kodervine/pen/KKepjNY)

## Conclusion

The Javascript `substring()` method returns the part of the string between the start and end indexes, or to the end of the string.

This makes it a very flexible method to use in your projects.

I hope that this will answer any lingering questions about the substring method so that you feel confident using it in your own projects.