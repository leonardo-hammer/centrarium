---
layout: post
title: 发布自己的项目到CocoaPods
categories: ios
comments: true
cover:  "assets/chess.png"
---

## 关于创建项目

如果你在开发之前，就计划着要发布项目到CocoaPods，那最好不过。CocoaPods提供了一套代码模板供我们使用。

创建一个Pods项目，CLCocoaExtension为项目名

{% highlight bash %}
pod lib create CLCocoaExtension
{% endhighlight %}

开始一堆问答

* What language do you want to use?? [ ObjC / Swift ] 开发语言是什么啊？
  * ObjC

* Would you like to include a demo application with your library? [ Yes / No ] 想不想包含示例程序啊？
  * Yes

* Which testing frameworks will you use? [ Specta / Kiwi / None ] 准备用哪些测试框架啊？这里提供了Specta和Kiwi两个可选。
  * None

* Would you like to do view based testing? [ Yes / No ] 想不想测试界面啊？
  * Yes

* What is your class prefix? 项目前缀是什么啊？
  * CL

根据你的回答，就会生成一个项目模板。

## 项目模板包含什么？

* _Pods.xcodeproj （当前Pod项目的xcodeproj文件）
* CLCocoaExtension.podspec （发布CocoaPods所需的配置文件）
* Example （示例项目，一般打开这个目录里面的.xcworkspace进行开发）
* LICENSE （协议）
* Pod （项目开发主目录）
  * Assets （项目资源文件目录）
  * Classes （项目源代码）
* README.md （显示在Github主页的说明文档）

接下来主要讲讲.podspec配置文件

{% highlight properties %}
Pod::Spec.new do |s|
# 项目名
s.name = "CLCocoaExtension"

# 版本号，对应后面设置的Tag
s.version = "0.1.0"

# 简介
s.summary = "CLCocoaExtension Contain some common property and method extension."

# 详细描述
s.description = "CLCocoaExtension Contain some common property and method extension, just it!"

# git仓库主页
s.homepage = "https://github.com/leonardo-hammer/CLCocoaExtension"
s.license = 'MIT'
s.author = { "leonardo-hammer" => "tuesleep@gmail.com" }
s.source = { :git => "https://github.com/leonardo-hammer/CLCocoaExtension.git", :tag => s.version.to_s }

# 支持的平台
s.platform = :ios, '7.0'

# 项目是否ARC
s.requires_arc = true

# 源代码路径，保持默认即可
s.source_files = 'Pod/Classes/**/*'
s.resource_bundles = {
'CLCocoaExtension' => ['Pod/Assets/*.png']
}

# 其他暂时没用到的配置
# s.social_media_url = 'https://twitter.com/'
# s.screenshots = "www.example.com/screenshots_1", "www.example.com/screenshots_2"
# s.public_header_files = 'Pod/Classes/**/*.h'
# s.frameworks = 'UIKit', 'MapKit'
# s.dependency 'AFNetworking', '~> 2.3'
end
{% endhighlight %}

配置完成之后，通过命令验证一下是否正确。添加 `--no-clean` 的作用是，打印出详细信息，便于确认错误的位置

{% highlight bash %}
pod lib lint --no-clean
{% endhighlight %}

通过验证会看见这些

{% highlight bash %}
$ pod lib lint --no-clean

 -> CLCocoaExtension (0.1.0)

Pods project available at /var/folders/p3/q4yksg4x7jd4zw_1dqclkmg40000gn/T/CocoaPods/Lint/Pods/Pods.xcodeproj for inspection.

CLCocoaExtension passed validation.
{% endhighlight %}

这时候可以把项目push到Github上，并且设置 **git tag** 。tag要和之前配置文件里面的s.version一致，不然后面会报错。

在项目目录中执行 `git tag` 命令

{% highlight bash %}
git tag "0.1.0"
{% endhighlight %}

当然，也要push上去

{% highlight bash %}
git push --tags
{% endhighlight %}

## 注册CocoaPods的trunk帐号

必须有 **trunk帐号** 才能push代码到CocoaPods，注册时，邮箱跟上面配置文件里保持一致比较合适。

{% highlight bash %}
pod trunk register chanricle@gmail.com 'chanricle' --description='chanricle, a iOS developer'
{% endhighlight %}

根据提示，查收邮件，验证一下，trunk帐号就注册好了。用命令来验证一下

{% highlight bash %}
pod trunk me
{% endhighlight %}

{% highlight bash %}
$ pod trunk me
  - Name:     chanricle
  - Email:    chanricle@gmail.com
  - Since:    January 5th, 03:36
  - Pods:     None
  - Sessions:
    - January 5th, 03:36 - May 12th, 20:00. IP: 218.85.99.72 Description: chanricle, iOS
    developer.
{% endhighlight %}

## 发布项目到CocoaPods

准备工作终于做完了，可以发布了。

{% highlight bash %}
pod trunk push
{% endhighlight %}

{% highlight bash %}
$ pod trunk push

[!] Found podspec `CLCocoaExtension.podspec`
Updating spec repo `master`

CocoaPods 1.0.0.beta.2 is available.
To update use: `gem install cocoapods --pre`
[!] This is a test version we'd love you to try.

For more information see http://blog.cocoapods.org
and the CHANGELOG for this version http://git.io/BaH8pQ.

Validating podspec
 -> CLCocoaExtension
{% endhighlight %}

如果在Validating podspec这里卡很久，请耐心等待。

{% highlight bash %}
[!] There was an error pushing a new version to trunk: getaddrinfo: nodename nor servname provided, or not known
{% endhighlight %}

卡了N久出来了这个错误，要冷静。通常是网络问题，多试几次，或者Terminal翻..

成功了之后出来一大段信息

{% highlight bash %}
Updating spec repo `master`

CocoaPods 1.0.0.beta.2 is available.
To update use: `gem install cocoapods --pre`
[!] This is a test version we'd love you to try.

For more information see http://blog.cocoapods.org
and the CHANGELOG for this version http://git.io/BaH8pQ.

  - Data URL: https://raw.githubusercontent.com/CocoaPods/Specs/bb36a2ff668d37d1dcbe63eb649f8acb34f35600/Specs/CLCocoaExtension/0.1.0/CLCocoaExtension.podspec.json
  - Log messages:
    - January 5th, 20:28: Push for `CLCocoaExtension 0.1.0' initiated.
    - January 5th, 20:28: Push for `CLCocoaExtension 0.1.0' has been pushed (3.033736075 s).
{% endhighlight %}

想知道到底有没有成功发布，可以使用命令查询自己的项目

{% highlight bash %}
pod search CLCocoaExtension
{% endhighlight %}

{% highlight bash %}
$ pod search CLCocoaExtension

-> CLCocoaExtension (0.1.0)
   CLCocoaExtension Contain some common property and method extension.
   pod 'CLCocoaExtension', '~> 0.1.0'
   - Homepage: https://github.com/leonardo-hammer/CLCocoaExtension
   - Source:   https://github.com/leonardo-hammer/CLCocoaExtension.git
   - Versions: 0.1.0 [master repo]
{% endhighlight %}
