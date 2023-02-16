# Firebase "auth/unauthorized-domain" Error and how to fix it

Sometimes, little errors can cause major problems. I was just setting up my firebase authentication with Google and I saw this error.

<iframe src="https://share.descript.com/embed/YxC0MiOXQ9c" width="640" height="360"></iframe>

**"FirebaseError: Firebase: Error (auth/unauthorized-domain)"** error message.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674292965507/81544a25-ee96-410d-942c-a9ff22111faf.png align="center")

This error message can be very annoying because it can stop your development work and leave you perplexed. If you've ever encountered this error, you don't need to worry.

Let's first analyze what this error message is trying to tell us. In essence, it's saying the domain being used for authentication isn't allowed. In other words, the authentication settings for the Firebase project do not include the domain being used in the list of allowed domains.

Several things, so if you had mistakenly utilized the wrong domain or improperly set up your Firebase project, that could have caused this. So how can we resolve this problem?

The solution is fairly straightforward:

Now, if your website is being hosted on [`http://127.0.0.1:5173/`](http://127.0.0.1:5173/)rather than [`localhost.com/5173`](http://localhost.com/5173) that is one common reason for this problem. This is a little but crucial distinction, as Firebase might not accept [`http://127.0.0.1:5173`](http://127.0.0.1:5173/) as a legitimate domain.

So, you just need to change your localhost

<iframe src="https://share.descript.com/embed/KmJEqM8mhOL" width="640" height="360"></iframe>

If your website isn’t on [`localhost`](http://localhost) and you are facing this issue, it means the domain being used should be added to the list of authorized domains in the Firebase project.

In the Firebase project's authentication settings, the domain in question needs to be added to the list of approved domains. You can accomplish this by going to the Firebase project's settings, selecting the **"Authentication"** tab, and adding the required domain to the list of permitted domains as seen in the images below.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674293244263/e9efceee-def3-4fdc-bcab-74233dcf796b.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674293266056/d5dfd0e9-485e-49d5-92f1-63d7e89013cd.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1674293286203/ce9e1b6c-69aa-4383-b22e-c44fe568cac9.png align="center")

In conclusion, the "FirebaseError: Firebase: Error (auth/unauthorized-domain)" is a common error message that developers might face while working with Firebase. The error message indicates that the domain used for authentication is not authorized. Hopefully, this article has helped resolve this error for you