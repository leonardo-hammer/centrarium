---
layout: post
title: NSParagraphStyle的坑
categories: ios
comments: true
---

我们为了使文本显示更加美观，一般会设置文本的行间距，大多数情况下，我们会使用 `lineHeightMultiple` 属性来对本身的行高进行倍数运算。

{% highlight swift %}
let paragraphStyle = NSMutableParagraphStyle()
paragraphStyle.lineHeightMultiple = 1.2
let attributedString = NSAttributedString(string: "hello world",
                    attributes:[NSParagraphStyleAttributeName: paragraphStyle])
{% endhighlight %}

我们创建了相同属性的 `NSAttributedString` ，来预先计算 `UITableViewCell` 的高度。但是文本显示出来，却发现最底下的若干行没有显示出来，仔细检查了数遍计算高度的代码，仍然没有发现问题，后来截屏仔细观察，发现是 `lineHeight` 变高了，所以文本剩下的几行无法显示出来。

某些情况下，`lineHeightMultiple` 会超出设置的值，例如存在多语言字符，Apple emoji😁等。我们在计算文本高度的时候（通过 `boundingRectWithSize:options:context:` 方法），是一定会按照固定的 `lineHeightMultiple` 来计算的，例如设置了1.2，就是按照1.2算出高度值，雷打不动。但是，`UITextView` 真正渲染文本的时候，可就不能保证这个值了，这可太不靠谱了。我们的设置的高度是有可能不够显示这些文本的。

解决方案：幸运的是，API还提供了另外两个约束的属性。这个还是靠谱的，如果希望 `lineHeightMultiple` 永远固定，只要设置相同的最大，最小值，就正常了。

{% highlight swift %}
// Class NSMutableParagraphStyle
// See Also
maximumLineHeight
minimumLineHeight
{% endhighlight %}

一开始不熟悉这些API，很容易踩这个坑，而且会花费很多功夫在检查文本高度计算的那部分代码上，实在是吃力不讨好。

参考：[http://stackoverflow.com/questions/36248156/calculate-uilabel-height-for-text-with-different-languages-and-emojis-in-objecti](http://stackoverflow.com/questions/36248156/calculate-uilabel-height-for-text-with-different-languages-and-emojis-in-objecti)
