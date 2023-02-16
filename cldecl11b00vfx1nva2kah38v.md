# Bug: Tailwind project not deploying properly on Github pages

## Problem

So, I deployed a project in which I used tailwind CSS to Github pages. However, I encountered 2 bugs in the process.

Firstly, the deployed website was showing only the ReadMd file.

Then, when I finally solved the "ReadMd" issue, my images were not displaying and my JavaScript commands were not working on the deployed website as well.

If you've ever dealt with these 2 bugs with tailwind deployment on Github pages, this article covers the solution to both.

## Resolve tailwind deployment showing only Readme.MD file

First, check where your `index.html file` is located.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674565793881/80354b2b-e963-4ce6-82ee-cc7e88401310.png align="center")

My `index.html` file was within the `src` folder. Therefore, the GitHub page was only deploying the ReadMe file as that was the only file in the root folder.

So, you simply need to move your `index.html` to the root folder

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674566166408/25a3d307-42c1-4360-8b0b-9d95e25acbd6.png align="center")

## Resolve Images and Javascript not working on deployment

### Option 1 -

Firstly, you can check the source of your images. GitHub views uppercase and lowercase alphabets as different entities

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674566135950/35d0f562-2958-4052-8821-c6a9e4fa3c65.png align="center")

So if your PNG or JPEG is in uppercase as indicated, you should change it to the lowercase "jpeg" or "png"

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674566244163/8796ce35-c2c4-429d-95c3-af3cb3ed5b08.png align="center")

### Option 2 -

If the first option didn't work for you, here is the second solution to check:

Check if your src starts with the `"/"` symbol

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674566416651/d5b5e717-348a-4fc0-8779-4aef8f01023c.png align="center")

That's because GitHub page is looking for a folder that starts with the forward slash among the folders.

Hence, you just need to remove the `"/"`. So the link to your file should have the path to the files without the first slash.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674566575853/0da0fa84-eafb-489d-90a9-12d37058e252.png align="center")

And this goes for both images and the JavaScript files.

## Conclusion

I hope this article has been helpful for this problem. Kindly Like and spread the word to others