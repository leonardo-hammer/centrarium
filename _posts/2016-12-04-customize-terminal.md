---
layout: post
title: 我是如何配置Terminal的
categories: mac
comments: true
---

作为一个Terminal的重度用户，拿到新电脑第一步，就是配置它，让它变得顺手好用。

## ZSH

首先切换系统SHELL为zsh

{% highlight bash %}
chsh -s /bin/zsh
{% endhighlight %}

接着我会使用oh-my-zsh来提供zsh的主题配色, 安装方式有两种, 使用 curl 或者 wget

#### via curl

{% highlight bash %}
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
{% endhighlight %}

#### via wget

{% highlight bash %}
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
{% endhighlight %}

装完之后重开一下 Terminal

## Homebrew

要安装Ruby环境，就会用到 [Homebrew](http://brew.sh)，所以先讲一下[Homebrew](http://brew.sh)。这玩意有点类似apt-get, yum, 我们用过Linux都知道，很好用的库依赖安装工具。

*Homebrew, The missing package manager for macOS*

Install Homebrew by one line.

{% highlight bash %}
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

查看一下Homebrew版本

{% highlight bash %}
brew -v

Homebrew 1.6.9
Homebrew/homebrew-core (git revision 269c3; last commit 2018-07-08)
{% endhighlight %}

更新Homebrew

{% highlight bash %}
brew update
{% endhighlight %}

更新brew已经安装的库

{% highlight bash %}
brew upgrade
{% endhighlight %}

## Ruby

很多相关的东西都来自于Ruby，也就是Gem，说Gem之前，先记录一下Ruby相关的配置。

macOS自带的Ruby环境，经常会涉及权限问题，sudo，输入管理员密码，运行起来并不安全。所以我使用[chruby](https://github.com/postmodern/chruby)来修改当前的Ruby环境。

#### 使用Homebrew安装chruby

{% highlight bash %}
brew install chruby
{% endhighlight %}

#### 安装ruby-install 来获取最新的ruby环境

{% highlight bash %}
brew install ruby-install
ruby-install ruby
{% endhighlight %}

#### 创建.ruby-version来指定使用的ruby的版本

{% highlight bash %}
echo "ruby-2.4.0" > ~/.ruby-version
{% endhighlight %}

添加配置文件和自动切换ruby版本的脚本到 `~/.bash_profile` 或者 `~/.zshrc` 文件中。

{% highlight bash %}
source /usr/local/share/chruby/chruby.sh
source /usr/local/share/chruby/auto.sh
{% endhighlight %}

使用source 命令使其立即生效

{% highlight bash %}
source ~/.zshrc
{% endhighlight %}

最后，确认一下ruby的版本

{% highlight bash %}
ruby -v

ruby 2.4.0p0 (2016-12-24 revision 57164) [x86_64-darwin17]
{% endhighlight %}

## Gem

gem全称RubyGems，用来安装Ruby相关的一系列包，工具。

*RubyGems is a sophisticated package manager for the Ruby programming language*

Bundler是用来管理一堆gem的。

*Bundler: The best way to manage a Ruby application's gems*

首先Bundler自己就是一个gem库，先通过gem来安装。

{% highlight bash %}
gem install bundler
{% endhighlight %}

有了Bundler之后，就可以通过一个配置文件Gemfile来指明想要安装的一系列gem库了。

Gemfile看起来像这个样子：

{% highlight bash %}
source 'https://rubygems.org'
gem 'nokogiri'
gem 'rack', '~>1.1'
gem 'rspec', :require => 'spec'
{% endhighlight %}

安装的命令是

{% highlight bash %}
bundle install
{% endhighlight %}

## 参考

* [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
* [Homebrew](http://brew.sh)
* [chruby](https://github.com/postmodern/chruby)
* [RubyGems](https://rubygems.org)
* [Bundler](http://bundler.io)
