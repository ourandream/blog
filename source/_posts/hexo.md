---
title: hexo
tags: blog
categories: others
mathjax: true
top: true
abbrlink: ab21860c
---
为了搭建网站，折腾了不久hexo和this，也算是玩出了一些心得。准备做些记录，免得忘记。
<!--more-->
## base
每次改完主题或者想更新博客，必须在博客的根目录下打开git bash,执行以下命令：
```git
hexo clean
hexo g
hexo d
```
然后便可部署到网站上了,注意若是利用gitee部署,则还需要前往仓库界面的gitee page界面手动更新,github等其他的网站则不需要

如果想测试而不线上部署,可试试如下命令:
```git
hexo clean
hexo g
hexo s
```
这些命令执行后,将会在本地部署网站,然后根据给定的网站进入即可

文章书写可在终端输入如下代码创建模板：
```
hexo new "title"
```
或者直接在\sources\_post里创建md文件
文章的前面的各项设置:
```markdown
---

title: hexo

tags: blog

categories: others

mathjax: true

top: true

---
```

## 侧边框标题自动添加数字去除
在next主题文件_config.yml文件下搜索toc,找到如下界面:
```yml
toc:

 enable: true

 # Automatically add list number to toc.

 number: true

 # If true, all words will placed on next lines if header width longer then sidebar width.

 wrap: false

 # If true, all level of TOC in a post will be displayed, rather than the activated part of it.

 expand_all: false

 # Maximum heading depth of generated toc.

 max_depth: 6
```
把其中的number改为false即可

## 文章底部标签美化
```swig
<div class="post-tags">
	{%- for tag in post.tags.toArray() %}
	  <a href="{{ url_for(tag.path) }}" rel="tag"><i class="fa fa-tag"></i> {{ tag.name }}</a>
	{%- endfor %}
```
找到相应的代码然后用以上代码覆盖即可

## 上传并在文章中使用图片
先在npm中安装**hexo-asset-image**

然后打开/node_modules/hexo-asset-image/index.js
然后将其中内容替换如下:
```js
'use strict';
var cheerio = require('cheerio');

// http://stackoverflow.com/questions/14480345/how-to-get-the-nth-occurrence-in-a-string
function getPosition(str, m, i) {
  return str.split(m, i).join(m).length;
}

var version = String(hexo.version).split('.');
hexo.extend.filter.register('after_post_render', function(data){
  var config = hexo.config;
  if(config.post_asset_folder){
        var link = data.permalink;
    if(version.length > 0 && Number(version[0]) == 3)
       var beginPos = getPosition(link, '/', 1) + 1;
    else
       var beginPos = getPosition(link, '/', 3) + 1;
    // In hexo 3.1.1, the permalink of "about" page is like ".../about/index.html".
    var endPos = link.lastIndexOf('/') + 1;
    link = link.substring(beginPos, endPos);

    var toprocess = ['excerpt', 'more', 'content'];
    for(var i = 0; i < toprocess.length; i++){
      var key = toprocess[i];
 
      var $ = cheerio.load(data[key], {
        ignoreWhitespace: false,
        xmlMode: false,
        lowerCaseTags: false,
        decodeEntities: false
      });

      $('img').each(function(){
        if ($(this).attr('src')){
            // For windows style path, we replace '\' to '/'.
            var src = $(this).attr('src').replace('\\', '/');
            if(!/http[s]*.*|\/\/.*/.test(src) &&
               !/^\s*\//.test(src)) {
              // For "about" page, the first part of "src" can't be removed.
              // In addition, to support multi-level local directory.
              var linkArray = link.split('/').filter(function(elem){
                return elem != '';
              });
              var srcArray = src.split('/').filter(function(elem){
                return elem != '' && elem != '.';
              });
              if(srcArray.length > 1)
                srcArray.shift();
              src = srcArray.join('/');
              $(this).attr('src', config.root + link + src);
              console.info&&console.info("update link as:-->"+config.root + link + src);
            }
        }else{
            console.info&&console.info("no src attr, skipped...");
            console.info&&console.info($(this));
        }
      });
      data[key] = $.html();
    }
  }
});
```

然后需要在_config.yml中找到如下代码并改为true
```js
post_asset_folder: true
```

然后要插入的图片需要放在_post文件夹里的一个和md文件同名的文件夹,而且使用markdown语法调用时忽略文件夹名
```
![](1/1.jpg)
![](1.jpg)
```
即使用下面的哪一行