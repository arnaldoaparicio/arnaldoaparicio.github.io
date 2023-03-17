---
layout: post
title: "Setting up Java in Gentoo"
---

# Setup

This is documentation on installing Java onto my computer. In this case, i will be doing it on Gentoo Linux. I will be going blind on this so I may make some mistakes along the way so bear with me.

Something not many people may know about myself is that I took Java classes in college. Admittedly, I haven't coded anything in Java since then.

This isn't so much a guide or tutorial but just documenting the steps I had to take to install everything needed.

I will be following a few resources that will be linked on the bottom of this page. I'll go into root and install the JDK.




## Installing OpenJDK


I'll be installing OpenJDK, Oracle's open-source JDK. I'm gonna do it as root.
{% highlight console %}
host /home/ren # emerge --ask --oneshot virtual/jdk

{% endhighlight %}

Now we see this
{% highlight console %}
 host /home/ren # emerge --ask --oneshot virtual/jdk

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


## Installing an IDE

I'll be installing an IDE. I use VSCode (RIP Atom) but I remember using BlueJ and Eclipse when I was writing Java code in college. I'll be installing IntelliJ IDEA: Community Edition, which is something completely unknown to me but I'll use it anyways.


Now while remaining as root, I'll enter the following:
{% highlight console %}
host /home/ren # emerge --ask dev-util/idea-community
{% endhighlight %}

And let the installation begin.

{% highlight console %}
...
>>> Installing (12 of 12) dev-util/idea-community-2022.2.3::gentoo

>>> Recording dev-util/idea-community in "world" favorites file...

 * Messages for package dev-lang/rust-1.65.0:

 * Rust installs a helper script for calling GDB and LLDB,
 * for your convenience it is installed under /usr/bin/rust-{gdb,lldb}-1.65.0.

 * Messages for package x11-libs/gtk+-2.24.33-r2:

 * Please install app-text/evince for print preview functionality.
 * Alternatively, check "gtk-print-preview-command" documentation and
 * add it to your gtkrc.

 * GNU info directory index is up-to-date.

 * IMPORTANT: 5 config files in '/etc' need updating.
 * See the CONFIGURATION FILES and CONFIGURATION FILES UPDATE TOOLS
 * sections of the emerge man page to learn how to update config files.

 * IMPORTANT: 12 news items need reading for repository 'gentoo'.
 * Use eselect news read to view new items.


{% endhighlight %}

Sheeesh. It took almost two hours to install Intellij.


## Results

At this point, I will be doing the underpants garden gnome 

1. Install JDK
2. Install Intellij
3. ???
4. PROFIT    <------ Running Java code




## Additional Links
- https://wiki.gentoo.org/wiki/Java

