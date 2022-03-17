# 【前端学习笔记】HTML5


# 网页构成
html的三个组成部分：
>结构：HTML
>表现：CSS
>行为：JavaScript

## HTML
超文本标记语言（超文本指的是超链接）

标记语言是一套*标记标签* (markup tag)

负责网页三要素中的结构

### HTML标签

HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

- HTML 标签是由*尖括号*包围的关键词，比如 \<html>
- HTML 标签通常是*成对出现*的，比如 \<b> 和 \</b>
- 标签对中的第一个标签是*开始标签*，第二个标签是*结束标签*
- 开始和结束标签也被称为*开放标签*和*闭合标签*

### HTML元素

HTML 元素指的是从开始标签（start tag）到结束标签（end tag）的所有代码。

> 根元素、子元素：

```html
<!--根元素：网页中所有内容都要写在根元素里面。-->
<html>

	<!--子元素：-->
	<!--head是网页的头部，head中的内容不会在网页中直接出现，主要用来帮助浏览器和搜索引擎解析网页-->
	<head>
		
		<!--meta元素用来设置网页的元数据，这里meta用来设置网页的字符集，避免乱码-->
		<meta charset="utf-8">

		<!--网页标题，显示在浏览器标题栏上，搜索引擎会主要根据title内容来判断网页主要内容SEO-->
		<title>标题</title>
	</head>

	<!--body是html的子元素，表示网页的主体，网页中所有的可见内容都应写在body里-->
	<body>
		<h1>文章大标题</h1>
	</body
</html>
```

> body的子元素：
* 非自结束元素
  标题：h1、h2、h3、h4
  段落：p
* 自结束元素：\<img />、\<input />
  
>注释：\<!-- -->

### 元素的属性
元素中可以设置属性。
属性是名值对结构（x=y）。
属性和元素名或其他属性应该使用空格符隔开。
属性不能随便写，应根据文档中的规定编写，有些有属性名有些没有，如果有属性值应使用引号引起来""或''。

```html
例：
<h1>这是我的名字：<fount color="yellow" size='3'>哈哈</font>！</h1>
```

### 文档声明
doctype，文档声明用来告诉浏览器当前网页版本
>迭代版本：HTMLl4、XHTML2.0、HTML5

```html
文档声明写在第一行
<!doctype html>
<html>
	<head></head>
	<body></body>
</html>
```



### 字符编码

#### 番外：二进制
> 8 bit  =  1 byte(字节)
> 1024 byte  =  1 kb（千字节）
> 1024 kb  =  1 mb（兆字节）
> 1024 mb  =  1gb（g字节）
> 1024 gb  =  1 tb（特字节）
> 1024 tb  =  1 pb

所有数据在计算机底层都以二进制形式保存，文字也不例外。

所以一段文字在存储到内存中时，都需要转换成二进制编码，当我们读取这段文字时，计算机会将编码转换为字符供我们读取。

**编码**
* 将字符转换为二进制的过程称为编码

**解码**
* 将二进制转换为字符的过程称为解码

**字符集（charset）**
* 编码和解码采用的规则称为字符集

**乱码问题**
* 加密和解密采用的字符集不同会出现乱码问题

> **常见字符集**
> > ASCII
> > ISO88591
> > GB2312（国标）
> > GBK（国标扩展）
> > UTF-8（万国码，包含世界上所有语言所有符号，推荐使用）

meta元素
* 设置网页的字符集，避免乱码问题
* 放在head元素里

```html
<!doctype html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body></body>
</html>
```

#### 番外：使用VSCode
> **插件：**
> 中文包、live serve（浏览器实时显示）


> **常用快捷键：**
> 快速生成页面基本元素：! + tab
> 注释：ctrl + /
> 向下复制当前行：alt + shift +&darr;
> 自动生成英文段落：lorem
> 生成中文段落（下插件）：jw
> 自动排版：alt + shift + f

### 实体 entity
在网页中编写的多个空格会被解析成一个空格。

在网页中有些时候，我们不能直接书写一些特殊符号，比如多个连续空格、大于号、小于号。

如果我们需要在网页中书写这些特殊符号，则需要使用html中的 **实体**（转义字符）。

