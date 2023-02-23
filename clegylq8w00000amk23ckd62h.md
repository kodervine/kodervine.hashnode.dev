# Alternatives to Handling Nested Arrays in state management: the useReducer hook and Formik library

The `useState` hook is an in-built react hook that is used to manage state in functional components. It allows you to change components based on user interaction. Let's take an e-commerce website for instance whereby a user has to add items to their shopping cart.

An instance of implementing the above feature with `useState` hook will go thus:

If a user adds a new item to their existing cart, the state variable allows their new cart items to be saved properly without overriding their previous data.

The react useState hook helps to implement this feature. Here is the syntax.

```javascript
import {useState} from "react"

const [data, setData] = useState("")
```

The useState hook takes in two parameters.

1. The initial state
    
2. The setter function.
    

From the above example, the data and setData are created using array destructuring. The `data` is the initial state while the `setData` is the setter function that is used to update the content of the data state.

To explore this further, let's take a look at the example below with handling data from a form.

## Prerequisites

1. Having node installed
    
2. Basic understanding of React and React hooks
    

## Handling data with the useState hook

```javascript
import {useState} from "react"
 
const [formData, setFormData] = useState({
    name: "",
    email: "",
    age: "",
    itemsContainer: [
      {
        itemContent: "",
        itemQty: "",
        itemPrice: "",
      },
    ],
  });
```

This is the initialization of `formData` state, with its setter function named as `setFormData`

## Handling Input change

```javascript
const handleInputChange = (e) => {
    const { name, value } = e.target;
    let index, field;

    if (name.includes("itemsContainer")) {
      [index, field] = name.split(".").slice(-2);
      index = parseInt(index);
      if (index >= 0 && index < formData.itemContainer.length) {
        setInvoiceFormData((prevState) => {
          const newItemContainer = [...prevState.itemContainer];
          newItemContainer[index][field] = value;
          return {
            ...prevState,
            itemsContainer: newItemContainer,
          };
        });
      }
    } else {
      setFormData((prevState) => {
        return {
          ...prevState,
          [name]: value,
        };
      });
    }
  };
```

**So, here is the explanation of the above function.**

Recall the initial state, `formData` has a nested array with the key of `itemsContainer.` The itemsContainer key is an array of objects.

So, accessing the name of the key-value pairs has to be done dynamically.

For instance, if the name of one of the input elements is `itemsContainer.2.quantity,` then `name` will be "`itemsContainer.2.quantity,`" and the function will extract `index = 2` and `field = "quantity"` from this name using the `split()` and `slice()` methods.

Once the index and field are extracted, the function creates a new copy of the `itemsContainer` array using the spread operator, like this:

```javascript
const newItemContainer = [...prevState.itemContainer];
```

This creates a new array `newItemContainer` that is a copy of the previous state's `itemContainer` array.

Then, the function updates the value at the specified index and field with the new value, using the following code:

```javascript
newItemContainer[index][field] = value;
```

This updates the `newItemContainer` array with the new value for the specified index and field.

Then, the function returns a new object which holds a copy of the previous state object, but with the updated `itemsContainer` array. It then uses the spread operator to copy all properties of the previous state object. This then overrides the `itemsContainer` property with the updated `newItemContainer` array:

```javascript
return {
  ...prevState,
  itemsContainer: newItemContainer,
};
```

This creates a new state object with the updated `itemsContainer` array, which is then passed to the `setInvoiceFormData` function to update the state.

## Handling form Submit with useState

```javascript
    const [allData, setAllData] = useState([])
    
    const handleInvoiceSubmit = async (e) => {
    e.preventDefault();
    const checkEmptyInput = Object.values(formData);
    if (checkEmptyInput.some((input) => !input)) {
      alert("please fill out all fields");
      return;
    }

    setAllData((prevdata) => {
      return [...prevdata, formData];
    });
    setFormData((data) => {
      return {
        ...data,
        name: "",
        email: "",
        age: "",
        itemContainer: [
          {
            itemContent: "",
            itemQty: "",
            itemPrice: "",
          },
        ],
      };
    });
}
```

**Code explanation**

The first line of code creates a state variable called `allData` and a function to update it called `setAllData`. The initial value of `allData` is an empty array.

When the form is submitted, the `handleInvoiceSubmit` function is called and this prevents the default form submission behavior using `e.preventDefault()`.

It then checks whether any of the form fields are empty by creating an array of the form data using `Object.values(formData)`, and using the `some` method to check whether any values are falsy (i.e. empty).

If any form fields are empty, the function displays an alert message and returns, preventing the form from being submitted.

