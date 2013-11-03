---
layout: post
title: "Arch Linux: Repair an encrypted system"
category: linux
---
## Introduction

The following article is for novice to medium users who broke their encrypted system, be it by accident, stupidity or choice. It'll show you how to regain access to your hard drive and conduct a repair attempt, but *not* tell you how to repair a broken encryption (e.g. corrupted headers), because without having countermeasures in place (e.g. backups of said headers) your data is probably lost for good.

<!--more-->

The situation: You have set up a full disk encryption using LVM on LUKS (probably something like in [my guide][lvmOnLuks2013]) and managed to somehow break the boot process of your system. Perhaps that last update didn't went smoothly, you finally decided to rearrange the folder structure on /root to something more intuitive and efficient or [Linux simply hates you][hateGoogle].

Hint: **Never** use `pacman --force` unless you know exactly what you are doing!

Either way your system doesn't boot anymore and you don't know what to do. Back in the old days when your system was an unencrypted source of joy and happiness, you would've whipped out your trusty Arch Linux Installation CD, chrooted into your installation and tried stuff you found on the forums until everything was fine again. In the worst case you could always format your `/root` (you *do* have a separate root partition, don't you?) and start over again with a fresh installation.

But those days are gone for good, because you decided to add another layer of security and to encrypt your whole system. Maybe you followed a guide to encryption step by step, tweaking the arguments to all the commands to your needs, without caring what every single command does (Don't feel bad about it! I've been there, I've done that too) or you just can't remember what you did exactly. Either way you're facing the problem of not being able to chroot into your installation. Keep it up! The solution is pretty easy.

## Step 0: Chrooting into the encrypted system 

I'm assuming you have an unencrypted `/boot`-partition on `/dev/sda1` and an encrypted partition containing your LVM on `/dev/sda2`. After booting with the installation CD and setting up your favourite keyboard-layout and font. You can just type

{% highlight console %}
# cryptsetup luksOpen /dev/sda2 crypt
{% endhighlight %}

and if you can provide the correct password, the partition will be made accessible as `/dev/mapper/crypt`. Even better, the LVM volumes will also be provided as `/dev/mapper/<volume-name>`. To check if everything went right you can use the `lsblk` command, the output should look something like this (important part highlighted red):

[![output of lsblk][lsblk]][lsblk]

As you can see, the existing LVM volumes were automatically recognized. Very convenient. Now you can mount the partitions and chroot into your environment:

{% highlight console %}
# mount /dev/mapper/lvmpool-root /mnt
# mount /dev/sda1 /mnt/boot
# mount /dev/mapper/lvmpool-home /mnt/home
# arch-chroot /mnt
{% endhighlight %}

That's it! Wasn't too difficult, was it? 

## Step 1: Repairing the system

The previous paragraph was just the groundwork, now comes the tricky part: Get the system up and running again. Depending on what happened you might be able to repair the system. A good place to start looking for fixes is usually the [ArchWiki][archWiki], especially the [pacman page][pacmanTroubleshooting]. Why of all things pacman you ask? Let's be honest, most of the breakage is a direct or indirect result of `pacman -Syu`, regardless whether your hardware doesn't like the new packages or you missed an important hint in the [news][archNews].

If you cannot find a solution consulting the wiki, you should look into the [forums][archForums], other people suffer from the same problems astonishingly often. Despite everything, there comes a day when only one solution is left: Reinstalling the system. (I told you running `rm -rf /` was a bad idea and you should feel bad.)

## Step 2: Reinstalling the system

To be honest: If I cannot find a quick fix for a problem I tend to reinstall my system, because it's sometimes quicker and easier than looking for a solution. To ensure a smooth transition there are several things to be taken care of.

1. **Make a backup of your data!** Even if you don't intend to touch `/home`, things might get horribly wrong.
2. Make a copy of modified configuration files: The command `pacman -Qii | awk '/^MODIFIED/ {print $2}'` will provide you with a list of modified configuration files. Store them somewhere save.
3. Make a list of installed packages from the repositories: This will make reinstalling all your stuff a lot easier later on. To generate the list use `pacman -Qq |grep -Fv -f <(pacman -Qqm) > pkglist`.
4. Make a list of packages you installed manually, `pacman -Qqm > pkglist_man` will do the job.

Now you are ready to reinstall everything. First of all you have to leave the chroot and unmount the partitions:

{% highlight console %}
# exit
# umount /mnt/{boot,home,}
{% endhighlight %}

Subsequently you can follow [my guide][lvmOnLuks2013] starting with Step 3. Don't format your home-partition if you want to keep it! A reboot later your system should be running again. You can now reinstall your packages using the previously generated list:

{% highlight console %}
# pacman -S $(< pkglist)
{% endhighlight %}

Unfortunately you have to install your manually installed packages, well, manually. Copy your old configuration files back to where they belong and your system should be as good as it was before. (Minus the breakage of course) 

[archForums]: https://bbs.archlinux.org/viewforum.php?id=44
[archNews]: https://www.archlinux.org/news/
[archWiki]: https://wiki.archlinux.org/index.php
[hateGoogle]: https://www.google.de/search?q=linux+hates+me
[lsblk]: {{site.url}}/assets/img/repair-encrypted/lsblk.png
[lvmOnLuks2013]: {% post_url 2013-03-23-arch-linux-lvm-on-top-of-luks-2013-style %} 
[pacmanTroubleshooting]: https://wiki.archlinux.org/index.php/Pacman#Troubleshooting
