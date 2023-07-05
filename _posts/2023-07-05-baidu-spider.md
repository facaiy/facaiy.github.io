---
layout: post
title: "如何让百度索引Github Pages上的Jekyll博客"
description: ""
category: misc
tags: []
modify: 2023-07-05 21:09:24
---

update: 2023-07-05


这个博客是用Jekyll静态生成，搭建在github pages上服务的。最近发现百度一直未收录博客内容，Google收录正常，搜了下原来是github禁止了百度爬虫访问，所以百度无法收录搭在github上的博客，有点坑。网上有CDN、Coding Pages和Vercel三种方案（[解决百度爬虫无法爬取 Github Pages 个人博客的问题](https://zpjiang.me/2020/01/15/let-baidu-index-github-page/)），我试了下，Vercel方案最简单，把操作流程记录并分享下。


### 从github pages迁移到Vercel

这个方案实质是把托管从github pages迁到Vercel，从而规避百度抓取的问题。

整个导入流程挺直觉的，访问[Vercel](https://vercel.com)，用github账号授权登陆，跟着演示流程把博客项目导入即可。如果没弄明白，可以参考官方文档[Import an existing project](https://vercel.com/docs/getting-started-with-vercel/import)。有一说一，这家文档建设得很完备，赞！

Jekyll项目在Build时可能报错：

```bash
sh: jekyll: command not found
Error: Command "jekyll build" exited with 127
```

这是因为没有正确配置Gemfile文件，其实官方文档[How to Deploy a Jekyll Site with Vercel](https://vercel.com/guides/deploying-jekyll-with-vercel)中有提供模板，具体文件在[这里](https://github.com/vercel/vercel/tree/main/examples/jekyll)。如果懒得看，也可以自己在项目目录下建个名为Gemfile的空白文件，把下面内容贴进去：

```bash
source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
gem "jekyll", "~> 4.0"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]
```

本地测试下，输入`bundle install && bundle exec jekyll serve`，看是否正常。这条命令会安装相关依赖并启动本地服务，还会产生Gemfile.lock文件。我担心Gemfile.lock文件会影响线上编译，把它排除在git检索了`echo "Gemfile.lock" >> .gitignore`。一切正常，就commit和push到github，这下Vercel应该就能顺利布署了。

另外，有个小坑，是看下Jekyll代码里有没有全局变量的判断，比如[Environments - Jekyll](https://jekyllrb.com/docs/configuration/environments/)中例子：

```liquid
{% raw %}
{% if jekyll.environment == "production" %}
   {% include disqus.html %}
{% endif %}
{% endraw %}
```

我的博客里就有生产环境的判断，所以需要去项目设置里的环境变量里增加`JEKYLL_ENV=production`，详细操作见[Environment Variables - Vercel](https://vercel.com/docs/concepts/projects/environment-variables)。


### 自有域名

我的博客有自己的域名，需要把它从github映射到vercel，官方有个详细文档[Adding & Configuring a Custom Domain](https://vercel.com/docs/concepts/projects/domains/add-a-domain)。简单说就是在vercel这添加你的域名，它会生成两条记录A record和CNAME record，把这两个内容抄到你的DNS服务器配置里，然后等vercel检测完成即可。


### 百度收录

DNS配置好后要等一段时间才会同步到全球，本地可以用命令查下`dig 你的域名`，如果查到变成vercel的A record里的IP，就可以去百度[站长工具](https://ziyuan.baidu.com/crawltools/index)里「抓取诊断」，扔一篇文章的链接抓取试试，成功就说明全好了。

最后，如果需要用sitemap提交收录的话，可以参考这篇文章[GENERATING A SITEMAP.XML WITHOUT A PLUGIN](http://www.independent-software.com/generating-a-sitemap-xml-with-jekyll-without-a-plugin.html)，操作很简单，或者在项目下建个sitemap.xml文件，把配置抄进去。

```liquid
{% raw %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    {% unless post.published == false %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.sitemap.lastmod %}
        <lastmod>{{ post.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif post.date %}
        <lastmod>{{ post.date | date: "%Y-%m-%d" }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date: "%Y-%m-%d" }}</lastmod>
      {% endif %}
      <changefreq>monthly</changefreq>
      <priority>0.5</priority>
    </url>
    {% endunless %}
  {% endfor %}
</urlset>
{% endraw %}
```

收工。
