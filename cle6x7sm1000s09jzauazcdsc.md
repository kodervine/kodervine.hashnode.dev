# Debugging as a newbie in tech: A web developer's Tale on Tailwind deployment bugs with Github pages

## **The Golden Rule of Programming.**

Your job as a programmer is to "create bugs and find solutions to the self-induced bugs."

This is exactly what I heard a YouTuber say as he encouraged programmers to learn debugging tools in their [I.D.Es](http://I.D.Es)

Actually, my first reaction when I heard this was, "Wait what??"

I mean, **I thought programmers are supposed to convert business solutions into software**.

I didn't quite wrap my head over the statement at first until I started building projects with Javascript.

At this point, it's safe to say that I spend more time debugging my projects than actually adding features.

Okay, let's start with the golden rule of programming!

<iframe src="https://share.descript.com/embed/8gSkRUm3heI" width="320" height="320"></iframe>

But because optimization have been drilled into me over and over, I'd often find myself in this debugging ditch.

![Cartoon image of creating extra bugs from solving bugs](https://cdn.hashnode.com/res/hashnode/image/upload/v1676539422547/8c969f5f-6979-4c7c-b100-14e630cac73e.jpeg align="center")

## The Power of Breaks...or Not

You hear this advice all the time: "Take a break! When you sleep, your brain will ruminate over the solution and you will wake up rejuvenated to solve it."

I guess my brain does the exact opposite because this image perfectly describes what it does instead.

![An illustration of a person's brain reminding them of a bug before they sleep](https://cdn.hashnode.com/res/hashnode/image/upload/v1676538995282/bb40cc95-a7db-48b9-8653-e82e2f328108.jpeg align="center")

It doesn't help that JavaScript takes utmost pleasure in not explicitly indicating some of these errors.

So, I figured that I should write articles about the bugs I've encountered as a way to improve my technical writing skills.

But as I wrote the first article, the second, third, fourth, it then hit me.

"Why not start a series on hashnode and compile these debugging tales?"

So I did. I created a series, and guess the title?

Yup! **"**[**Debugging Jungle**](https://kodervine.hashnode.dev/series/errors-and-bugs)**"**

That's because I feel like I'm in a jungle whenever I'm stuck with a bug.

## The Debugging Jungle Series

At first, the articles were supposed to be for me, in case I inevitably run into them in future projects.

But I figured I should make the articles more professional with explicit examples since other programmers might run into such errors as well.

Now, I have 10 articles in the series. Feel free to check them out here - [https://kodervine.hashnode.dev/series/errors-and-bugs](https://kodervine.hashnode.dev/series/errors-and-bugs)

## Frustrating Debugging Adventure

While I currently have 10 articles on debugging, here is a quick rundown of the most frustrating debugging adventure as a beginner in tech.

### [Tailwind not deploying properly on github pages](https://kodervine.hashnode.dev/bug-tailwind-project-not-deploying-properly-on-github-pages)

Okay, I confess that I brought this bug upon myself.

Because why did I not consult the documentation on my very first attempt at deploying a website?

Definitely because of an unspoken rule in programming. *"Before reading the documentation, try to figure out the solution on your own. Even though you've never used the tool before."*

![Tweet by programming humour showing a person's leg not wearing slippers properly. It can be likened to programmers not reading documentations at first](https://cdn.hashnode.com/res/hashnode/image/upload/v1676539912339/976755fc-b027-4436-97fc-a5e5d6251155.jpeg align="center")

Or even, don't read the documentation at all and spend all day feeling frustrated about why a bug is disrupting my day.

In this case, my website wasn't deploying properly both on Netlify and GitHub pages. I eventually decided to concentrate on GitHub pages.

So, I was attempting to deploy a Tailwind CSS project on `Github` pages, only to be met with not one, but two bugs.

First, I faced the dread of the "ReadMe" issue as the deployed website showed only the `ReadMe` file.

The solution was considerably simple: **"move the index.html" file to the root folder.**

And just when I thought it was safe to breathe a sigh of relief, the **"Images and JavaScript"** files reared their ugly heads.

"**Images not displaying on deployment, JavaScript commands not working,**" it was like a nightmare come to life.

The worst part was I’d shared the link to the supposed deployed page with a recruiter.

So, I was desperate to find a solution before anyone actually clicks the link.

I scoured stack overflow trying to deduce this solution. Then, I hadn’t hopped on utilizing chatGPT cos I thought it was yet another shiny new product in the tech industry.

But eventually, I discovered two possible solutions to this problem. Ensure that image sources are in lowercase (because apparently, Github is case-sensitive like a grammar-nazi English teacher), or remove the forward slash at the beginning of the file path (because even a forward slash can cause chaos and destruction in the world of web development).

[You can read the article I wrote about it with pictorial solutions here:](https://kodervine.hashnode.dev/bug-tailwind-project-not-deploying-properly-on-github-pages)

## Conclusion

My experience with deploying a Tailwind CSS project on Github pages was a challenging one, but not without lessons learned.

As a web developer, I have encountered numerous technical errors, but this one was particularly frustrating as I was still learning how to use platforms like Github pages.

Without any indication in my console, it felt like I was searching for a needle in a haystack. Despite the difficulties, I remain undaunted and determined to continue my journey as a web developer. And even more motivated to create articles about the bugs I’ve faced in my debugging series. You can follow me [here for more debugging articles.](https://hashnode.com/@kodervine)

If you've encountered similar frustrating technical errors or have your own debugging tales to share, feel free to comment below and let me know your experience.