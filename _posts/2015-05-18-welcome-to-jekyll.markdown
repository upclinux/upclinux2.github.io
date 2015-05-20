---
layout: post
title:  "欢迎使用 Jekyll!"
author: vjudge1
date:   2015-05-18 14:23:38
categories: Jekyll
---
此博客是个纯静态博客。空间由 [GitHub Pages](https://pages.github.com) 提供，而所有代码均保存在 [GitHub 项目](https://github.com/upclinux/upclinux.github.io)中。

博客代码是完全公开的，可以按照自己意愿来进行修改。如果有什么好的内容想分享给大家，可以 fork & pull，甚至向会长申请成为 Owner。

博客本身使用 [Jekyll](https://github.com/jekyll/jekyll) 来架设。为什么不用 WordPress 呢？因为 GitHub Pages 不支持那些动态语言。





*以下不仅仅是一篇翻译。*

*Jekyll 不支持 Windows。由于文件名和文件内容的汉字编码的问题（Windows 默认使用 GB2312 编码，而 Linux/Mac 均使用 UTF-8 编码），请尽量避免在 Windows 下编辑和管理博客。*

    -_-||

你可以在项目的`_posts`目录里发现这篇文章。直接编辑然后重新“编译”来查看更改。你可以用很多种方法来“编译”，但是人们用得最多的还是输入 `jekyll serve`，因为这样正好可以启动一个小型 Web 服务器，然后你就可以直接从浏览器来查看了。

如果想添加文章，只需要在 `_posts` 目录里添加一个形如 `YYYY-MM-DD-标题.markdown` 的文件，然后在文件前面加一些必要的头部代码。如果不知道该加什么，可以仿照这篇文章来写。

Markdown 是一种轻型的排版语言。GitHub 有很多地方支持这种语言，并且，支持 Markdown 的编辑器也有很多。相比于其他语言，Markdown 并不难学。

Jekyll 支持代码高亮：

{% highlight c %}
#include <stdio.h>

int main(int argc, char *argv[])
{
    printf("Hello, world!\n");
    return 0;
}
{% endhighlight %}

阅读 [Jekyll 文档][jekyll] 来了解如何使用 Jekyll。如果发现 bug 或者想添加什么功能，请到 [Jekyll’s GitHub repo][jekyll-gh]. 如果有什么疑问，请到 [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
