# 1. 初识 HTML

**Hyper Type Markup Language**（超文本标记语言）

**超文本**包括：文字，图片，音频，视频等

**W3C**：World Wide Web Consortium 万维网联盟

**W3C标准**：结构化标准语言HTML，表现标准语言CSS，行为标准DOM

**文档对象模型DOM**是HTML和XML文档的编程接口

**META标签是HTML标记HEAD区的一个关键标签**，它位于HTML文档的<head>和<title>之间（有些也不是在<head>和<title>之间）。 它提供的信息虽然用户不可见，但却是文档的最基本的元信息。 <meta>除了提供文档字符集、使用语言、作者等基本信息外，还涉及对关键词和网页等级的设定。

```html
<!DOCTYPE html>
<html lang="en">
<!--代表网页头部-->
<head>
<!--    描述性标签，一般用来描述网站的一些信息，做搜索引擎优化（SEO）-->
    <meta charset="UTF-8">
    <meta name="keywords" content="isaiah learn html5, do you like it?"/>
    <meta name="description" content="in here we can learn freely"/>
<!--    代表网页标题（标签页名字）-->
    <title>my first title</title>
</head>
<!--代表网页主体-->
<body>
    Hello World!
</body>
</html>
```



# 2. 网页的标签

```html
<h1>
    标题标签 h1~h6
</h1>

<p>
    段落标签
</p>

<br/> 换行标签
<hr/> 水平线标签

字体样式标签
<strong>粗体</strong>
<em>斜体</em>

注释
<!-- 我是注释 -->

特殊符号
&nbsp; 空格
&gt; 大于符号
&lt; 小于符号
&copy; 版权符号©
&xxxx; 特殊符号
```



# 3. 图像标签

```html
<img src="path" alt="text" tilte="text" width="x" height="y"/>
src 为图像的地址（必填）
alt 为图像的替代文字（如果图片没加载出来，就显示alt的值）（必填）
title 为鼠标悬停在图片上时显示的文字
width 为图像的宽度
height 为图像的高度
```



# 4. 链接标签

```html
<a href="path" target="目标窗口位置">链接文本或图像</a>
href 跳转到的位置
target 表示窗口在哪里打开 
	_blank 在新标签页打开
	_self 在当前的网页打开

锚链接：使用id作为标记
<a id="top">顶部</a>
<a href="1.html#top"></a>

功能性链接
<a href="mailto:xxxxxx@qq.com">点击联系我</a>
```



# 5. 行内元素和块元素

行内元素：无论内容多少，该元素独占一行 \<p>\</p> \<h1>\</h1>

行内元素：宽度与内容的多少相关 \<strong>\</strong> \<em>\</em>



# 6. 列表标签

无序列表

```html
<ul>
    <li>c</li>
    <li>c++</li>
    <li>java</li>
    <li>python</li>
    <li>lisp</li>
</ul>
```

+ c
+ c++
+ java
+ lisp



有序列表

```html
有序列表 ol = order list
<ol>
    <li>c</li>
    <li>c++</li>
    <li>java</li>
    <li>python</li>
    <li>lisp</li>
</ol>

1. c
2. c++
3. java
4. lisp
```



自定义列表

```html
dl: 标签
dt: 列表名称
dd: 列表内容
<dl>
    <dt>学科</dt>   
    <dd>c</dd>
    <dd>c++</dd>
    <dd>java</dd>
    
    <dt>城市</dt>
    <dd>london</dd>
    <dd>tokyo</dd>
    <dd>hongkong</dd>
</dl>

学科
	c
	c++
	java
城市
	london
	tokyo
	hongkong
```



# 7. 表格标签

```html
tr 行row 
td 列data
在td 标签中设置colspan可以跨列
在td 标签中设置rowspan可以跨行
<table border="1px">
    <tr>
        <td>11</td>
        <td>12</td>
        <td>13</td>
    </tr>
    <tr>
        <td>21</td>
        <td>22</td>
        <td>23</td>
    </tr>
</table>
```



# 8. 媒体元素

