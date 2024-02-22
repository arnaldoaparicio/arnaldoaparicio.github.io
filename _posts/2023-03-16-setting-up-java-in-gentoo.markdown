---
layout: post
title: "Setting up Java on Gentoo Linux"
permalink: /blog/setting-up-java-in-gentoo/
---

# Setup

This is documentation on installing Java onto my Gentoo Linux machine. I will be going blind on this with nothing other than the documentation I'm reading.

Something not many people may know about myself is that I took Java classes in college. Admittedly, as of the time I'm writing this, I haven't coded anything in Java since then.

This isn't so much a guide or tutorial but just documenting the process I had to take to install everything needed.

## Installing OpenJDK

I'll be installing OpenJDK, Oracle's open-source JDK. I'm gonna do it as root.
{% highlight console %}
host /home/ren # emerge --ask --oneshot virtual/jdk

{% endhighlight %}

Now we see this
{% highlight shell %}
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

I'm gonna type "yes".


{% highlight shell %}
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

OpenJDK has been successfully installed!

## Installing an IDE

I'll be installing an IDE. I use VSCode (_RIP Atom_) but I remember using BlueJ and Eclipse when I was writing Java code in college. I'll be installing IntelliJ IDEA: Community Edition, which is something completely unknown to me but I'll use it anyways.

While remaining as root, I'll enter the following:
{% highlight console %}
host /home/ren # emerge --ask dev-util/idea-community
{% endhighlight %}

And let the installation begin!

{% highlight shell %}
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

Sheeesh! The installation took about two hours!!

## Results

At this point, I will be doing the Underpants Gnome logic and abstract the process of writing Java code.

<img src="https://i.imgur.com/ELE7t9h.png">

And here are the results:

<img src="https://i.imgur.com/MDlvSkR.png">

Great success!


So after finishing all of this, it was rather painless albeit very time consuming (wait times on installing packages can be brutal). This wasn't so bad after all.

Now my journey into relearning Java begins!

## Additional Links
- [https://wiki.gentoo.org/wiki/Java](https://wiki.gentoo.org/wiki/Java)
- [https://packages.gentoo.org/packages/dev-util/idea-community](https://packages.gentoo.org/packages/dev-util/idea-community)