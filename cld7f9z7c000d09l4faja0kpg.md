# How to resolve "Enoent: can't find module" error in react

Error messages are a given for programmers. Although these errors can be annoying, they are an essential tool for troubleshooting your code. In this article, I'll examine two different kinds of error messages you could see when using React and go over how to troubleshoot and resolve them.

Syntax errors are the first category of error messages. When the code you have developed does not follow the specifications of the programming language you are using, it triggers this bug.

For instance, you might have used the incorrect kind of `quotation marks` or forgotten to use a semicolon at the end of a code. These mistakes are typically highlighted in your code editor and are not too difficult to correct.

**Runtime errors** make up the second category of error messages. This happens when your code executes flawlessly, but an error occurs. You might, for instance, be attempting to access a property of a nonexistent object. These errors are more frequent, but they can also be more challenging to identify and correct.

The `"can't find module"` bug is a frequent runtime error you might come into when working with React. An instance of this is the error in the image below -

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674393784910/5fab391a-6961-448c-967c-2bc2460b03fd.png align="center")

This error appears when you try to run a command on the terminal like `npm start"` or "`npm run dev`" (on vite), but the system cannot locate the necessary module. This issue is frequently caused by running the command on the parent file rather than the most recent folder.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674393910409/5ebf2bf1-e044-4238-8ee7-fd5c084bdfd1.png align="center")

The fix for this error is relatively simple. You just need to navigate to the newest folder using the `cd` command on your terminal and then run the `npm start` or `npm run dev` command again.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674393929276/1d17d693-2aba-4af3-bcfd-793aeb4b71ce.png align="center")

In most cases, I just move the newly created React app from its parent folder and delete the parent folder. This helps me avoid forgetting to start the development on the parent folder.

In conclusion, error messages are an inevitable part of programming, but they are also an important tool for debugging and troubleshooting your code. I hope this post has been helpful if you've ever encountered this error.