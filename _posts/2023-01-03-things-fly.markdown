---
layout: post
permalink: /things-fly/
---

<p><img src="https://i.imgur.com/Dctgbys.png"></p>
# fly.io
<p>Considered to be a great alternative to Heroku.</p>
<p>Since Heroku has changed the way they do business, many have scrambled to other alternatives. One of which that is heavily pushed is fly.io. </p>
<p>I was primarily using Heroku and much like a lot of other people, I decided to try fly.io and, in my opinion, it's a great alternative to Heroku. It's very easy to use and the learning curve isn't that steep. It has been an overall joy to use. Along the way, I have run into some issues when deploying so I have a bit of info that may be useful.
</p>
<p>These are mostly applicable to Rails apps but these tips may help you better understand what and where to look for.</p>
<br>
## 1. If you're deploying a Rails app, you may get a Javascript runtime error.
<p>To fix this, go into your "Docker" file and head down to the line that starts with "ARG DEPLOY_PACKAGES" and enter "nodejs".</p>
<p><img src="https://i.imgur.com/1epjqsh.png"></p>
<p>Then deploy the app again and it should start work.</p>
<br>

## 2. Deploying a Rails API may fail immediately.
<p>To fix this go to "lib/tasks/fly.rake" and find this line
</p>
<p><img src="https://i.imgur.com/mOSUHA9.png"></p>
<p>and change to </p>
<p><img src="https://i.imgur.com/OASEd7D.png"></p>
<p>Once those changes are made, deploy your app again.</p>
<br>
## 3. If you're using a VPN, it MAY AFFECT YOUR DEPLOYMENT.
<p>I'm not sure of the technicalities behind this, but my guess is that your location
based on your VPN settings causes issues while you're choosing you location upon setting
up deployment to fly.io.
</p>
<p>One time, I was teaching a friend how to use fly.io. While I was deploying an app, 
        it kept giving me errors and it wouldn't deploy. I realized that my VPN was on and 
        I suspected that it may have been causing my issue.
</p>
<p>All I did to fix the problem was to turn off my VPN and deploy my app again and it deployed successfully.</p>

<br>
<br>

<p>Hopefully this info may be useful when using fly.io. Give fly.io a try. It's been a pleasure to use. If there's more info I find, I'll
    be sure to update this post to include it.
</p>