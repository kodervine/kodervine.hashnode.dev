# 7 practical use cases for the Array filter method in Javascript with code examples

# **Introduction**

The Array filter method is a powerful tool for filtering and manipulating arrays in JavaScript. It is widely used in many types of applications, but in this article, we will be focusing on 7 practical use cases that can help you to understand how to use it in your projects.

Throughout this article, I will be providing clear examples and explanations to help you to understand how the filter method works and how it can be applied to your own projects.

According to [**MDN**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) -

*It calls a provided* `callbackFn` *function once for each element in an array, and constructs a new array of all the values for which* `callbackFn` *returns a* [***truthy***](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) *value. Array elements that do not pass the* `callbackFn` *test is not included in the new array.*

[![Syntax of the filter method from MDN](https://cdn.hashnode.com/res/hashnode/image/upload/v1674564680727/81156dc4-9131-4509-bf26-b2864ead3205.png?auto=compress,format&format=webp align="left")](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

The filter method can be especially useful in the context of list-based applications. It's a simple but powerful method that can save you time and make your code easier to read and understand.

## **Use cases of the filter method**

### **1\. Deleting items from a list**

In the context of a simple application like a to-do list, the filter method is a concise and efficient way to delete items from an array. Rather than transversing the DOM to find the specific item you want to remove, you can simply use the filter method to return all elements that do not match the ID of the item you clicked.

If you have an array of objects, with each object representing a single to-do list item. For example, let's say you have the following array of to-do list items:

```javascript
const toDoList = [
  { id: 1, task: 'Take out the trash' },
  { id: 2, task: 'Do the dishes' },
  { id: 3, task: 'Mow the lawn' }

];
```

To delete an item from this array using the filter method, you can use the following code:

```javascript
const updatedToDoList = toDoList.filter(item => item.id !== idToRemove);
```

This will create a new array called `updatedToDoList` that contains all of the elements from the original `toDoList` array, except for the one with an ID that matches `idToRemove.`

For example, if you wanted to remove the item with an ID of 2 (i.e., 'Do the dishes'), you would set idToRemove to 2 and the resulting updatedToDoList array would be:

```javascript
[
  { id: 1, task: 'Take out the trash' },
  { id: 3, task: 'Mow the lawn' }
]
```

### **2\. Search and Filter large datasets**

One common use of the filter method in real-life projects is to search and filter large datasets. For example, let's say you have an array of objects representing a list of customers for a company:

```javascript
const customers = [
  { id: 1, name: 'Femi', city: 'Lagos' },
  { id: 2, name: 'Obi', city: 'Enugu' },
  { id: 3, name: 'Charlie', city: 'Abuja' },
  { id: 4, name: 'Amara', city: 'Enugu' },
  { id: 5, name: 'Mary', city: 'Asaba' }

];
```

If you want to allow the user to search this list of customers by city, you can use the filter method to return only those customers that match the search term. For example, the following code would return an array of all customers who live in Enugu:

```javascript
const chicagoCustomers = customers.filter(customer => customer.city === 'Enugu');
```

This would give you the following array:

```javascript
[
  { id: 2, name: 'Obi', city: 'Enugu' },
  { id: 4, name: 'Amara', city: 'Enugu' },
]
```

You could then display this filtered array of customers in a table or list on the page, allowing the user to easily view and interact with the relevant data.

### **3\. Get odd or even numbers**

Filtering an array of numbers to only include even numbers:

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const oddNumbers = numbers.filter(number => number % 2 === 0);
// evenNumbers = [1, 3, 5, 7, 9]
```

### **4\. Have a reference that includes only specific property type**

If you are building an eCommerce store, and you want to display products that are in specific categories, you can use the filter method. Also, you can sort company data of their employees according to specific departments.

Filtering an array of objects to only include those with a specific property value:

```javascript
// Instance 1
const products = [
  { name: 'Product A', price: 50, category: 'Electronics' },
  { name: 'Product B', price: 25, category: 'Apparel' },
  { name: 'Product C', price: 100, category: 'Electronics' },
  { name: 'Product D', price: 100, category: 'Furniture' },
  { name: 'Product E', price: 125, category: 'Clothing' }
];

const electronics = products.filter(product => product.category === 'Electronics');

// electronics = [{ name: 'Product A', price: 50, category: 'Electronics' }, { name: 'Product C', price: 100, category: 'Electronics' }]

// Instance 2
const employees = [
  { id: 1, name: 'Alice', department: 'Sales' },
  { id: 2, name: 'Bob', department: 'Marketing' },
  { id: 3, name: 'Charlie', department: 'Sales' },
  { id: 4, name: 'Dave', department: 'Engineering' },
  { id: 5, name: 'Eve', department: 'Marketing' }
];

const marketingEmployees = employees.filter(employee => employee.department === 'Marketing');
// salesEmployees = [{ id: 1, name: 'Alice', department: 'Sales' }, { id: 3, name: 'Charlie', department: 'Sales' }]
```

### **5\. Get character length**

For instance, if a quiz app requires the user to write fruits that are certain words or longer. You can use the filter method to evaluate whether the response is true with the filter method

Filtering an array of strings to only include those that are at least 5 characters long:

```javascript
const words = ['apple', 'banana', 'cherry', 'pear', 'mango', 'orange', 'grape'];

const longWords = words.filter(word => word.length >= 5);

// longWords = ['apple', 'banana', 'cherry', 'mango', 'orange']
```

### **6\. Know price ranges**

For instance, eCommerce stores, and real estate listing websites. You can use the filter method that allows users to sort products according to specific price ranges.

Filtering an array of objects to only include those that are in a specific price range:

```javascript
const products = [
  { name: 'Product 1', price: 50 },
  { name: 'Product 2', price: 25 },
  { name: 'Product 3', price: 75 },
  { name: 'Product 4', price: 100 },
  { name: 'Product 5', price: 125 }
];

const affordableProducts = products.filter(product => product.price < 50);

// affordableProducts = [{ name: 'Product 2', price: 25 }, { name: 'Product 3', price: 75 }]
```

### **7\. Reference to truthy/falsy properties in a list**

Filtering an array of objects to only include those that have a specific property set to true:

An app that allows admins to make certain changes that a normal user can't. You can use the filter method to store users that are admins. You can use the filtered array to add more features to those who are admins.

```javascript
const users = [
  { id: 1, name: 'Alice', isAdmin: true },
  { id: 2, name: 'Bob', isAdmin: false },
  { id: 3, name: 'Charlie', isAdmin: true },
  { id: 4, name: 'Dave', isAdmin: false },
  { id: 5, name: 'Eve', isAdmin: true }
];

const admins = users.filter(user => user.isAdmin === true);

// admins = [{ id: 1, name: 'Alice', isAdmin: true }, { id: 3, name: 'Charlie', isAdmin: true }, { id: 5, name: 'Eve', isAdmin: true }]
```

An eCommerce store sort to display only products that are still in stock

```javascript
const products = [
  { name: 'Product A', inStock: true },
  { name: 'Product B', inStock: false },
  { name: 'Product C', inStock: true },
  { name: 'Product D', inStock: false },
  { name: 'Product E', inStock: true }
];

const inStockProducts = products.filter(product => product.inStock === false);

// inStockProducts = [{ name: 'Product B', inStock: false }, { name: 'Product D', inStock: false }]
```

## **Conclusion**

In this article, I detailed several examples of how the Array filter method can be used in different scenarios. The filter method is a flexible method that helps developers work more productively and efficiently by utilizing only a few lines of code. It can be used to remove specific properties from an array of elements. Also, it is effective for searching and filtering large databases. Now, you can easily implement the filter method in any of these use cases in your project.