```html
controls: 控制条
autoplay: 自动播放
<video src="path" controls autoplay></video>
<audio src="path" controls></audio>
```



# 9. 页面结构分析

| 元素名  | 描述                       |
| :------ | -------------------------- |
| header  | 标题，头部区域的内容       |
| footer  | 标记脚部区域的内容         |
| section | 页面中的一块独立的区域     |
| article | 独立的文章内容             |
| aside   | 相关的内容（常用于侧边栏） |
| nav     | 导航类辅助内容             |



# 10. iframe 内联框架

```html
src 引用的页面地址
name 框架标识名，可以作为超链接的target值
width 宽度
height 高度
<iframe src="path" name="mainFrame"></iframe>
```



# 11. 初识表单

```html
method: 规定如何发送表单数据
	get: 可以在url中看到我们提交的信息，不安全，但是效率高
	post: 在url看不到提交的信息，但是在请求中能轻易破解
action: 表示向何处发送表单数据
<form method="post" action="result.html">
    <p>
        名字：<input type="text" name="username">
    </p>
    <p>
        密码：<input type="password" name="pwd">
    </p>
    <p>
        <input type="submit">
        <input type="reset">
    </p>
</form>
```



# 12. 表单元素格式

| 属性      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| type      | 指定元素的类型，默认为text（password，checkbox，radio单选框，submit，reset，file，hidden，image，button） |
| name      | 指定表单元素的名称                                           |
| value     | 元素的初始值，当type为radio时必须指定一个值                  |
| size      | 指定表单元素的初始宽度，当type为text或password时，以字符为单位，其它以像素为单位（文本框的长度） |
| maxlength | type为text或password时，表示输入的最大字符数（最长能写几个字符） |
| checked   | type为radio或checkbox时，指定的按钮是否被选中                |

**都需要name属性，用来作为键值对里面的键**



# 13. 单选框，多选框和按钮

```html
radio 单选框
name 表示组
<p>
    <input type="radio" value="boy" name="sex" checked>男
    <input type="radio" value="girl" name="sex">女
</p>

checkbox 多选框
name 表示组
<p>爱好：
    <input type="checkbox" value="sleep" name="hobby">睡觉
    <input type="checkbox" value="code" name="hobby" checked>代码
    <input type="checkbox" value="chat" name="hobby">聊天
</p>

value: 按钮的名字
name: 提交的键值对里面，键的名称
<p>按钮：
    <input type="button" name="btn1" value="点击变长">
    <input type="image" src
</p>
```



# 14. 下拉框，文本域与文件域

```html
name是键，value是值
<P>下拉框：
    <select name="列表名称">
        <option value="china">中国</option>
        <option value="us" selected>美国</option>
        <option value="uk">英国</option>
    </select>
</P>

textarea 文本域
cols 列
rows 行
<p>
    <textarea name="textarea" cols="50" rows="10">文本内容</textarea>
</p>

文件域
<p>
    <input type="file" name="files">
    <input type="button" value="上传" name="upload">
</p>
```



# 15. 验证，滑块与搜索框

```html
邮箱验证（初级）
<p>
    <input type="email" name="email">
</p>

url验证
<p>
    <input type="url" name="url">
</p>

数字验证（step是加减号增加减少的值）
<p>
    <input type="number" name="num" max="100" min="0" step="10">
</p>

滑块
<p>
    <input type="range" name="voice" min="0" max="100" step="2">
</p>

搜索框（后面有一个x可以直接删除框里面已经输入的东西）
<p>
    <input type="search" name="search">
</p>
```



# 16. 表单的应用

+ readonly 只读（默认值）
+ disable 禁用（用不了）
+ hidden 隐藏（没有输入框）



# 17. 增强鼠标功能

```html
点击“点我”文字，即可点亮id为for值的输入框
<p>
    <label for="mark">点我</label>
    <input type="text" id="mark">
</p>
```



# 18. 表单的初级验证

旨在减轻服务器的压力，高级验证通过JS来实现

+ placeholder 输入框提示信息
+ required 不能为空，为空不能提交
+ pattern 正则表达式

