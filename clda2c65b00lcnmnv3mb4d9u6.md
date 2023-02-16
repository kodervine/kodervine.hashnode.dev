# How to resolve "Typeerror: cannot read properties of null reading map" in React

## Introduction

In this article, we will be discussing a common error that may arise when building a React project. The error that may occur is the `TypeError: Cannot read property 'map' of null` which occurs when an array of elements set in state value evaluates to null when the map method is called on it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674475572943/de4d0bbf-25a4-45aa-af21-f9ee989d3b28.png align="center")

### Code snippets

For this article, I will be using the following code snippets to explain and resolve this error.

```javascript

  const [allGratitude, setAllGratitude] = useState(null);
  const [gratitude, setGratitude] = useState({
    gratitudeForm: "",
    tag: "",
  });

  const handleGratitudeFormInputChange = (e) => {
    const { name, value } = e.target;
    setGratitude({
      ...gratitude,
      [name]: value,
    });
  };

  const handleGratitudeFormSubmit = (e) => {
    e.preventDefault();
    setGratitude({ gratitudeForm: "", tag: "" });
    if (gratitude.gratitudeForm && gratitude.tag) {
      setAllGratitude((prevArray) => [...prevArray, gratitude]);
      e.currentTarget.reset();
    } else {
      alert("fill all fields");
    }
  };
```

### **Explanation of Sample Code**

The code snippet has two main state variables, `allGratitude` and `gratitude`. The `allGratitude` state variable is an array that holds all of the gratitude submitted by the user and set to `null`. The `gratitude` state variable is an object that holds the current form input of the user.

The `handleGratitudeFormInputChange` function is used to handle changes to the form input. It takes in the event object and destructures the `name` and `value` properties. It then updates the `gratitude` state variable using the spread operator to keep the previous state and only update the specific input that has changed.

The `handleGratitudeFormSubmit` function is used to handle the form submission. It takes in the event object and prevents the default form submission behavior. It then checks if the `gratitudeForm` and `tag` properties of the `gratitude` state variable have a truthy value. If they do, it concatenates the current gratitude to the `allGratitude` array and resets the form. If not, it alerts the user to fill in all fields.

### **Rendering the state in a component**

```javascript
import { useGlobalContext } from "../context";

const Dashboard = () => {
  const { allGratitude } = useGlobalContext();

  return (
    <div className="dashboard-container">
      {allGratitude.map((items, index) => {
        const { gratitudeForm, tag } = items;
        return (
          <article key={index}>
            <p>{gratitudeForm}</p>
            <button>{tag}</button>
          </article>
        );
      })}
    </div>
  );
};

export default Dashboard;
```

### Explanation of rendered component

This code snippet utilizes a custom `useGlobalContext` hook to access the `allGratitude` state variable from the global context.

The component then uses a `map` method to loop through the `allGratitude` array and renders an `article` element for each item in the array. It destructures the `gratitudeForm` and `tag` properties from each item in the array and uses them to render the `p` and `button` elements respectively.

The `key` attribute is added to the `article` element to optimize the performance of the component when it updates and re-renders.

## Reasons for the error

It's important to note that since the `allGratitude` state variable is set to `null`, this code will throw an error because the `map` method can only be called on arrays and not on `null` values.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674476065116/1d5f414c-ece7-4761-81ce-c16438eee563.png align="center")

The `TypeError: Cannot read property 'map' of null` error that occurs on this is because the `allGratitude` state variable is set to `null` and the `map` method is called on it. Hence, when your page renders, it automatically sees the allGratitude state array already in null, and throws the error immediately as the `map` method can only be called on arrays and not on null values.

## Solution 1 - Set the initial state to empty array

To avoid this error, either the `allGratitude` state variable should be set to an empty array `[]` instead of `null` when the component first renders. This way, when the `map` method is called, it is called on an empty array and will not throw an error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674476349593/b02e1e3c-31b9-4192-a3c2-5267c0b6c555.png align="center")

**As seen below.**

<iframe src="https://share.descript.com/embed/5Ag41ArIKdy" width="400" height="360"></iframe>

## Solution 2 - conditional rendering

The second solution is to only render the component that utilizes the `map` method when the `allGratitude` state variable has a truthy value. This can be achieved by using a conditional rendering statement, such as an `if` statement, or the `logical AND` operator that checks if the `allGratitude` state variable is truthy before rendering the component that uses the `map` method.

So if your state variable is still set to null as seen below:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674476472336/999ef6db-333f-4530-a413-caeeb7bbe4b0.png align="center")

Your rendered component should look like this with the `logical AND` operator

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674476606356/18053b43-5163-4a1c-afcd-dae11af8395d.png align="center")

So, the above code means that the map method will run only when there are elements present in the `allGratitude` state variable.

**As seen below.**

<iframe src="https://share.descript.com/embed/NdiEH4I7N5p" width="400" height="360"></iframe>

## Conclusion

It's important to keep in mind that this error can occur when working with arrays and not specifically for the `useState` hook and that these solutions can be applied in similar situations.