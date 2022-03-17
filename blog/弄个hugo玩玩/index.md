# 使用hugo


## 搭建hugo

[官方文档](https://gohugo.io/getting-started/)、[DoIt主题文档](https://hugodoit.pages.dev/zh-cn/theme-documentation-basics/)、[Hugo配置文件详解](https://dp2px.com/2019/09/04/hugo-config/)、[Hugo文档](https://www.andbible.com/post/hugo-command-usage/)


## Hugo使用纪录

优点：实时预览、生成速度快、配置简单。

缺点：对比HEXO，漂亮的主题资源少，相关的中文文档也少。

### 基础配置config.toml

```bash
#URL 路径区分大小写。影响最终生成的静态资源目录地址，如Docker.md生成的路径为Docker/index.md
#默认值为false，生成的文件夹为docker/index.md
disablePathToLower = true

#自动检测是否包含 中文\日文\韩文，影响预览
hasCJKLanguage = true    

#保留原文件名。已经被移除了，文件名如果包含除-以外的特殊字符会被清除，空格会转化成-
preserveTaxonomyNames = true  

#Permalinks 配置 (https://gohugo.io/content-management/urls/#permalinks)
[Permalinks]
  posts = ":year/:month/:filename"
```

### 文章内容配置

```bash
---
title: "{{ replace .ContentBaseName  "-" " " | title }}"
subtitle: ""
date: {{ .Date }}
lastmod: {{ .Date }}
draft: true
authors: [Forest]
description: ""

tags: []
categories: []
series: []
series_weight: 1

#首页屏蔽、搜索屏蔽
hiddenFromHomePage: false
hiddenFromSearch: false

#如果使用 typora 编辑文章，通过以下两项设置图片默认路径
typora-root-url: {{.File.Dir | replaceRE "[^\\\\]+\\\\" "../" }}../static/blog/{{.BaseFileName}}
typora-copy-images-to: {{.File.Dir | replaceRE "[^\\\\]+\\\\" "../" }}../static/blog/{{.BaseFileName}}/{{.BaseFileName}}.assets/
---


#Hugo不会发布下面内容:
publishdate 发布日期值设定在未来的内容
draft: true 草案状态设置为真的内容
expirydate 过期日期值设置为过去某事件的内容
```

### 命令

```bash
# 创建blog
hugo new site my_website
cd my_website

# 创建第一篇文章
hugo new posts/first_post.md

# 本地启动网站，去查看 http://localhost:1313
hugo serve -e production -D -F

#生成静态资源到目标位置
hugo -d C:\Users\wxz\Desktop\publish
```

```bash
[/]: hugo -help

Usage:
  hugo [flags]
  hugo [command]

Available Commands:
  check       包含一些验证检查
  config      打印站点配置信息
  convert     转化内容为其他不同模式
  env         打印Hugo版本和环境信息
  gen         几个有帮助的生成器的集合
  help        关于命令的帮助
  import      从它处导入你的站点
  list        不同类型内容的列表
  new         为站点生成新内容
  server      高效能web服务器
  version     打印Hugo版本号

Flags:
  -b, --baseURL string         对应于站点的域名(和路径),比如 https://spf13.com/
  -D, --buildDrafts            包含标记为草案的内容
  -E, --buildExpired           包含标记为过期的内容
  -F, --buildFuture            包含标记发布日期为未来的的内容
      --cacheDir string        缓存目录 默认是: $TMPDIR/hugo_cache/
      --cleanDestinationDir    从目标目录删除不在static目录中内容
      --config string          配置文件(默认是 path/config.yaml|json|toml)
      --configDir string       配置目录 (默认是 "config")
  -c, --contentDir string      内容目录路径
      --debug                  输出调试信息
  -d, --destination string     输出文件的文件系统路径
      --disableKinds strings   禁用其他不同格式的页面(home, RSS 等)
      --enableGitInfo          为页面添加Git修订版本、日期和作者等信息
  -e, --environment string     构建环境
      --forceSyncStatic        当static文件有变化时copy所有文件
      --gc                     构建后执行一些清理工作 (移除未用的缓存文件)
  -h, --help                   hugo的帮助
      --i18n-warnings          显示丢失的翻译
      --ignoreCache            忽略缓存
  -l, --layoutDir string       layout布局目录
      --log                    启用Log日志
      --logFile string         日志文件路径 (如果设置此选项,自动启动日志)
      --minify                 对支持的输出内容格式最小化(如HTML, XML 等)
      --noChmod                不同步文件的许可模式
      --noTimes                不同步文件的修改时间
      --path-warnings          目标路径重复是打印警告信息
      --quiet                  在安静模式构建
      --renderToMemory         显示在内存中(仅仅在性能测试中有用)
  -s, --source string          读取文件的相对文件系统路径
      --templateMetrics        显示模板执行的指标
      --templateMetricsHints   计算一些改善的提示，当和 --templateMetrics一起使用时
  -t, --theme strings          要使用的主题(位于 /themes/THEMENAME/)
      --themesDir string       主题路径的文件系统路径
      --trace file             写跟踪信息的文件(一般来说无用)
  -v, --verbose                显示冗长的输出
      --verboseLog             记录冗长的日志
  -w, --watch                  监控文件系统变化,并且在必要时重新创建网站文件

使用 "hugo [command] --help" 获得某个命令的更多信息.
```

### 目录及模板说明

```bash
#post目录下的文章模板，具体配置见“文章内容配置”
archetypes\post.md

#资源文件夹，里面的所有内容都会生成为静态资源文件
static\
```

### ShortCode

Hugo提供的简单代码段，可以生成HTML代码。[简介](https://gohugo.io/templates/shortcode-templates/)、[提供的方法](https://gohugo.io/functions/)

## DoIt主题使用纪录

```text
# 推荐服务启动命令
# --disableFastRender 参数来实时预览你正在编辑的文章页面.
# -e production 命令来开启 评论系统, CDN 和 fingerprint 特性.
hugo serve -e productio --disableFastRender
```

## Hugo搭配typora，静态资源如何保存？

1. Typora 设置图片复制到指定目录

   ```text
   my_website_DoIt\static\blog\${filename}\${filename}.assets\
   ```

2. 设置Hugo 模板 archetypes\default.md，加上 Typora 的资源根目录 typora-root-url ，使用hugo 的 shortcode 自动生成相对路径。

   ```text
   typora-root-url: {{.File.Dir | replaceRE "[^\\\\]+\\\\" "../" }}../static/blog/{{.BaseFileName}}
   
   #也可以单独加上设置图片复制到指定目录
   typora-copy-images-to: {{.File.Dir | replaceRE "[^\\\\]+\\\\" "../" }}../static/blog/{{.BaseFileName}}/{{.BaseFileName}}.assets/
   ```

3. md和静态资源结构

   ```text
   my_blog
   └── content
   	└── posts
           ├── first.md
           └── second.md
   └── static
   	└── blog
           ├── first.assets
           │   ├── image1
           │   ├── image2
           └── second.assets
           │   ├── image1
           │   ├── image2
   ```

4. Hugo 设置 config.toml设置文章目录

   ```text
   [Permalinks]
     posts = "blog/:filename"
   ```

5. md中图片路径

   ```text
   ![image](BaseFileName.assets/image.png)
   ```

6. 最终生成静态资源结构

      ```text
      blog
      └── first
          ├── first.assets
          │   ├── image1
          │   ├── image2
          └── first.md
      └── second
      	├── second.assets
          │   ├── image1
          │   ├── image2
          └── second.md
      ```

   