If all form fields are filled out, the function updates the `allData` state variable by creating a new array that includes the previous data (`prevdata`) and the current form data (`formData`). This is done using the `setAllData` function and the spread operator (`...`).

The function then resets the form data by calling `setFormData`. It creates a new object with the same structure as the initial form data, but with empty values. This object is returned by the function, and React updates the form to show the new values.

## Limitations to useState

As can be seen in the above code, the codebase is starting to get complex real quick. Now imagine in the real-world cases where you have multiple cross-functional forms.

This is the reason other state management options like the `useReducer` hook and `formik` exist. In this article, the `useReducer` hook will be used to manage situations like the above where there are complex state management.

Let's refactor the above code using the useReducer hook.

## Using useReducer for complex state management

The `useReducer` hook is one of the hooks introduced by React to help in cases like the above when handling state values is becoming complex.

First, it's important to understand how the `useReducer` hook works.

1. It has a reducer function
    
2. It implements the reducer function with the useReducer hook
    

### Creating a reducer function

The reducer function takes in two arguments which are:

1. The state object, and
    
2. The action object which the reducer function is supposed to implement.
    

It then returns a new state value based on the action it had undertaken

**Here is a simple syntax for the reducer function**

```javascript
const reducerFunction = (state, action) => {
switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}
```

This reducer function takes in a `state` object and an `action` object. The `action` object has a `type` property that tells the reducer what action to perform. In this example, the reducer either increments or decrements the `count` property of the `state` object, depending on the action.

### Implement useReducer in a component

The code below shows how to implement the reducer function in a component using the `useReducer` hook

```javascript
import {useReducer} from "react"

const [state, dispatch] = useReducer(reducer, { count: 0 });
```

This code initializes the `state` object with a `count` property set to `0`. The `dispatch` function can be called with an `action` object to update the `state`. For example, to increment the count, you would call `dispatch({ type: 'INCREMENT' })`.

## Refactoring the formData state with useReducer

To refactor the above code with the useReducer hook, this is how it should be.

### STEP 1 : Define an initial Form state.

```javascript
const INIT_FORM_STATE = {
  formData: {
    name: "",
    email: "",
    age: "",
    itemContainer: [
      {
        itemContent: "",
        itemQty: "",
        itemPrice: "",
      },
    ],
  },
  allData: [],
};
```

### STEP 2: Create the reducer function

### a. Handling input change

```javascript
const formReducer = (state, action) => {
  switch(action.type){
    case "UPDATE_FORM":
        const { name, value } = action.payload;
        let index, field;

    if (name.includes("itemsContainer")) {
      [index, field] = name.split(".").slice(-2);
      index = parseInt(index);
      if (index >= 0 && index <                   state.formData.itemsContainer.length) {
          const newItemContainer =         [...state.formData.itemsContainer];
          newItemContainer[index][field] = value;
          return {
            ...state,
            invoiceFormData: {
              ...state.formData,
              itemsContainer: newItemContainer,
            },
          };
        }
     } else {
        return {
          ...state,
          invoiceFormData: {
            ...state.formData,
            [name]: value,
          },
        };
      }
      break
      default:
        return state;
    }
}
```

The `formReducer` takes two parameters: `state` and `action`. The `state` parameter represents the current state of the form data, and the `action` parameter represents the action that triggered the state change.

1. Inside the `formReducer` function, a `switch` statement is implemented to handle different types of actions within the block. In this case, there is only one action type defined, which is `UPDATE_FORM`.
    
2. When the `UPDATE_FORM` action is triggered, the `formReducer` function extracts the `name` and `value` from the `payload` property of the `action` object. The `name` property shows the name of the changed form field, while the `value` property shows the updated form field's new value.
    
3. The function then determines whether the string "`itemsContainer`" is present in the name property. If it does, the method then updates the state appropriately since the `itemsContainer` array has been modified.
    
4. The code also separates the `index` and `field` properties from the name property before updating the state for each item in the `itemsContainer` array. The field property indicates the field that was modified, and the index property indicates the item's updated index.
    
5. The function then determines whether the index value is within the `itemContainer` array's parameters and, if it is, uses the spread operator to construct a new instance of the `itemsContainer` array with the updated value `(...)`. The revised value is assigned to the relevant field using array indexing, and the new copy of the `itemsContainer` array is assigned to newItemContainer.
    
6. The function then updates the `itemsContainer` property to the new copy of the itemContainer array and provides a new state object that is a replica of the previous state.
    
7. If the string "`itemsContainer`" is absent from the name property, the else statement will be executed. This indicates the updating of a form field that is not an array. The function in this instance returns a new state object.
    

### b. Handle form submit

Here's an example of how you could turn the `handleFormSubmit` function into a reducer case:

