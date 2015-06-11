# upclinux.github.io
协会博客 [upclinux.github.io](https://upclinux.github.io)

采用 GitHub Pages 和 Jekyll 架设。Jekyll 是 GitHub 官方认可和采用的静态博客软件。

**因为使用了 Bootstrap，博客不兼容旧版 IE 浏览器。**

## 如何编辑博客

### 写博客

博客文本都放在了`_posts`目录中。博客文件的文件名要遵循`YYYY-MM-DD-标题.markdown`的格式，否则系统不识别。

文件名可以使用中文，但是不要这样做。

文件必须使用无 BOM 的 UTF-8 编码。**尽量不要在 Windows 系统中编辑**。如果只有 Windows，可以用 Vim、Notepad++ 一类支持 UTF-8 的文本编辑器。

博客文件要有一个 YAML 格式的头部。如果不知道是啥，可以仿照其他博客改写：

	---
	layout: post
	title: "Linux Show & 第一次纳新"
	author: vjudge1
	date:  2013-12-07 19:23:33
	categories: 活动	
	tags: 图书馆 展示
	---
	
博客发布时间和文件名没有关系，要以上面那段代码为准。

如果需要目录，在头部后面紧跟以下两行：

	* contents
	{:toc}
	
在宽屏情况下，目录会显示在页面右侧并且自动跟踪。窄屏时目录只在标题下面显示。

正文要使用 Markdown 语言来编写。如果不知道 Markdown 是啥，[这里](https://guides.github.com/features/mastering-markdown/)有一份简短的说明。如果对 e 文不感冒，可以仿照其他博客来改写，毕竟这门语言是门轻型描述性语言，并不是很难学。

需要注意的是，博客目前使用的 Markdown 引擎不支持 ``` (3 个撇)。如果想插入代码，可以加 Tab，或者使用 Jekyll 的代码高亮：

	{% highlight c %}
	int main() { return 0; }
	{% endhighlight %}

### Markdown 编辑器

Markdown 编辑器有很多。推荐 [Atom](https://atom.io) 和 [Visual Studio Code](https://code.visualstudio.com)（不是 Visual Studio），这两个编辑器免费，支持以目录为单位进行编辑，而且支持 Markdown 的实时预览。

### 改主题

Jekyll 采用 Liquid 模板语言，这门语言虽然不像那些动态语言一样强大（毕竟是静态博客），但是仍然可以非常自由灵活地定制博客。

* [Jekyll 的文档](http://jekyllrb.com/docs/home/)
* [Liquid 的基本语法](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)

在这里，你必须要会 e 文了，而且计算机鹰语就那么点单词，很好学的。

### git commit

在确定博客没有问题之后（下一节讲述如何在本地预览），可以简单地通过 Git 的 Commit 和 Push 把改动推送到服务器上。

同步成功后，几秒之内就可以看到网站更新。

## 如何在本地预览博客

**强烈建议不要在 Windows 系统中操作。Jekyll 官方不支持 Windows。**


OS X 的配置是最轻松的。但是对于 Ubuntu 等 Linux 系统，我们要稍稍做点事儿：
	
	sudo apt-get install ruby-dev nodejs

接下来的步骤同时适用于 Linux 和 OS X：

1. 先逛一下[淘宝 (ruby.taobao.org)](http://ruby.taobao.org)。大家都知道方校长做了什么，所以淘宝为我们提供了 RubyGems 镜像。按照页面上的说明换源。
2. 照敲：

		sudo gem install jekyll jemoji
3. 完事儿了。

### 预览

预览就很轻松了。在“终端”中 cd 到博客的根目录中，然后

	jekyll serve
	
Jekyll 会自动产生静态网页。在浏览器中输入`http://127.0.0.1:4000`即可访问。

Jekyll 会自动监视文件变化，随时更新。按 Ctrl-C (苹果电脑是 Control-C) 退出。