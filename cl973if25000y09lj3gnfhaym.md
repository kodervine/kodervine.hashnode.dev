# On JavaScript reduce method

When I watched a lesson on JavaScript Array reduce method and heard the instructor say it's one of the hardest, I simply shrugged - "meh"

That's because at first, I understood its basic process. It reduces the values in the array parameters to a single digit.

Simple right?

Nope!

A few JavaScript exercises later and I'm nearly smacking my head against the wall - ***"I can't seem to understand this reduce method anymore."***

Thanks to web development simplified, I finally understood the method

\*\*So here's a simple function to use reduce: \*\*

```javascript
function reduceArr((){

})
```

This can accept 4 parameters. But for purposes of clarity, let's start with the commonly used parameters.

2 parameters alone -

```javascript
function reduceArr ((total, iteration){

 }, initialElement)
```

So, this method accepts a callback function and another parameter.

The last parameter serves as the starter "total" parameter

That is, the initialElement parameter is the element that will be the initial total in the first iteration.

Which is usually "0"

So, in the next iteration, the next element of the array ends up in the iteration parameter, and it adds to the total.

And it continues the loop until the end of the array.

So to finalize,

```javascript
function reduceArr((total, iteration) => { 
return total

},0)
```

To run the function -

```javascript
reduceArr([1,2,3,4])
```

The end result should be `10`

[Watch Web simplified tutorial here](https://www.youtube.com/watch?v=s1XVfm5mIuU&feature=youtu.be)