> **实体语法：**[常用的实体](https://www.w3school.com.cn/html/html_entities.asp)
> >& + 实体的名字 + ;
> >>空格&nbsp;：\&nbsp;
> >>大于&gt;：\&gt;
> >>小于&lt;：\&lt;
> >>版权符号&copy;：\&copy;

### meta元素
用于设置网页中的元数据，元数据是给浏览器和搜索引擎看的。
空元素，必须有开始标签，没有结束标签。

> **常用属性**：
> > charset 指定网页字符集
> > name 指定的数据名称
> > content 指定的数据内容
> > http-equiv 设置http协议，一般用来将页面重定向到另一个页面

```html
<!--指定关键字-->
<meta name="keywords" content="HTML5,前端">
<!--指定网站描述-->
<meta name="description" content="这是一个网站">
<!--3秒后将页面重定向到百度：
<meta http-equiv="refresh" content="3;url=https://www.baidu.com">
-->
```

### 语义化元素
在网页中，**html负责网页的结构**，所以在使用html标签时，应关注的时标签的语义而非样式。

因为通过css可使标签样式千变万化。
<br/>

**在页面中独占一行的元素叫做块元素。**
* 在网页中一般通过块元素来布局。
* 一般在块元素中放行内元素。
* p元素中不能放任何块元素（但是因为浏览器在解析网页时会自动对网页中不符合规范的内容修正，所以看起来生效了，可在devtools中看到纠正后的代码）。

> 标题元素：h1、h2、h3、h4、h5、h6
> 段落元素：p
> 换行长引用：blockquote

> 区分页面块（非必须）
> > header 网页头部
> > main 网页主体
> > footer 网页底部
> > nav 导航
> > aside 侧边栏
> > article 文章
> > section 区块
> > **div** 区块，使用最广，可替代上面的

**在页面中不会独占一行的元素叫行内元素（内联元素）。**
* 行内元素一般用于包裹文字。

> 斜：em
> 加粗：strong
> 引号引用：q
> 换行：br

> **span** 段落，使用最广

#### 列表
无序列表（使用较广）、有序列表、定义列表
```html
无序列表
<ul>
   <li></li>
   <li></li>
</ul>
有序列表
<ol>
   <li></li>
   <li></li>
</ol>
定义列表
使用dl创建定义列表，dt表示定义内容，dd对内容说明
<dl>
   <dt>选项</dt>
   <dd>说明</dd>
</dl>
```

#### 超链接a
超链接可以使我们从一个页面跳转到其他页面或当前页面其他位置。
a元素是行内元素，但是在a标签中可以嵌套除它自身外的任何元素。
> **属性**：
> &nbsp;
> href 指定跳转目标路径，可以是外部网站地址也可以是内部网页地址
> > href="#" 回到顶部
> > href="#id" 根据元素id跳转到对应位置
> > href="javascript:;" 点击后什么都不会发生，占位用

>target 指定连接打开位置
>> target="_self" 默认在当前页面打开
>> target="_blank" 在一个新的页面中打开

> id 每一个元素都可以添加id属性，同一个页面不能出现重复id属性

```html
<a href="http://www.baidu.com">超链接</a>

<!--a标签可以放div元素-->
<a href=""> 超链接<div></div> </a>

<!--同级目录下跳转-->
<a href="index.html">超链接</a>

<!--当在服务器内部跳转时，一般使用相对路径，写法：
“./” 或“../” -->
<a href="../index.html">超链接</a>
```

#### 图片img
图片元素用于向当前页面引入一个外部图片。
替换元素，介于块元素和行内元素之间
> **属性**：
> &nbsp;
> src 指定外部图片路径
> &nbsp;
> alt 描述（有些浏览器在图片无法显示时展示描述）
> &nbsp;
> width 图片宽度（单位 像素px）
> height 图片高度
>
> > 宽度或高度只修改一个，另一个会等比例缩放

> **图片格式**
> &nbsp;
> jpeg（jpg）支持颜色丰富，不支持透明效果和动图。
> &nbsp;
> gif 支持颜色较少，支持简单透明和动图
> &nbsp;
> png 支持颜色丰富，支持复杂透明，不支持动图
> &nbsp;
> webp 谷歌新推出的专门用来表示网页图片的一种格式，具备其他图片所有优点，不仅小功能也多
>
> > 缺点：兼容性不行

> base64
>
> > 将图片进行base64编码，这样可以直接将图片转换为字符，通过字符形式引入图片，使用场景低

```html
<img src="https://www.w3school.com.cn/i/eg_tulip.jpg">
<img src="./resources/1.jpg">

<!--base64格式图片-->
<img src="data:image/jpeg;base64,/9j/4QAYRUDWADAWD...">
```

#### 内联框架iframe
内联框架用于向当前页面引入一个其他页面。

> **属性：**
> &nbsp;
> src 指定要引入的网页的路径
> &nbsp;
> frameborder 指定内联框架的边框粗细

```html
<iframe src="https://www.baidu.com"></iframe>
```

#### 音视频
audio元素用来向页面中引入一个外部音频文件
> **属性：**
> controls 是否允许用户控制播放
> autoplay 自动播放，部分浏览器禁止此功能
> loop 循环播放

```html
<!--controls允许用户自己控制，autoplay自动播放-->
<audio src="./xx.mp3" controls autoplay></audio>

<!--可以多资源、提示不兼容浏览器写法-->
<audiocontrols autoplay>
	对不起您的浏览器不支持此音频格式。
	<source src="./xx.mp3">
	<source src="./xx1.mp3">
	<source src="./xx2.mp3">
</audio>
```

video元素用来向网页引入一个视频文件
使用方式和audio基本一样

```html
    <video controls>
        <source src="./xxx.mp4">
    </video>
```

#### Input

```html
<input type="text"\>
```

#### 选择框selecte

1. value=""  默认会排在最上面

````html
<select>
    <option disabled value="">请选择</option>
    <option value="A">A</option>
    <option value="B">B</option>
    <option value="C">C</option>
</select>
````
