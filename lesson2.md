## 第二讲 CSS基础和网页基本布局

主讲人: 张实唯(QQ 410421579)

#### 层叠样式表

CSS定义网页的表现形式，它的基本写法如下:

```css
selector {
	property1: value1;
	property2: value2;
}
```

其中selector是选择器，用于指代HTML中的部分元素，property和value则规定该元素的样式。比如下面的CSS规则:

```css
p {
	color: red;
	font-size: 24px;
}
```

它的含义是页面中所有的`<p>`标签中的文字都用红色、24px的字体显示。

#### CSS引入方法和优先顺序

有3种方法将CSS样式引入网页中:

1. 规定HTML标签的style属性(称为内联样式)，如`<p style="font-size:24px">Hello World!</p>`
2. 使用`<style>`标签(称为内部样式表)，如`<style>p { font-size:24px; }</style>`
3. 使用`<link>`标签(称为外部样式表)，如`<link href="sample.css" rel="stylesheet"/>`

其中方法3会让浏览器发出一个HTTP请求下载sample.css文件，然后将其中的规则应用到页面上。实际使用中方法1和方法3用得比较多，其中方法3的一个优点是实现文档内容和展现形式的分离，也利于多个页面使用同一份样式表。

这三种方法引入的样式规则往往会有冲突，即使是同一个文件里的规则，冲突也是非常常见的，比如下面的代码:

```css
p {
	color: red
}

.hello {
	color: black
}
```

第二条规则使用了类选择器，如果网页中有一个p标签同时又是hello类，此时两条规则就发生了冲突。发生冲突时，CSS根据以下几个规则确定优先级:

1. 精确的选择器 > 模糊的选择器
2. 内联样式 > 内部样式表 > 外部样式表 > 用户设置/浏览器插件 > 浏览器缺省设置
3. 同一个文件里，后声明的 > 先声明的

