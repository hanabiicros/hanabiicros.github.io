---
title: 基于Hexo创建博客(进阶篇)
author: 张耀斌
thumbnail: /images/wc01.jpg
toc: true
tags:
  - Hexo
  - Blog
categories: 技术
abbrlink: 413d9fe2
date: 2020-03-26 22:08:31
---
这是对基础篇的补充，方便了解Hexo，并对其进行个性化配置

<!--more-->
## **Hexo基本配置**
### _config.yml配置文件
　　该文件中是整个'Hexo'框架的配置文件，下面对其部分参数信息进行描述.
#### Site
```yml
title: icrosの小窝
subtitle: ''
description: '帅斌的博客'
keywords:
author: Yaobin Zhang
language: zh-CN
timezone: ''
```

    -'title'：网站的标题
    -'subtitle'：网站的副标题
    -'description'：网站的描述，用于SEO
    -'keywords'：网站的关键词
    -'author'：网站的作者
    -'language'：网站所使用的语言，默认是en，你可以改为'zh-CN'简体中文
    -'timezone'：网站的时区，默认使用你电脑的时区

<br/>

#### URL
```yml
url: https://hanabiicros.github.io/
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true 
  trailing_html: true
```

    -'url'：网址，如果你的网站是放在子路径下，则将其设置为'http://yoursite.com/child'
    -'root'：网站的根目录
    -'permalink'：文章的永久链接格式，即发布的文章在网站下生成的路径格式，默认是'yoursite.com/year/month/day/title'
    -'pretty_urls'：改写permalink的值来美化URL
    -'pretty_urls.trailing_index'：是否在永久链接中保留尾部的index.html
    -'pretty_urls.trailing_html'：是否在永久链接中保留尾部的.html

<br/>

#### Directory
```yml
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:
```
　　主要配置了各个文件夹的存放位置，另外'skip_render'表示跳过指定文件的渲染，即你可以使用自己的样式，一般默认即可。
<br/>

#### home page setting
```yml
index_generator:
  path: ''
  per_page: 10
  order_by: -date
```

    -'index_generator.path'：指明博客主页的根路径
    -'index_generator.per_page'：每页显示的最多文章数
    -'index_generator.order_by'：文章的排序方式

<br/>

#### Extensions
```yml
theme: icarus
```
    -'theme'：当前主题名称
    -'theme_config'：主题的配置文件，会覆盖主题目录下的'_config.yml'中的配置
    -'deploy'：部署部分的设置

　　其他配置默认即可，详细信息可见[官方文档](https://hexo.io/zh-cn/docs)
<br/>

## **Hexo基本操作**
### 写作
　　'基础篇'中讲了用如下命令，新建一篇文章
```bash
hexo new post 'title'
```
　　除了新建文章外，还可以新建页面'page'或者草稿'draft'
```bash
hexo new page 'title'
hexo new draft 'title'
```
　　其中新建的页面'page'的'title'，和你在导航栏中自定义的路径'/title'相匹配的。即点击导航栏的'title'按钮，会定位到这个页面中。~~(ps:当初看官方文档的时候看到了这里，不过没仔细看，结果在'关于'页面卡了好久，不知道怎么加这个页面。han(lll￢ω￢))~~
<br/>

### Front-matter
　　Front-matter是每个post中最上方以'---'分割的区域，指定个别文件的变量
```yml
---
title: 基于Hexo创建博客(进阶篇)
date: 2020-03-26 22:08:31
thumbnail: /images/wc01.jpg
toc: true
tags:            #标签
  - Hexo
  - Blog
categories: 技术
---
```

    -'title'：标题
    -'date'：建立日期
    -'updated'：更新日期
    -'tags'：标签，网站会根据每篇文章的标签进行归类
    -'categories'：分类，同样网站会根据每篇文章所属分类进行归类
    -'comments'：开启文章的评论功能，不加默认开启
    -'permalink'：覆盖文章的网址

　　另外'thumbnail'和'toc'是'icarus'主题特有的。
<br/>

### 资源插入

#### 插入图片
　　在Hexo 2之前，许多人是推荐安装'hexo-asset-image'，该插件貌似是解决了因常规的md语法和相对路径引用资源的方式造成在存档页和主页显示不正确的问题~~(ps:因为我是一上手就直接体验icarus主题，因而没遇到这个问题，况且我还是最新版的Hexo呢)~~
```yml
npm install https://github.com/CodeFalling/hexo-asset-image --save
```
　　然后在博客根目录下的'_config.yml'文件中将'post_asset_folder'属性设置为true，这样在每次创建新的文章时，他会自动在同目录下创建一个同名的文件夹，该文件夹用于存放与文章相关的资源。
　　接着用传统的md形式就能实现图片的插入
```yml
![title](xxx.jpg/png)
```
　　但是我在官方提供的插件库中，并未找到'hexo-asset-image'，只有'hexo-asset'，不过提供的功能大致是一样的。
<br/>

#### 标签插件
　　除了上述方法，在Hexo 3中，官方提供了名为'标签插件'的东东
```yml
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}
```
　　像这样也可以实现路径、图片、链接的插入，当然这个功能是很强大的，官方提供了'引用块'、'代码块'、'Image'、'Youtube'等的插入方式。
对于码农而言，主要用到的还是代码块的插入。一般使用，只需指明使用的语言'lang:'即可，而且它默认显示行号。
```yml
{% codeblock [title] [lang:language] [url] [link text] [additional options] %}
code snippet
{% endcodeblock %}
```
　　还有一种就是用反引号的形式(格式问题，不能换行只能放在一行，实际使用将代码和第二个反引号换行)
{% codeblock lang:c %}
``` [language] [title] [url] [link text] code snippet```
{% endcodeblock %}


