---
layout: post
title: "Setting up Java in Gentoo"
---

# Setup

This is documentation on installing java onto my computer. In this case, i will be doing it on Gentoo Linux. I will be going blind on this so I may make some mistakes along the way so bear with me.

I will be following a guide as shown. I'll go into root and install the JDK

Something not many people may know about myself is that I took Java classes in college. Admittedly, I haven't coded anything in Java since then.



{% highlight console %}
yasss /home/ren # emerge --ask --oneshot virtual/jdk

{% endhighlight %}

Now we see this
{% highlight console %}
 yasss /home/ren # emerge --ask --oneshot virtual/jdk

 * IMPORTANT: 12 news items need reading for repository 'gentoo'.
 * Use eselect news read to view new items.

 * Last emerge --sync was 117d 11h 5m 39s ago.

 * IMPORTANT: 3 config files in '/etc/portage' need updating.
 * See the CONFIGURATION FILES and CONFIGURATION FILES UPDATE TOOLS
 * sections of the emerge man page to learn how to update config files.

These are the packages that would be merged, in order:

Calculating dependencies... done!
[ebuild  N     ] sys-apps/baselayout-java-0.1.0-r1 
[ebuild  N     ] app-eselect/eselect-java-0.5.0 
[ebuild  N     ] dev-java/java-config-2.3.1  USE="-test" PYTHON_TARGETS="python3_10 -python3_8 -python3_9" 
[ebuild  N     ] dev-java/openjdk-bin-17.0.5_p8  USE="alsa cups -headless-awt (-selinux) -source" 
[ebuild  N     ] virtual/jdk-17  USE="-headless-awt" 

Would you like to merge these packages? [Yes/No] 

{% endhighlight %}

We're gonna type "yes"


{% highlight console %}
...

 * Final size of build directory: 4 KiB
 * Final size of installed tree:  4 KiB


>>> Installing (5 of 5) virtual/jdk-17::gentoo

 * GNU info directory index is up-to-date.

 * IMPORTANT: 5 config files in '/etc' need updating.
 * See the CONFIGURATION FILES and CONFIGURATION FILES UPDATE TOOLS
 * sections of the emerge man page to learn how to update config files.

 * IMPORTANT: 12 news items need reading for repository 'gentoo'.
 * Use eselect news read to view new items.

{% endhighlight %}

Now we have successfully installed it.



We will install an IDE. I use VsCode (RIP Atom) but I remember using BlueJ and Eclipse when I was coding in Java. I'll be installing Intellij: Community Edition. Something completely unknown to me but I'll use it anyways.







## Additional Links
- https://wiki.gentoo.org/wiki/Java

