# On map method in Javascript

If you want to create a new array by making adjustments to an initial array, you can use the \*\* map method\*\* to do so

The map method can be used in an array of just strings, or even array of objects

So this is how it works -

Let's say you have an array (of numbers) saved into a variable called `"wholeNums"`

`const wholeNums = [1,2,3,4,5];`

And you want to return each of the elements, but multiplied into 3

## This is how the map method works with it -

### Step 1

You create a new variable for it and call the map method to the initial variable

```javascript
const newWholeNums = wholeNums.map( )
```

### Step 2

Inside the map, you have to add a callback function. Basically, add argument that you want to run inside the map method.

So it becomes this -

```javascript
const newWholeNums = wholeNums.map(function ( ) {


})
```

### Step 3

Now, this function is supposed to have an argument to represent each of the elements in the wholeNums array

```javascript
const newWholeNums = wholeNums.map(function (nums ) {


})
```

### Step 4

This is where we add the **"return"** logic, whether it's multiplication, if statement, or any other command we want to run.

Let's return `"multiplication of each element by 3"`

const newWholeNums = wholeNums.map(function (nums ) {

return nums \* 3

})

If we console.log, each element in the newWholeNums will become multiplied by 3

```javascript
const newWholeNums = [3,6,9,12,15]
```

Now, it doesn't change the initial variable (wholeNums) because the map method is saved onto a new variable (newWholeNums)

## We can also use Array map in an object

Let's say the object is -

```javascript
const meal = [

    {Breakfast: Bread, Tea: Night},

    {Breakfast: Egg, Tea: Night},

]
```

And we want to map and get the value of each of the objects. Let's take breakfast for instance

Same method as above

```javascript
const morningMeal = meal.map(function (food ) {

    return food.breakfast

})
```

The result will be

`morningMeal = [Bread, Egg]`

The **"food"** argument in the callback function means each of the object literals as a whole

Therefore, adding the `"property" value (.breakfast),` thus indicates that we want to return just the value of the key

From the above, one can say that the map method is probably the most used array method in real life projects

If you want to know more use cases about the map method, [read this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)