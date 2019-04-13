---
layout: post
title: iCloud Documents 相关概念
categories: ios
comments: true
---

整理了一些iCloud Documents相关的概念。

### iCloud Container

iCloud Container是设备本地的一个private目录。
相关API：`NSFileManager URLForUbiquityContainerIdentifier:`

{% highlight bash %}
# URL示例
file:///private/var/mobile/Library/Mobile%20Documents/iCloud~com~sumiapp~iclouddrivedemo
{% endhighlight %}

### 上传文件到iCloud drive

一般我们会在iCloud Container的应用根目录iCloud~bundle~id下再创建Documents，这个目录里面的文件会上传到iCloud drive中。因为有这个特性，所以我们上传文件到iCloud drive，其实就是复制文件到iCloud Container里。
相关API： `UIDocument saveToURL:forSaveOperation:completionHandler:`

### 从iCloud drive下载文件

假如iCloud drive里有一些文件，iCloud Container里目前没有，要如何下载呢？

* 一种情况，应用试图打开或访问这个文件，它就会被下载。
* 另一种方法，程序内部直接调用`NSFileManager startDownloadingUbiquitousItemAtURL:error:`，启动一个下载进程，把iCloud drive里面的文件下载到iCloud container，启动成功的话，这个方法会返回 true，只是启动而已，如果文件过大，需要等待一会儿，可以使用`NSURLUbiquitousItemDownloadingStatusKey`查询检测下载进度。

OK，终于下载到iCloud container里面了，推荐使用UIDocument访问。

但是，这时候我还想复制到Sandbox Documents里面，该怎么做呢？

首先Override `UIDocument loadFromContents:ofType:error:` 对其中的content进行保存操作。

{% highlight swift %}
override func loadFromContents(contents: AnyObject, ofType typeName: String?) throws {
    guard let data = contents as? NSData else {
        fatalError("Cannot handle contents of type \(contents.dynamicType).")
    }

    self.contentData = data
}
{% endhighlight %}

我用了一个`contentData`来接收content的值。

接着调用`UIDocument openWithCompletionHandler:`，调用了这个方法后，会触发 `loadFromContents:ofType:error:`。然后在回调中进行写入操作。

{% highlight swift %}
document.openWithCompletionHandler { success in
    if success {
        if let documentPath = NSSearchPathForDirectoriesInDomains(.DocumentDirectory, .UserDomainMask, true).last {
            document.contentData.writeToFile(documentPath, atomically: true)
        }
    }
}
{% endhighlight %}

这样，文件就被复制到Sandbox Documents中了。

如果遇到iCloud drive的目录中没有出现文件的问题，可以尝试修改app的版本号，Version，Build。我将之前的Build数字+1之后，重新安装测试，iCloud drive中就立即出现文件了。

---

这篇文章主要是针对iCloud Document一些模糊的概念的梳理，其内容并不完整，详尽信息还是要查看官方文档。

参考：

* [iCloud Fundamentals (Key-Value and Document Storage)](https://developer.apple.com/library/content/documentation/General/Conceptual/iCloudDesignGuide/Chapters/iCloudFundametals.html)
* [ShapeEdit](https://developer.apple.com/library/content/samplecode/ShapeEdit/Introduction/Intro.html)
