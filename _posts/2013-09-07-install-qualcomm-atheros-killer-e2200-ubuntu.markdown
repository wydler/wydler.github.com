---
layout: post
title:  "Install Quallcomm Atheros Killer E2200 Chip on (K)Ubuntu 13.04"
date:   2013-09-07 11:58
categories: linux driver
tags: linux driver install killer e2200
---

In this post I will show you, how to install the Quallcomm Atheros Killer E2200 ethernet driver on (K)Ubuntu. I'm using a MSI Z87-G45 GAMING mainboard.  
Unfortunately there are no drivers included in (K)Ubuntu 13.04 for this ethernet chip, so you need to compile and install it on your own.

First you need to download the latest compat-drivers from [kernel.org][compat-drivers] and extract it.

{% highlight bash %}
$ wget https://www.kernel.org/pub/linux/kernel/projects/backports/2013/03/04/compat-drivers-2013-03-04-u.tar.bz2
$ tar -xjvf compat-drivers-2013-03-04-u.tar.bz2
{% endhighlight %}

Now you need to patch the driver with this file [Download][e2200-support-mod]. First we make a test run to see if there a problems with the patch.

{% highlight bash %}
$ patch -p1 --dry-run < e2200-support-mod.patch
{% endhighlight %}

If no error occur, you should see the following output.

{% highlight bash %}
patching file compat-drivers-2013-03-04-u/drivers/net/ethernet/atheros/alx/alx_ethtool.c
patching file compat-drivers-2013-03-04-u/drivers/net/ethernet/atheros/alx/alx_main.c
Hunk #1 succeeded at 57 (offset 2 lines).
Hunk #2 succeeded at 1017 (offset 1 line).
patching file compat-drivers-2013-03-04-u/drivers/net/ethernet/atheros/alx/alx_reg.h
{% endhighlight %}

Then we could start the patch (it should show the same output as above).

{% highlight bash %}
$ patch -p1 < e2200-support-mod.patch
{% endhighlight %}

The next step is to switch to the compat-driver director and select the required `alx` driver.

{% highlight bash %}
$ cd compat-drivers-2013-03-04-u/
$ ./scripts/driver-select alx
Processing new driver-select request...
Backing up makefile: Makefile.bk
Backup exists: Makefile.bk
Backing up makefile: drivers/net/ethernet/broadcom/Makefile.bk
Backing up makefile: drivers/net/ethernet/atheros/Makefile.bk
Backup exists: Makefile.bk
Backup exists: Makefile.bk
Backup exists: drivers/net/ethernet/broadcom/Makefile.bk
{% endhighlight %}

The last step is to compile the driver and install it.

{% highlight bash %}
$ make
$ sudo make install
{% endhighlight %}

To activate the driver run the following commands or just reboot your computer.

{% highlight bash %}
$ sudo make unload
$ sudo modprobe alx
{% endhighlight %}

Now everything sould work. If you have still problems, write a comment or send me a mail.

[compat-drivers]: https://www.kernel.org/pub/linux/kernel/projects/backports/2013/03/04/compat-drivers-2013-03-04-u.tar.bz2
[e2200-support-mod]: /download/e2200-support.patch
