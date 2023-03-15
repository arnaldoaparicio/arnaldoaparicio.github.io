---
layout: post
title: "Useful fly.io tips"
permalink: /things-fly/
---

<img src="https://i.imgur.com/Dctgbys.png">
# fly.io

Considered to be a great alternative to Heroku.

Since Heroku has changed the way they do business, many have scrambled to other alternatives. One of which that is heavily pushed is fly.io. 

I was primarily using Heroku and much like a lot of other people, I decided to try fly.io and, in my opinion, it's a great alternative to Heroku. It's very easy to use and the learning curve isn't that steep. It has been an overall joy to use. Along the way, I have run into some issues when deploying so I have a bit of info that may be useful.

These are mostly applicable to Rails apps but these tips may help you better understand what and where to look for.
<br>
## 1. If you're deploying a Rails app, you may get a Javascript runtime error.

To fix this, go into your "Docker" file and head down to the line that starts with "ARG DEPLOY_PACKAGES" and enter "nodejs".
<img src="https://i.imgur.com/1epjqsh.png">
Then deploy the app again and it should start work.
<br>

## 2. Deploying a Rails API may fail immediately.
To fix this go to "lib/tasks/fly.rake" and find this line

<img src="https://i.imgur.com/mOSUHA9.png">
and change to 
<img src="https://i.imgur.com/OASEd7D.png">
Once those changes are made, deploy your app again.
<br>
## 3. If you're using a VPN, it MAY AFFECT YOUR DEPLOYMENT.
I'm not sure of the technicalities behind this, but my guess is that your location
based on your VPN settings causes issues while you're choosing you location upon setting
up deployment to fly.io.

One time, I was teaching a friend how to use fly.io. While I was deploying an app, 
        it kept giving me errors and it wouldn't deploy. I realized that my VPN was on and 
        I suspected that it may have been causing my issue.

All I did to fix the problem was to turn off my VPN and deploy my app again and it deployed successfully.

## 4. Make sure to set your secrets
There may be a time where your deployment will fail and you may feel like you configured everything correctly. Perhaps you haven't added your credentials.

For instance, the backend of my MUGEN DB project uses Amazon S3. I spent over an hour trying to fix my failing deployment and for the life of me, couldn't figure out the problem. I stared long and hard at my logs. As a last resort, I added my Amazon credentials to my project. And guess what??

It worked!

For reference, be sure to set your credentials, API keys, etc. in your terminal as shown

{% highlight console %}
$ flyctl secrets set <VARIABLE>=<YOUR_SECRET>
{% endhighlight %}

For more on how fly.io secrets, over here for more info: [https://fly.io/docs/reference/secrets/](https://fly.io/docs/reference/secrets/)

<br>
<br>

Hopefully this info may be useful when using fly.io. Give fly.io a try. It's been a pleasure to use. If there's more info I find, I'll
    be sure to update this post to include it.