其中选择器的"精确度"在[这里](http://www.w3.org/TR/selectors/#specificity)有明确的规定。此外，使用`!important`可以让某条规则优先级提升到最前面，多个冲突的`!important`规则之间依然遵守上述优先顺序。

#### 选择器

选择器是用于指代HTML中某些元素的一种语言，就像正则表达式用来获取字符串中的某些部分，选择器用来获取HTML中的元素。选择器不仅出现在CSS里，凡涉及到HTML的事情都可能用到选择器，比如javascript DOM操作，爬虫等。

关于选择器的语法，建议阅读[w3school上的教程](http://www.w3school.com.cn/css/css_selector_type.asp)(从链接的这一节开始，一共10节)，此外英语水平好的可以阅读[官方文档](http://www.w3.org/TR/selectors/#selector-syntax)，关于选择器的几乎所有东西都在这里，并且权威度极高。主要看第4章、第6章和第8章。

#### 常用属性

CSS的属性特别多，而且对于某些特定的元素还有专门的属性，想要全部记住是不现实的，而且也意义不大。好在网上关于css的资料非常丰富，使用搜索引擎可以快速找到想要的属性名。举个例子，你现在希望让一个元素在鼠标移动到它上面的时候让它显示下划线，只需要先在[google](https://google.com)上搜索"css mouse over"，第一条结果的标题是`:hover`伪类；然后搜索"css underline"，第一条结果就是`text-decoration`，然后再去[MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)上查找`:hover`和`text-decoration`的用法即可。

不过以下几个属性比较常用，建议先了解一下：

1. 文本格式:
	- `font-size`：字体大小
	- `font-weight`：粗体
	- `font-family`：字体
	- `color`：文字颜色
	- `text-align`：对齐方式，不只是文字，`inline`和`inline-block`元素都有效，水平居中的方式之一
	- `line-height`：行高。设置较高的行高是竖直方向居中的方式之一
2. 布局:
	- `position`：布局方式，常见的有`static`,`absolute`,`relative`,`fixed`几种
	- `top`,`right`,`bottom`,`left`：在absolute和relative布局时这四个属性的含义是不同的
	- `float`：定义浮动布局
	- `clear`：用来清除浮动
	- `z-index`：用来定义元素离用户的远近，当有元素重叠时上面的元素会覆盖下面的
3. 盒模型:
	- `display`：常见的有`block`,`inline`,`inline-block`,`none`几种
	- `padding`：定义内补
	- `margin`：定义外边距
	- `border`：定义边框样式。实现圆形的元素比如头像等就可以用`border-radius: 50%`

#### 单位

CSS的值一般都带单位，比如"20px","20%","20rem","20vmin"等。此外对于特殊的对象，比如URL,base64等都有不同的写法。[这里](http://www.w3school.com.cn/cssref/css_units.asp)有少量关于长度和颜色的单位(不完整)。

需要特别注意的是百分比，在不同的属性里百分比的意义不同。比如`width`百分比是相对父元素，而`transform`是相对自身。`padding`总是相对于父元素的**宽度**，即使`padding-top`也是如此，这个特性可以用来实现固定长宽比。

#### 盒模型

盒模型是CSS布局时的一个重要概念，可以参考[MDN的介绍](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)

简单说一个元素占用的空间由外到内分别是`margin`,`border`,`padding`,`content`，其中`border`,`padding`和元素的内容算是元素“内部”的部分，像`background-color`的作用范围就是这一块，点击一个元素也只有点击到这个范围内才有效。`margin`可以理解为“该元素距离其它元素的最短保留距离”，因此当两个`margin`为`20px`的元素排列时，它们的`border`之间的距离是`20px`而不是`40px`。

#### 定位方式

CSS有很多种定位方式，嵌套是实现复杂布局的基本方式，很多看起来棘手的问题加几个wraper就能解决。

##### static

设置`position: static`可以让元素使用“普通”布局，这也是默认的布局方式。在这种布局方式下，内联(inline)元素按照`text-align`规定的方向在一行内排布直到换行，而块级(block)元素则另起一行，即使前一行并未排满。

##### relative

设置`position: relative`可以使用相对定位。相对定位的元素仍在普通文档流中占据位置，可以简单理解为在当成普通布局完成之后，再将该元素按照`top,right,bottom,left`属性的值进行偏移，而其它的元素保持普通布局的位置不变。

##### absolute

设置`position: absolute`可以使用绝对定位。绝对定位是指相对于**最近的已定位父元素**定位，其中"最近"是指HTML嵌套距离，而"已定位"是指relative/absolute/fixed的元素。绝对定位元素的`top,right,bottom,left`是指该元素到最近已定位父元素的相应边缘的距离。绝对定位的元素不在普通文档流中占据位置，因此如果不为绝对定位元素保留空白的话可能会和其它元素重叠。

由于`relative`定位不指定`top,right,bottom,left`属性的话和`static`是一样的，因此常常用它来指定`absolute`定位所相对的父元素。

##### fixed

`position: fixed`和绝对定位类似，但是它是相对于**浏览器窗口**定位，也就是说`fixed`元素的位置和它在HTML中的位置没有关联，而且随着用户滚动页面，`fixed`元素不会跟随其它元素被滚动，而是一直停留在原来的位置。

##### float

使用`float: left/right`可以让元素浮动。浮动定位时元素不在标准文档流中占据位置，并且向指定的方向移动到碰到父元素边缘或者其它浮动元素边缘为止。

关于浮动布局更详细一些的介绍可以参考[w3school的教程](http://www.w3school.com.cn/css/css_positioning_floating.asp)

##### *sticky

由于各浏览器对`sticky`定位的支持度并不高，不推荐使用。`sticky`的功能可以利用javascript实现。

##### *flex

`flex`布局是一种新的布局方式，它能够实现更为强大并且对各种尺寸的屏幕适应更好的布局，以往一些需要借助各种hack的难题比如垂直居中等使用`flex`可以轻易的解决。关于`flex`布局可以参考这两篇博客:

- [http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
- [http://www.ruanyifeng.com/blog/2015/07/flex-examples.html](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)

现阶段浏览器对flex布局的支持度还不是很高，但是将来flex布局应该会成为主流。

#### 推荐网站

CSS的功能很强大，但有的时候有些功能却不那么容易想到。下面几个网站里有很多常见问题的解决方案和一些有意思的CSS用法，可以收藏起来没事的时候浏览一下开开脑洞。

- [webheck](http://www.webhek.com)
- [css-tricks](https://css-tricks.com)
- [简明现代魔法](http://www.nowamagic.net/librarys/veda/channel/FrontEnd)

#### *Inspect

很多现代浏览器都提供了inspect的功能，以火狐为例，在页面任意元素上右键选择"inspect element"就可以查看渲染后的DOM树，并且右边的选项卡里可以看到该元素应用的CSS规则。CSS规则按照优先级从上到下排列，并且被覆盖的规则会用删除线标记。利用此功能可以在编写的CSS没有按照预期工作的时候查看问题到底出在哪里。

### 扩展阅读

CSS3有很多吸引人的特性，包括过渡特效(`transition`)，动画(`@keyframes`)，媒体查询(`@media`)，flexbox(`display: flex`)，3D效果及显卡加速(`translate3d`)等。其中不少已经被主流浏览器支持，可以用在实际项目中了，建议搜索学习一下。

### 作业

编写HTML和CSS还原[知乎的登陆页面](http://www.zhihu.com/#signup)(如果你已经登陆，需要先退出登陆)，要求如下:

1. 不需要实现点击效果，包括登陆标签、“注册知乎”按钮、“社交账户登陆”以及底下的一堆小链接
2. 右上角的"下载知乎APP"鼠标放上去弹出二维码的效果需要实现
3. 背景动画不需要，验证码用一张写死的图片就行
4. 原网页在不同的屏幕下会有不同的显示效果，只需要实现在>740px的设备(笔记本电脑)上的版本就行

提示:

1. 主体内容的水平居中可以利用`inline-block`+`text-align`实现，也可以利用`margin:auto`
2. “注册”标签下面的短线可以用`<hr>`元素，也可以利用`border-bottom`来实现
3. “注册”和“登陆”按钮建议使用`float`布局方式
4. 注册码图片可以使用`absolute`定位
5. "下载知乎APP"鼠标放上去弹出二维码的效果，使用`:hover`结合`display: none`就可以实现