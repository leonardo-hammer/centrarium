---
layout: post
title: Xcode Components 下载艰难问题的终结方法
categories: ios
comments: true
---

找到了解决Xcode Components下载艰难的方法。

## 首先得到Simulator或者Documentation的下载地址

Terminal中启动Xcode

{% highlight bash %}
sudo /Applications/Xcode.app/Contents/MacOS/Xcode
{% endhighlight %}

Preference - Components, 点击下载按钮，然后点击取消。这时候Terminal会输出一段Log

{% highlight bash %}
DVTDownloadable: Download Cancelled. Downloadable: https://devimages.apple.com.edgekey.net/downloads/xcode/simulators/com.apple.pkg.iPhoneSimulatorSDK9_2-9.2.1.1451951473.dmg.
{% endhighlight %}

## 直接通过地址下载文件

其中 [https://devimages.apple.com.edgekey.net/downloads/xcode/simulators/com.apple.pkg.iPhoneSimulatorSDK9_2-9.2.1.1451951473.dmg](https://devimages.apple.com.edgekey.net/downloads/xcode/simulators/com.apple.pkg.iPhoneSimulatorSDK9_2-9.2.1.\
1451951473.dmg) 就是这个文件的下载地址，我们用自己的工具下载它，速度一般很快。

## 将文件放到缓存目录中

打开 Finder `command + shift + G` 进入到 `~/Library/Caches` 目录，找到 com.apple.dt.Xcode 文件，右键点击选择 Show Packages Contents，找Downloads目录，如果没有，自己创建一个，创建好之后，把下载好的dmg文件复制进来。

## 在Xcode中点击下载

重新打开Xcode，进到Component里，点击之前的下载按钮，这时候进度条走的速度会比之前快特别多，稍等一会儿，就显示完成，打钩了。

![](/assets/img/xcode-component.png)