```javascript
case "SUBMIT_FORM":
  const checkEmptyInput = Object.values(state.formData);
  if (checkEmptyInput.some((input) => !input)) {
    alert("please fill out all fields");
    return state;
  }

  return {
    ...state,
    allData: [...state.allData, state.formData],
    formData: INIT_FORM_STATE
  };
```

It's using the action type "SUBMIT\_FORM".

1. First, it checks if any of the form fields are empty. If any are empty, it shows an alert message to the user asking them to fill out all fields, and then returns the current state without making any changes.
    
2. If all of the fields are filled out, it updates the state by creating a new object that includes all the data from the previous state, plus a new array that includes the submitted form data.
    
3. It also resets the form data to the initial state so that the input is cleared.
    

## STEP 3: Implement the reducer function in component

You could then dispatch this action from your component like so:

```javascript
import React, { useReducer } from "react";
const MyForm = () => {
  const [formState, dispatch] = useReducer(formReducer, INIT_FORM_STATE);

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    dispatch({ type: "UPDATE_FORM", payload: { name, value } });
  };

  const handleFormSubmit = (e) => {
    e.preventDefault();
    dispatch({
      type: "SUBMIT_FORM",
      payload: state.formData,
    });
  };

 return (
  <form onSubmit={handleFormSubmit}>
    <label htmlFor="name">Name:</label>
    <input
      type="text"
      name="name"
      value={formState.formData.name}
      onChange={handleInputChange}
    />

    <label htmlFor="email">Email:</label>
    <input
      type="email"
      name="email"
      value={formState.formData.email}
      onChange={handleInputChange}
    />

    <label htmlFor="age">Age:</label>
    <input
      type="number"
      name="age"
      value={formState.formData.age}
      onChange={handleInputChange}
    />

    {formState.formData.itemsContainer.map((item, index) => (
      <div key={index}>
        <label htmlFor={`itemContent${index}`}>Item Content:</label>
        <input
          type="text"
          name={`itemContainer.${index}.itemContent`}
          value={item.itemContent}
          onChange={handleInputChange}
        />

        <label htmlFor={`itemQty${index}`}>Item Quantity:</label>
        <input
          type="number"
          name={`itemContainer.${index}.itemQty`}
          value={item.itemQty}
          onChange={handleInputChange}
        />

        <label htmlFor={`itemPrice${index}`}>Item Price:</label>
        <input
          type="number"
          name={`itemContainer.${index}.itemPrice`}
          value={item.itemPrice}
          onChange={handleInputChange}
        />
      </div>
    ))}

    <button type="submit">Submit</button>
  </form>
);
```

1. The component uses the `useReducer` hook to manage the form state, with an initial state defined as `INIT_FORM_STATE`.
    
2. When a user types something into an input field, the `handleInputChange` function is called. The `dispatch` function is used to change the form state by running the `formReducer` function after extracting the name and value properties from the `event target`. The new form data is then returned in a new state object by the `formReducer` function.
    
3. When a form is submitted by the user, the `handleFormSubmit` function is called. The `formData` object from the state is sent into an action of type `SUBMIT_FORM` that is dispatched. The state's `allData` property is then updated by the formReducer function using the new form data.
    
4. The `useReducer` hook's `formState` object is used to fill in the input fields and update the state if something changes. For each item in the array, an input field is dynamically rendered by mapping over the `formState.formData.itemsContainer` array.
    
5. A `submit` button is then displayed to send the form. The `handleFormSubmit` function is called when the button is pressed. The `formReducer` function receives the `formState` object and updates the `allData` property with the newly submitted form data.
    

## Why the useReducer hook is better

The above is an instance of the implementation of managing complex states such as a nested array with the `useReducer` hook. The core difference between the `useReducer` implementation and a `useState` implementation is that it allows you to know exactly when a specific function is executed.

Unlike a `useState` hook where you're always updating the initial state, it can get easily confusing to know the current state value as the app gets complex. So, the `useReducer` helps to keep track of when a specific action type is modifying the initial state. This helps to keep the code based organized and easily scalable.

## Conclusion

This article explored how to implement the useReducer hook for complex state management in react. The next part of this article will be focused on how to use the formik library for complex state management. You can [follow me here](https://hashnode.com/@kodervine) to get notified when the next article is published. Happy coding!!

## Additional resources

[https://beta.reactjs.org/learn/state-a-components-memory](https://beta.reactjs.org/learn/state-a-components-memory)

[https://beta.reactjs.org/reference/react/useState](https://beta.reactjs.org/reference/react/useState)

[https://beta.reactjs.org/reference/react/useReducer](https://beta.reactjs.org/reference/react/useReducer)