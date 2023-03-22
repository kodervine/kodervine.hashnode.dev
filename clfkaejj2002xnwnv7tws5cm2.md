---
title: "Sorting Arrays by Most Recent: Tips and Tricks in JavaScript"
datePublished: Wed Mar 22 2023 22:59:39 GMT+0000 (Coordinated Universal Time)
cuid: clfkaejj2002xnwnv7tws5cm2
slug: sorting-arrays-by-most-recent-tips-and-tricks-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1679411723367/928b353b-6f75-41ff-bae7-5001880a003e.png
tags: programming-blogs, programming, javascript, reactjs, array

---

## Introduction.

Let's say you are a librarian at a library. Every day, you receive dozens of books from authors to be added to the library collections. Now, it is your job to ensure that the books are organized and easy to find. You might decide to sort the books by categories and genres. And even within that, you could decide to have a section that sorts books from specific authors.

On the other hand, you might also decide to display the books according to the most recent orders. Now suppose you eventually decide to sort the books by recent orders, here is how it might look in Javascript.

To keep track of the new books, you use a system where they are added to a list in the order they were received. This way, you can easily see which books need to be shelved first and ensure that the library's collection is always up-to-date.

Let's take it a step further. Suppose your library is an online library, you would most likely need a dashboard in order to display the most recent books. How can one implement this?

It might look something like this:

## Code example

```javascript
const newBooks = newBooksData // An array of new books
newBooks.slice().reverse().map((items, index) => {
    const { author, dateAdded } = items;
    return (<div key={index}>
    <h1>{author}</h1>
    <p>{dateAdded}</p>
</div>    );
  });
```

Let's break down what's happening here. The `newBooks` array contains all the data about newly added books, but it's not in the right order that you might want to display it on the dashboard.

### Slice() method

To fix this, the `slice()` method is used to create a new instance of the array with the same elements as `newBooks`, but in a new order.

Now, you might be wondering why a new array is created using the `slice()` method instead of just modifying the original `newBooks` array directly. There is an important reason. And that is because modifying the original array directly can have unintended consequences.

In JavaScript, arrays are objects. Javascript objects are passed by **reference**, and not by their values. What this means is that if you modify an array, you are modifying the elements or data in the original array, not just the elements within the array.

Now, if multiple parts of your code rely on the original array being in a specific order, modifying it directly could cause unexpected bugs and errors.

For instance, suppose the same `newBooks` array is used in another section of the app to display the books based on genres or similar categories.

So, using the `slice()` method to create a new instance of the array with the same elements as the original array, ensures that you work with a fresh copy of the data. This way, you can modify the new array without worrying about affecting the original array used in other parts of the application.

### Reverse () method

We then use the `reverse()` method to reverse the order of the elements so that the most recent order is first.

## Map() method

Once the array is reversed as indicated above, the `map()` method is then used to create a new array of React components that will display the data on the dashboard. For each element in the array, the `author` and `dateAdded` properties are also destructured from the `items` object and used to create a new `div` component. Finally, a unique `key` prop is added to each component so that React can efficiently render the dashboard.

## Conclusion

The different array methods are crucial for implementation like the above. This is because it gives you the liberty to modify data from the array at will without directly affecting the original array. This enables you to use the same data across multiple sections of your application without inducing bugs or errors.

If you found this article helpful, you can Subscribe to my newsletter below in order to get first information when I release a new one.

## Additional resources

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

[https://flexiple.com/javascript/javascript-pass-by-reference-or-value/](https://flexiple.com/javascript/javascript-pass-by-reference-or-value/)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/reverse](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global\_Objects/Array/map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)