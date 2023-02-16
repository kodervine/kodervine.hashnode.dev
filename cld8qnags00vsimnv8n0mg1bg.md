# Error: Identifier has already been declared in javascript

Recently, I encountered an error message when trying to create a new variable on my JavaScript file. I was using the "`const`" or "`let`" keyword to declare the variable, but I kept getting an error message.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674394877811/dc16ad72-c4a8-402e-9f95-8660703756d1.png align="center")

So, I decided to use the `var` keyword declaration instead, and it was working fine. However, this bug continued to perturb me. I couldn't understand why the `const` and `let` keywords weren't working, and it was only until I realized this very simple mistake that I had made unknowingly.

I had linked the script tag twice in my HTML file, once on the `head tag`, and once inside the `body tag`.

So what was happening under the hood was that it was defining the same variable twice, which throws an error with either `const` or `let`, but it was allowed with the "var" keyword because var allows itself to be overridden with declarations.

For example, consider the following code:

```xml
<html>
  <head>
    <script src="/script.js"></script>
  </head>
  <body>
      <p is="paragraph">Javascript bug</p>
      <div id="javascript-div"></div>
    <script src="script.js"></script>
  </body>
</html>
```

In the above code, I have linked the script.js file twice, once in the head tag and once in the body tag. So, if in the script.js file, I have the following code:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674395097734/e233f898-8b0e-42ec-9aa2-bce64f5310d9.png align="center")

It will throw an error because we cannot reassign a value to a variable declared with "const" keyword. But if I use the "var" keyword to declare the variable, it will not throw an error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674395155175/0d54044d-6ab8-4f35-bad0-8a86b88210f3.png align="center")

I stopped getting the error after I removed the script file from the head tag

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674395207827/50ec43dd-a60d-47f8-8fd7-ce55e7e24a3f.png align="center")

So, that's it. If you ever come across this error, check your script tag to confirm if you haven't linked your script tag twice. It's a small mistake, but it can cause big problems. Always double-check your code and make sure that everything is in the right place.

This will save you a lot of time and frustration in the long run. Remember, programming is all about attention to detail, and taking the time to understand and fix bugs will make you a better and more efficient programmer.