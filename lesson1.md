## 第一讲 HTML和HTTP基础

主讲人: 张实唯(QQ 410421579)

#### Hello World

在学习如何编写网页之前，先学会怎么运行别人的网页是非常重要的，这样在接下来的学习过程中，你可以随时运行自己的网页来查看效果。下面几步使用Python作为简易的后台运行一个只有一个页面的网站，在"前端基础"系列课程中，我们都使用这个作为Web服务器。

*下面步骤中与操作系统相关的部分都以windows为例。Linux用户既然都用Linux了，想必知道怎么在Linux上完成这些操作。OS X用户如果对终端操作不了解的话，可以考虑Windows虚拟机。

第一步: 下载安装Python。在[官方下载页面](https://www.python.org/downloads/windows/)上可以找到下载链接。如果你不知道这些链接该下载哪个，那就[点击这里下载](https://www.python.org/ftp/python/3.4.4/python-3.4.4.amd64.msi)。

第二步: 使用终端。按Win+R组合键可以调出"运行"对话框，输入`cmd`并回车就可以打开命令行。觉得命令提示符不好看的可以使用PowerShell，win10用户使用Cortana就可以轻松打开PowerShell。建议创建cmd或者PowerShell的快捷方式，以后会经常使用。可以在命令行里输入`python -V`来检查Python是否安装成功。如果没有输出python的版本，而是提示找不到程序的话，可以[参考这里](http://jingyan.baidu.com/article/48206aeafdcf2a216ad6b316.html)添加环境变量。

第三步: 在任意位置建立一个文件夹，在里面建立一个空白文本文件并命名为"index.html"，并把下面的内容复制进去。编辑使用记事本就可以，不过强烈建议使用更加专业一些的文本编辑器，如[Sublime](http://www.sublimetext.com),[Atom](https://atom.io),[Emacs](http://www.gnu.org/software/emacs/emacs.html)或者[Vim](http://www.vim.org)。

```HTML
<!DOCTYPE html>
<html lang="zh_CN">
<head>
    <meta charset="utf8" />
    <title>Hello Wolrd!</title>
</head>
<body>
    <p>Hello World!</p>
</body>
</html>
```

第四步: 在刚建立的文件夹里**按住Shift**同时右键点击空白处，菜单里会有"在此处打开命令提示符"的选项，点击，然后运行`python -m http.server`，python2用户使用`python -m SimpleHTTPServer`，然后不要关闭终端，打开浏览器浏览[http://127.0.0.1:8000]()，你会看到Hello World!显示在页面上。

使用这种方式打开的页面是采用货真价实的HTTP协议进行的，只要其他人可以通过ip访问到你的电脑，他们的浏览器也可以用同样的方法打开你现在运行的这个网页。

#### 网站结构

我们通常说的Web网站分前端和后端两个部分，前端是指会被服务器通过HTTP协议发送到用户浏览器上的东西，包括HTML,CSS,Javascript以及图片等资源。而后端是运行在服务器上，接受HTTP请求并进行相应处理的程序，在我们刚才的例子里，python的http.server(或者SimpleHTTPServer)就是后端程序，而你复制的那段HTML是前端内容。

由于我们看到的网页大多不是一成不变的，而是会不断地更新，因此将所有网页写成HTML是不现实的，通常采用的方法是写某种形式的HTML模板，然后用模板和数据生成最终的HTML文件发送给浏览器。在05年之前，AJAX还未广泛应用的时候，主流的方法是在服务器上编写模板文件，然后服务器在接到HTTP请求的时候，从数据库查询数据并和模板拼接成HTML字符串相应请求。而在AJAX迅速普及开来之后，在前端获取数据并插入到DOM中的方法开始流行，这种方法的好处在于可以在不刷新页面的情况下请求多次数据并进行更新(Web APP)。

#### 网页结构 

可以参考[这篇文章](http://ued.taobao.org/blog/2008/10/understanding-progressiveen-hancement-chs-translation/)了解HTML,CSS,JavaScript三者的关系和分工。

#### HTML结构

HTML是网页内容的主体，写出具备清晰语义的HTML是Web前端工程师的基本功。HTML是一门标记语言，它和编程语言不同之处在于它是"描述性"的，也就是说HTML是告诉浏览器"此处是什么"，而不是"先干什么，再干什么"。对HTML来说，符合语义是非常重要的，它直接关系到你的网页的可访问性。一定要根据语义，而不是默认样式来选择标签。

MDN上的[这篇教程](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Introduction)比较简洁地说明了HTML的语法和一个HTML文件的组成部分，文章很短并且是中文的。

#### HTTP协议简介

HTTP是Web采用的应用层协议，它是不保持连接、无状态的一种通信协议。HTTP分为请求和相应，请求由客户端发出，而服务器在收到请求后作出响应。

请求主要包括Verb, URL, Headers和body四个部分的内容。Verb常用的值有`GET`,`POST`,`PUT`,`DELETE`四种，此外`HEADER`,`OPTION`等也很常见但是我们较少用到。URL则指服务器上的一个资源，它可能是服务器上对应路径的一个文件，也可能是数据库的一个表，甚至可以是抽象的东西比如Authentication。Headers是关于请求的一些信息，说明body的格式、客户端的操作系统和浏览器版本等。body则是发送给服务器的数据，比如用户在网页上的表单控件里填写的东西。

现代网站通常还会使用Cookie技术，Cookie的内容也会在请求里发送给服务器。[Cookie简介](https://zh.wikipedia.org/wiki/Cookie)

响应主要包括Status,Headers和body等，Status表示请求的结果，是成功了还是失败了，失败原因等。body则是请求成功的内容，比如请求到的HTML网页、图片等。

网站的URL如何设计是一个比较复杂的话题，不过有一些原则是得到大多数人认可的，比如RESTful，现在正在得到广泛地使用。[RESTful简介](http://www.ruanyifeng.com/blog/2011/09/restful)

### 扩展阅读

0. [HTML标签参考文档](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element)
1. [GET,POST,PUT,DELETE的用法和语义辨析](http://www.restapitutorial.com/lessons/httpmethods.html)
2. [常见的HTTP状态码](http://www.restapitutorial.com/httpstatuscodes.html)
3. [w3school的HTML教程](http://www.w3school.com.cn/html/html_getstarted.asp)

### 作业

1. 运行上述的Hello World并截图，要求截图中能看到浏览器访问的链接地址，必须是`http:`开头

2. 把[http://www.w3school.com.cn/tags/index.asp](http://www.w3school.com.cn/tags/index.asp)里面除了不赞成使用的标签以外都看一下标签的含义(最好能自己试一试)，并把这些标签分成以下几类:
    - 文本格式类，如`<li>`, `<h2>`, `<td>`
    - 语义块类，如`<code>`, `<footer>`, `<nav>`
    - 媒体类，如`<object>`, `<img>`
    - 表单类，如`<select>`
    - 其它，像iframe什么的比较特殊的东西

上述分类不能覆盖所有标签，其间也可能有重叠部分，对于不太好分类的标签单独说明一下它的含义即可。如果你觉得这样分类不好的话，也可以采用自己的分类标准。

作业提交一个截图，和分类结果。