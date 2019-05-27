---
layout: post
title: 制作MacOS安装U盘
categories: mac
comments: true
---

接上U盘，打开Disk Utility，把U盘格式化成OS X Extended(Journaled), GUID Partition Map, 并记住格式化时的Name，例如我设成 Installer

下载原版的安装文件，例如：Install OS X Yosemite.app, 如果你也跟我一样从App Store下载的，这个安装文件应该在/Applicatoins里面

打开Terminal，将安装文件写入U盘

`sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Installer --applicationpath /Applications/Install\ macOS\ Sierra.app --nointeraction`

{% highlight bash %}
Erasing Disk: 0%... 10%... 20%... 30%...100%...
Copying installer files to disk...
Copy complete.
Making disk bootable...
Copying boot files...
Copy complete.
Done.
{% endhighlight %}

All done.
