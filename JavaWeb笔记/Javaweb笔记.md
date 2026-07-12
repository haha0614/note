**Javaweb笔记**

---



# Day01.web开发-介绍

## 什么是web

●  Web：全球广域网，也称为万维网（www World Wide Web）,能够通过浏览器访问的网站

## Web网站的工作流程

 ![img](./图片/clip_image002.jpg)

## Web前端开发

### Web标准

1. Web标准也称为网页标准，由一系列的标准组成，大部分由W3C（World Wide Web Consortium，万维网联盟）负责制定
2. 三个组成部分：

​		HTML：负责网页的结构（页面元素和内容）

​		CSS：负责网页的表现（页面元素的外观，位置等页面样式，如：颜色，大小等）

​		JavaScript：负责网页的行为（交互效果）

## HTML、CSS

### 什么是HTML、CSS

#### HTML

1. HTML（Hyper Text Markup Language）：超文本标记语言。

​			超文本：超越了文本的限制，比普通文本更强大。除了文字信息，还可以定义图片、音频、视频等内容

​			标记语言：由标签构成的语言

​			HTML标签都是预定义好的。例如：使用`<a>`展示超链接，使用`<img>`展示图片，`<video>`展示视频。	

​			HTML代码直接在浏览器中运行，HTML标签由浏览器解析

2. CSS

​			CSS（Cascading Style Sheet）：层叠样式表，用于控制页面的样式（表现）

#### HTML快速入门

```html
<html>
​    <head>
​       <title>HTML快速入门</title>
​    </head>
​    <body>
​       <h1>Hello HTML</h1>
​           <img src = "D:\AAA学习资料\02-Javaweb\资料\day01-HTML-CSS\资料\1.jpg"/>
​    </body>

</html>
```

#### 总结

```html
    1.HTML结构标签
    <html>
     <head>
    ​    <title>HTML快速入门</title>
     </head>
     <body>
    ​    <h1>Hello HTML</h1>
    ​        <img src = "D:\AAA学习资料\02-Javaweb\资料\day01-HTML-CSS\资料\1.jpg"/>
     </body>
    </html>
```

​		2.   特点

​			HTML标签不区分大小写

​			HTML标签属性值单双引号都可以

​			HTML语法松散

## HTML-新浪新闻

### 基本标签&样式

图片标签：`<img>`

​	●  src：指定图像的url（绝对路径/相对路径）

​	●  width：图像的宽度（像素/相对于父元素的百分比）

​	●  height：图像的高度（像素/相对于父元素的百分比）

标题标签：`<h1>~<h6>`

水平线标签：`<hr>`     

标题排版

```html
<!-- 文档类型为HTML -->
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- 指定字符集为UTF-8 -->
    <meta charset="UTF-8">
  <!-- 设置浏览器兼容性 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>焦点访谈：中国底气 新思想夯实大国粮仓</title>
</head>
<body>
  <!-- 
​    img标签：
​    src：图片资源路径
​    width：宽度(px：像素；%，相对于父元素的百分比)
​    height：高度
​        <img src="./img/news_logo.png" width="300px" height="300px">
​        <img src="./img/news_logo.png" width="80%" height="80%">

  路径书写方式：
​    绝对路径：
​      1.绝对磁盘路径：D:\AAA学习资料\02-Javaweb\Day01\img\1.jpg
​             <img src="D:\AAA学习资料\02-Javaweb\Day01\img\news_logo.png">
​      2.绝对网络路径
​             <img src="https://i2.sinaimg.cn/dy/deco/2012/0613/yocc20120613img01/news_logo.png">
​    相对路径：
​      \1. ./：当前目录，./可以省略的
​      ../：上一级目录
  -->
​ <img src="./img/news_logo.png">新浪政务>正文
  <h1>焦点访谈：中国底气 新思想夯实大国粮仓</h1>
  <hr>
  2024年6月3日18:00:26 央视网
  <hr>
</body>
</html>
```

### 标题样式

#### ●  CSS引入方式

行内样式：写在标签的style属性中（不推荐）

内嵌样式：写在style标签中（可以写在页面任何位置，但通常约定写在head标签中）

外联样式：写在一个单独的.css文件中（需要通过link标签在网页中引入）

####  颜色表现形式：

![img](./图片/clip_image004.jpg)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>焦点访谈：中国底气 新思想夯实大国粮仓</title>
  <!-- 方式二：内嵌样式 -->
    <style>
​    h1{
​      /* color: red; */
​      /* color: rgb(255, 0, 0); */
​      /* color: #f00; */
​      color: #4d4f53;
​    }
  </style>

  <!-- 样式三：外联样式 -->
  <!-- <link rel="stylesheet" href="./css/news.css"> -->
</head>
<body>
​    <img src="./img/news_logo.png">新浪政务>正文
  <!-- 方式一：行内样式 -->
  <!-- <h1 style="color: red;">焦点访谈：中国底气 新思想夯实大国粮仓</h1> -->

  <h1>焦点访谈：中国底气 新思想夯实大国粮仓</h1>
  <hr>
  2024年6月3日18:00:26 央视网
  <hr>
</body>
</html>
```

```html
news.css**
h1 {
  color: red;
}
```

●  CSS选择器：用来选取需要设置样式的元素（标签）

​		元素选择器 元素名称{}

​		id选择器   #id属性值

​		类选择器   .class属性值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>焦点访谈：中国底气 新思想夯实大国粮仓</title>
    <style>
​    h1{
​      color: #4d4f53;
​    }
​    /* 元素选择器 优先级最低*/
​    /* span{
​      color: #888888;
​    }  */

​    /* 类选择器 优先级其次*/
​    /* .cls{
​      color: #fff;
​    }  */

​    /* id选择器 优先级最高 */
​    \#time{
​      color: #888888;
​      font-size: 20px; /* 设置字体 */
​    }
  </style>
</head>
<body>
​    <img src="./img/news_logo.png">新浪政务>正文

  <h1>焦点访谈：中国底气 新思想夯实大国粮仓</h1>

  <hr>
  <span id="time" class="cls">2024年6月3日18:00:26 </span> <span>央视网</span> 
  <hr>
</body>
</html>
```

#### 小结

1. <span>标签

​		<span>是一个在开发网页时大量会用到的没有语义的布局标签

​		特点：一行可以显示多个（组合行内元素），宽度和高度默认由内容撑开

2. CSS选择器

​		元素选择器：标签名{…}

​		id选择器：#id{…}

​		类选择器：.class属性值{…}

​		优先级：id选择器 → 类选择器 → 元素选择器

3. CSS属性

​		color：设置文本颜色

​		font-size：字体大小（注意：记得加px）

### 超链接

●  标签：

`<a href="…" target="…">央视网</a>`

●  属性：

href：指定资源访问的url

target：指定在何处打开资源链接

_self：默认值，在当前页面打开

_blank：在空白页面打开

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>焦点访谈：中国底气 新思想夯实大国粮仓</title>
    <style>
​    h1{
​      color: #4d4f53;
​    }
​    \#time{
​      color: #888888;
​      font-size: 13px; 
​    }

​    a{
​      color:black;
​      /* 设置文本为一个标准的文本 */
​      text-decoration: none;
​    }
  </style>
</head>
<body>
​    <img src="./img/news_logo.png"> <a href="https://gov.sina.com.cn/" target="_self">新浪政务</a> >正文

  <h1>焦点访谈：中国底气 新思想夯实大国粮仓</h1>

  <hr>
  <span id="time" class="cls">2024年6月3日18:00:26 </span> <span> <a href="https://news.cctv.com/2023/03/02/ARTIUCKFf9kE9eXgYE46ugx3230302.shtml" target="_blank">央视网</a> </span> 
  <hr>
</body>
</html>
```

#### 小结

1. 超链接

​			标签：

​			`<a href="…" target="…">央视网</a>`

​			属性：

​				href：指定资源访问的url

​				target：指定在何处打开资源链接

​				_self：默认值，在当前页面打开

​				_blank：在空白页面打开

2. CSS属性

​				text-deforation：规定添加到文本的修饰，none表示定义标准的文本

​				color：定义文本的颜色

### 正文排版

●  视频标签：`<video>`

​		src：规定视频的url

​		controls：显示播放控件

​		width：播放器的宽度

​		height：播放器的高度

●  音频标签：`<audio>`

​		src：规定音频的url

​		controls：显示播放控件

●  段落标签：`<p>`

●  文本加粗标签：`<b>/<strong>`

●  换行：`<br>`

●  CSS样式

​		line-height：设置行高

​		text-indent：定义第一个行内容的锁紧

​		text-align：规定元素中的文本的水平对齐方式

●  注意

​		在HTML中无论输入多少个空格，只会显示一个，可以使用空格占位符：&nbsp;

### 页面布局

​    盒子：页面中所有的元素（标签），都可以看作是一个盒子，由盒子将页面中的元素包含在一个矩形区域内，通过盒子的视角更方便的进行页面布局

​    盒子模型组成：**内容区域（content）、内边距区域（padding）、边框区域（border）、        外边距区域（margin）**

●  布局标签：实际开发网页中，会大量频繁的使用div和span这两个没有语义的布局标签

●  标签：`<div><span>`

●  特点：

​		div标签：

​			一行只显示一个（独占一行）

​			宽度默认是父元素的宽度，高度默认由内容撑开

​			可以设置宽高

​		span标签：

​			一行可以显示多个

​			宽度和高度默认由内容撑开

​			不可以设置宽高

**注意：如果只需要设置某一个方位的边框、内边距、外边距、可以再属性名后加上-位置**

**如：padding-top、padding-left、padding-right**

## HTML表格标签

●  场景：在网页中以表格（行、列）姓氏整齐展示数据，如：班级表

●  标签

![img](./图片/clip_image006.jpg)

## 表单标签

●  场景：在网页中主要负责数据采集功能，如：注册、登录等数据采集

●  标签：`<form>`

●  表单项：不同类型的input元素、下拉列表、文本域等

​	`<input>`：定义表单项，通过type属性控制输入形式

​	`<select>`：定义下拉列表

​	`<textarea>`：定义文本域

●  属性：

​	`action`：规定当提交表单时向何处发送表单数据，URL

​	`method`：规定用于发送表单数据的方式。GET、POST

​	**注意：表单项必须有name属性才可以提交**

<!DOCTYPE html>

```html
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <!-- 
​    form表单属性：
​      action：表单提交的url，往何处提交数据，如果不指定，默认提交到当前页面
​      method：表单的提交方式
​        get:在url后面拼接表单数据，比如：?userName=Tom&age=12，url长度有限制   默认值
​        post：在消息体（请求体）中传递的，参数大小无限制的
   -->
   <form action="" method="post">
​    用户名:<input type="text" name="userName">
​    年龄:<input type="text" name="age">
​    <input type="submit" value="提交">
   </form>
</body>
</html>
```

## 表单项

●  `<input>`：表单项，通过type属性控制输入形式

![img](file:///C:/Users/QingQ/AppData/Local/Temp/msohtmlclip1/01/clip_image008.jpg)

●  `<selece>`：定义下拉列表，<option>定义列表项

●  `<textarea>`：文本域

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HTML-表单项标签</title>
</head>
<body>
<!-- value: 表单项提交的值 -->
<form action="" method="post">
   姓名: <input type="text" name="name"> <br><br>
   密码: <input type="password" name="password"> <br><br> 
   性别: <label><input type="radio" name="gender" value="1"> 男</label>
​     <label><input type="radio" name="gender" value="2"> 女 </label> <br><br>
   爱好: <label><input type="checkbox" name="hobby" value="java"> java </label>
​     <label><input type="checkbox" name="hobby" value="game"> game </label>
​     <label><input type="checkbox" name="hobby" value="sing"> sing </label> <br><br>
   图像: <input type="file" name="image">  <br><br>
   生日: <input type="date" name="birthday"> <br><br>
   时间: <input type="time" name="time"> <br><br>
   日期时间: <input type="datetime-local" name="datetime"> <br><br>
   邮箱: <input type="email" name="email"> <br><br>
   年龄: <input type="number" name="age"> <br><br>
   学历: <select name="degree">
​        <option value="">----------- 请选择 -----------</option>
​        <option value="1">大专</option>
​        <option value="2">本科</option>
​        <option value="3">硕士</option>
​        <option value="4">博士</option>
​     </select>  <br><br>
   描述: <textarea name="description" cols="30" rows="10"></textarea>  <br><br>
   <input type="hidden" name="id" value="1">

   <!-- 表单常见按钮 -->
   <input type="button" value="按钮">
   <input type="reset" value="重置"> 
   <input type="submit" value="提交">  
   <br>
</form>

</body>
</html>
```

# Day02 JavaScript

## 01-    JS介绍

●  JavaScript（简称：JS）是一门跨平台、面向对象的脚本语言。是用来控制网页行为的，他能使网页可交互

●  JavaScript和Java是完全不同的语言，不论是概念还是设计。但是基础语法类似

●  JavaScript在1995年由Brendan Eich发明，并于1997年称为ECMA标准

●  ECMAScript6（ES6）是最新的JavaScript版本（发布于2015年）

## 02-    JS引入方式

●  内部脚本：将JS代码定义在HTML页面中

​	JavaScript代码必须位于`<script></script>`标签之间

​	在HTML文档中，可以在任意地方，放置任意数量的`<script>`

​	一般会把脚本置于`<body>`元素的底部，可改善显示速度

●  外部脚本：将JS代码定义在外部js文件中，然后引入到HTML页面中

​	外部JS文件中，质保函JS代码，不包含`<script>`标签

​	`<script>`标签不能自闭合

![img](./图片/clip_image010.jpg)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JS-引入方式</title>
  <!-- 内部脚本 -->
  <!-- <script>
​    alert("hello JS")
  </script> -->

  <!-- 外部脚本 -->
    <script src="JS/demo.js"></script>
</head>
<body>

</body>
</html>
```

 外部JS文件

```html
alert("hello JS")
```

##  JavaScript基础语法-书写语法

1. 区分大小写：与Java一样，变量名、函数名以及其他一切东西都是区分大小写的
2. 每行结尾的分号可有可无
3. 注释
   1. 单行注释：//注释内容
   2. 多行注释：/*注释内容*/

4. 大括号代表代码块

##  输出语句

1. 使用`window.alert()`写入警告框

   ![image-20240604000332670](./图片/image-20240604000332670.png)

2. 使用`document.write()`写入HTML输出

   ![image-20240604000400916](./图片/image-20240604000400916.png)

3. 使用`console.log()`写入浏览器控制台

![image-20240604000415667](./图片/image-20240604000415667.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>

</body>
<script>
    // alert("JS");/*  */

    // 方式一：弹出警告框
    window.alert("hello js");

    //方式二：写入HTML页面中
    document.write('hello js');

    // 方式三：往浏览器控制台输出
    console.log('hello js');
</script>

</html>
```

##  JS基础语法-变量

1. JavaScript中用`var`关键字（variable的缩写）来声明变量。

1. JavaScript是一门弱类型语言，变量可以存放**<font color='red'>不同类型的值</font>**

1. 变量名需要遵循如下规则：

   1. 组成字符可以是任何字母、数字、下划线或者美元符号
   1. 数字不能开头
   1. 建议使用驼峰命名注意事项

   **<font color='red'>注意事项</font>**

   	- ECMAScript 6 新增了<font color='red'>`let`</font>关键字来定义变量，它的用法类似于var，但是所声明的变量，只在let关键字所在的代码块内有效，且不允许重复声明
   	- ECMAScript 6 新增了<font color='red'>`const`</font>关键字，用来声明一个只读的常量。一旦声明，常量的值就不能改变

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
</body>
<script>
    //var 定义变量
    // var a=10;
    // a='张三';
    // alert(a);

    //特点1：作用于比较大，属于全局变量
    //特点2：可以重复定义
    // {
    //     var x = 1;
    //     alert(x);//1
    //     var x = 'A';
    // }
    // alert(x);//A

    //let：局部变量；不能重复定义
    // {
    //     let x = 1;
    //     alert(x); //1
    // }
    
    //const：常量，不能被改变的
    const pi = 3.14;
    alert(pi);
</script>
</html>
```

##  JS-基础语法-数据类型&运算符

###  数据类型

- JavaScript中分为：原始类型 和 引用类型

  原始类型

  - <font color='red'>number</font>：数字（整数、小数、NaN（Not a Number））
  - <font color='red'>string</font>：字符串，单双引号皆可
  - <font color='red'>boolean</font>：布尔
  - <font color='red'>null</font>：对象为空
  - <font color='red'>undefined</font>：当声明的变量未初始化时，该变量的默认值是undefined

- 使用typeof运算符可以获取数据类型

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>

</body>

<script>
    //原始数据类型
    alert(typeof 3);//number
    alert(typeof 3.14);//number

    alert(typeof 'A');//string
    alert(typeof 'Hello');//string

    alert(typeof true);//boolean
    alert(typeof false);//boolean

    alert(typeof null);//object

    var a;
    alert(typeof a);//undefined
</script>

</html>
```

##  Day02-06-JS-函数

- 介绍：函数（方法）是被设计为执行特定任务的代码块

- 定义方式一：JavaScript函数通过function关键字进行定义，语法为

  ```html
  	function functionName(参数1，参数2...){
  ​		//要执行的代码
  ​	}
  ```

- 注意：

  - 形式参数不需要类型，因为JavaScript是弱类型语言
  - 返回值也不需要定义类型，可以在函数内部直接使用return返回即可

- 调用：函数名称（实际参数列表）

```html
    <script>
        function add(a,b){
            return a+b;
        }
        alert(add(1,2));
    </script>
```

- 定义方式二：

  ```html
  var functionName = function(参数1,参数2){
  	//要执行的代码
  }
  ```

  ```html
          //定义方法2
          var add = function(a,b){
              return a+b;
          }
  
          //方法调用
          var sum = add(1,2);
          alert(sum);
  ```

  - JS中，方法调用可以传递任意个数的参数。

##  JS-对象-Array数组

- JavaScript中Array对象用于定义数组

- 定义

  ```html
  var 变量名 = new Array(元素列表);//方式1
  var arr = new Array(1,2,3,4);
  
  var 变量名 = [元素列表];//方式二
  var arr = [1,2,3,4];
  ```

- 访问

  ```html
  arr[索引] = 值;
  arr[10] = "Hello";
  ```

  ```html
  <script>
          // var a = new Array(1,2,3,4);
          // console.log(a[0]);
  
          // var a = [1,2,3,4];
          // alert(a);//1,2,3,4
  
          //特点：长度可变，类型可变
          var arr = [1,2,3,4];
          arr[10] = 50;
          console.log(arr);//[1, 2, 3, 4, empty × 6, 50]
          console.log(arr[8]);//undefined
  
          arr[9] = 'A';
          arr[8] = true;
  
          console.log(arr);//[1, 2, 3, 4, empty × 4, true, 'A', 50]
      </script>
  ```

  <font color='red'>注意事项</font>

  - JavaScript中的数组相当于Java中的集合，数组的长度是可变的，而JavaScript是弱类型，所以可以存储任意的类型的数据

 ![image-20240604021231934](./图片/image-20240604021231934.png)

 

```html
		var arr = [1,2,3,4];
         for (let i = 0; i < arr.length; i++) {
            console.log(arr[i]);
            /*      1
                    2
                    3
                    4 */
        }
```

```html
        var arr = [1,2,3,4];

        arr.forEach(function(e){
            console.log(e);
        })

        //ES6 箭头函数：()=>{}--简化函数定义
        arr.forEach((e)=>console.log(e));

        arr.push(7,8,9);
        console.log(arr);//[1, 2, 3, 4, 7, 8, 9]

        arr.splice(2,2);
        console.log(arr);//[1, 2, 7, 8, 9]
```

 **箭头函数（ES6）**：是用来简化函数定义语法的。具体形式为：<font color='red'>(...)=>{...}</font>，如果需要给箭头函数起名字：<font color='red'>var xxx = (...)=>{...}</font>

## JS-String字符串

- String字符串对象创建方式有两种：

  ```html
  var 变量名 = new String("...");方式一
  var str = new String("Hello String");
  
  var 变量名 = "...";方式二
  var str = "Hello String";
  ```

  ![image-20240604022735070](./图片/image-20240604022735070.png)

```html
<script>
        //创建字符串对象
        //var str = new String('Hello String');
        var str = 'Hello World'; //Hello World
        console.log(str);

        //length
        console.log(str.length); //11

        //chatAt
        console.log(str.charAt(4)); //o

        //indexof
        console.log(str.indexOf("lo")); //3

        //trim
        str = '    Hello World    ';
        console.log(str.trim()); //Hello World

        //subString
        str = 'Hello World';
        console.log(str.substring(0, 5)); //Hello
    </script>
```

##  JS-对象-JSON

###  JavaScript自定义对象

- 定义格式

  ```html
  var 对象名 = {
  	属性名1:属性值1,
  	属性名2:属性值2,
  	属性名3:属性值3,
  	函数名称:function(形参列表){}
  };
  
  var user = {
  	name:'Tom',
  	age:20,
  	gender:'male',
  	ear:function(){
  		alert('用膳~');
  	}
  };
  ```

- 调用格式

  ```html
  对象名.属性名;
  console.log(user.name);
  
  对象名.函数名();
  user.eat();
  ```

  ```html
  <script>
          var user = {
              userName:'Tom',
              age:20,
              gender:'男',
              // eat:function(){
              //     alert("用膳~")
              // }
              eat(){
                  alert('用膳~');
              }
          }
  
          console.log(user.userName);//Tom
          user.eat();//用膳~
      </script>
  ```

### JSON-介绍

- 概念：JavaScript Object Notation，JavaScript对象标记法
- JSON是通过JavaScript对象标记语法书写的<font color='red'>文本</font>
- 由于其语法简单，层次结构鲜明，现多用于作为<font color='red'>数据载体</font>，在网络中进行数据传输

### JSON-基础语法

- 定义

  ```
  var 变量名 = '{"key1":value1,"key2":value2}';
  var userStr = '{"name":"Jerry","age":18,"addr":["北京","上海","西安"]}';
  ```

  ![image-20240604025205217](./图片/image-20240604025205217.png)

- JSON字符串转为JS对象

  ```html
  var jsObject = JSON.parse(userStr);	
  ```

- JS对象转为JSON字符串

  ```html
  var jsonStr = JSON.stringify(jsObject);
  ```

  ```html
  	<script>
          //定义JSON
          var jsonStr = '{"name":"Jerry","age":18,"addr":["北京","上海","西安"]}';
  
          //json字符串=>js对象
          var obj = JSON.parse(jsonStr);
          console.log(obj);
          /*  
              Object
              addr: (3) ['北京', '上海', '西安']
              age: 18
              name: "Jerry"
              [[Prototype]]: Object 
          */
  
          //js对象=>json字符串
          var objStr = JSON.stringify(obj);
          console.log(objStr);
          //{"name":"Jerry","age":18,"addr":["北京","上海","西安"]}
      </script>
  ```

## JS-对象-BOM

- 概念：Brower Object Model 浏览器对象模型，允许JavaScript与浏览器对话，JavaScript将浏览器的各个组成部分封装成对象。
- 组成：
  - **Window：浏览器窗口对象****
  - Navigator：浏览器对象
  - Screen：屏幕对象
  - History：历史记录对象
  - **Location：地址栏对象**

###  Window

- 介绍：浏览器窗口对象

- 获取：直接使用window，其中window.可以省略

  ```html
  window.alert("Hello Window");
  alert("Hello Window");
  ```

- 属性

  - <font color='red'>history</font>：对History对象的只读引用，请参阅[<font color='red'>History对象</font>](https://www.w3school.com.cn/js/js_window_history.asp)
  - <font color='red'>location</font>：用于窗口或框架的Location对象，请参阅[<font color='red'>Location对象</font>](https://www.w3school.com.cn/js/js_window_location.asp)
  - <font color='red'>navigator</font>：对Navigator对象的只读引用，请参阅[<font color='red'>Navigator对象</font>](https://www.w3school.com.cn/js/js_window_navigator.asp)

- 方法

  - <font color='red'>alert()</font>：显示带有一段消息和一个确认按钮的警告框
  - <font color='red'>confirm()</font>：显示带有一段消息已经确认按钮和取消按钮的对话框
  - <font color='red'>setInterval()</font>：按照指定的周期（以毫秒计）来调用函数或计算表达式
  - <font color='red'>setTimeout()</font>：在指定的毫秒数后调用函数或计算表达式

```html
	<script>
        //获取
        //window.alert('hello BOM');//hello BOM
        //alert("hello BOM window");//hello BOM window

        //方法
        //confirm--对话框--确认=>true--取消=>false
        //var flag = confirm("您确认删除吗");

        //定时器--setInterval--周期性的执行某一个函数
        //var count = 0;
        //setInterval(() => {
        //    count++;
        //    console.log("定时器执行了" + count + "次");
        //}, 2000);

        //定时器--setTimeout--延迟指定时间执行一次--只执行一次
        setTimeout(() => {
            alert("hh");
        }, 3000);
    </script>
```

###  Location

- 介绍：地址栏对象。

- 获取：使用window.location获取，window.可以省略

  ```html
  window.location.属性;			location.属性;
  ```

- 属性：

  - href：设置或返回完整的URL。

    ```html
    location.href = "https://www.itcast.cn";
    ```

```html
	<script>
        //location地址栏对象
        alert(location.href);
        //file:///D:/AAA%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/02-Javaweb/Day01/18.JS-%E5%AF%B9%E8%B1%A1-BOM.html

        location.href = "https://www.itcast.cn";
    </script>
```

##  JS-对象-DOM

- 概念：Document Object Model，文档对象模型
- 将标记语言的各个组成部分封装成对应的对象：
  - Document：整个文档对象
  - Element：元素对象
  - Attribute：属性对象
  - Text：文本对象
  - Comment：注释对象

![image-20240604124315323](./图片/image-20240604124315323.png)

- JavaScript通过DOM，就能够对HTML进行操作：

  - 改变HTML元素的内容
  - 改变HTML元素的样式（CSS）
  - 对HTML DOM事件做出反应
  - 添加和删除HTML元素

- DOM是W3C（万维网联盟）的标准，定义了访问HTML和XML文档的标准，分为3个不同的部分：

  1. Core DOM - 所有文档类型的标准模型
     - Document：整个文档对象
     - Element：元素对象
     - Attribute：属性对象
     - Text：文本对象
     - Comment：注释对象
  2. XML DOM - XML文档的标准模型

  3. HTML DOM - HTML文档的标准模型
     - Image：`<img>`
     - Button：`<inout type = 'button'>`

- HTML中的Element对象可以通过Document对象获取，而Document对象是通过window对象获取的。

- Document对象中提供了以下获取Element元素对象的函数：

  1. 根据id属性值获取，返回单个Element对象

     ```html
     var h1 = document.getElementById("h1");
     ```

  2. 根据标签名称获取，返回Element对象<font color='red'>数组</font>

     ```html
     var divs = document.getElementsByTagName('div');
     ```

  3. 根据name属性值获取，返回Element对象<font color='red'>数组</font>

     ```html
     var hobbys = document.getElementsByname('hobby');
     ```

  4. 根据class属性值获取，返回Element对象<font color='red'>数组</font>

     ```html
     var clss = document.getElementsByClassName('cls');
     ```

  ```html
  <script>
      //1.获取Element元素
      //1.1 获取元素-根据ID获取
      var h1 = document.getElementById("h1");
      //console.log(h1);//<img id="h1" src="img/off.gif">
      //alert(h1);//[object HTMLImageElement]
  
      //1.2 获取元素-根据标签获取
      var divs = document.getElementsByTagName("div");
      //console.log(divs);
      /*  
          HTMLCollection(2) [divs.cls, divs.cls]
          0: divs.cls
          1: divs.cls
          length: 2
          [[Prototype]]: HTMLCollection 
      */
      //alert(div)//[object HTMLCollection]
  
      //1.3 获取元素-根据name属性获取
      var ins = document.getElementsByName('hobby');
      //alert(ins);//[object NodeList]
      for (let i = 0; i < ins.length; i++) {
          //alert(ins[i]); 
          //[object HTMLInputElement]
          //[object HTMLInputElement]
          //[object HTMLInputElement]
      }
  
      //1.4 获取元素-根据class属性获取
      var cls = document.getElementsByClassName('cls');
      //alert(cls);//[object HTMLCollection]
      for (let i = 0; i < cls.length; i++) {
          //alert(cls[i]); 
          //[object HTMLDivElement]
          //[object HTMLDivElement]
      }
  
      //2.. 查询参考手册，属性、方法
      divs[0].innerHTML = '你好啊';
  </script>
  ```

## JS-对象-DOM案例

 通过DOM操作，完成如下效果实现

	1.  点亮灯泡
	1.  将所有的div标签的标签体内容后面加上：<font color='red'>very good</font>
	1.  使所有的复选框呈现被选中状态

 ![image-20240604132850133](./图片/image-20240604132850133.png)

```html
<script>
    var h1 = document.getElementById('h1');
    h1.src = "img/on.gif";

    var divs = document.getElementsByTagName('div');
    for (let i = 0; i < divs.length; i++) {
        divs[i].innerHTML += '<font color="red">very good</font>';
        
    }

    var checkBoxs = document.getElementsByName('hobby');
    for (let i = 0; i < checkBoxs.length; i++) {
        checkBoxs[i].checked = 1;
        
    }
</script>
```

##  JS-事件-事件绑定&常见事件

###  事件监听

- 事件：HTML事件是发生在HTML元素上的“事情”。比如：
  - 按钮被点击
  - 鼠标移动到元素上
  - 按下键盘按键
- 事件监听：JavaScript可以在事件被侦测到时<font color='red'>执行代码</font>

###  事件绑定

 方式一：通过HTML标签中的事件属性进行绑定

```html
<input type="button" onclick='on()' value='按钮1'>
<script>
	function on(){
		alert("我被点击了")
	}
</script>
```

方式二：通过DOM元素属性绑定

```html
<input type='button' id='btn' value='按钮2'>
<script>
	document.getElementById('btn').onclick=function(){
        alert('我被点击了')
    }
</script>
```

```html
<body>
    <!-- 方式一 -->
    <input type="button" id="btn1" value="事件绑定1" onclick="on()">
    <input type="button" id="btn2" value="事件绑定2">
</body>

<script>
    // 方式一
    function on(){
        alert('我被点击了！')
    }

    //方式二
    document.getElementById('btn2').onclick=function(){
        alert('我被点击了');
    }
</script>
```

### 常见事件

![image-20240604140900368](./图片/image-20240604140900368.png)

```html
<table width="800px" border="1" cellspacing="0" align="center" onmouseover="over()" onmouseout="out()">
        <tr>
            <th>学号</th>
            <th>姓名</th>
            <th>分数</th>
            <th>评语</th>
        </tr>
        <tr align="center">
            <td>001</td>
            <td>张三</td>
            <td>90</td>
            <td>很优秀</td>
        </tr>
        <tr align="center">
            <td>002</td>
            <td>李四</td>
            <td>92</td>
            <td>优秀</td>
        </tr>
    </table>

</body>

<script>
    //onload : 页面/元素加载完成后触发
    function load(){
        console.log("页面加载完成...")
    }

    //onclick: 鼠标点击事件
    function fn1(){
        console.log("我被点击了...");
    }

    //onblur: 失去焦点事件
    function bfn(){
        console.log("失去焦点...");
    }

    //onfocus: 元素获得焦点
    function ffn(){
        console.log("获得焦点...");
    }

    //onkeydown: 某个键盘的键被按下
    function kfn(){
        console.log("键盘被按下了...");
    }

    //onmouseover: 鼠标移动到元素之上
    function over(){
        console.log("鼠标移入了...")
    }

    //onmouseout: 鼠标移出某元素
    function out(){
        console.log("鼠标移出了...")
    }

    //onsubmit: 提交表单事件
    function subfn(){
        alert("表单被提交了...");
    }
</script>
```

## JS-事件-案例

通过事件监听及DOM操作，完成如下效果实现

1. 点击“点亮”按钮电量灯泡，点击“熄灭”按钮熄灭灯泡
2. 输入框鼠标聚焦后，展示小写；鼠标离焦后，展示大写
3. 点击“全选”按钮使所有的复选框呈现被选中的状态，点击“反选”按钮使所有的复选框呈现取消勾选的状态

```html
<body>
    <img src="img/off.gif" id="img">
    <br>
    <input type="button" id="btn1" value="点亮">
    <input type="button" id="btn2" value="熄灭">
    <input type="button" id="btn5" value="开关">
    <br>
    <input type="text" id="t1" value="ABC">
    <br>
    <label><input type="checkbox" name="cb">电影</label>
    <label><input type="checkbox" name="cb">旅游</label>
    <label><input type="checkbox" name="cb">游戏</label>
    <br>
    <input type="button" id="btn3" value="全选">
    <input type="button" id="btn4" value="反选">
</body>
<script>
    var img1 = document.getElementById('img');
    var btn = document.getElementById('btn5');
    btn.onclick=function(){
        if(img1.src == "file:///D:/AAA%E5%AD%A6%E4%B9%A0%E8%B5%84%E6%96%99/02-Javaweb/Day02/img/off.gif"){
            img1.src='img/on.gif';
            btn.value='状态：开';
        } else {
            img1.src = "img/off.gif";
            btn.value='状态：关';
        }
    }
    document.getElementById('btn1').onclick=function(){
        img1.src='img/on.gif';  
          
    }
    document.getElementById('btn2').onclick=function(){
        img1.src='img/off.gif';    
    }
    var t1 = document.getElementById('t1')
    t1.onblur=function(){
        t1.value=t1.value.toUpperCase();
    }
    t1.onfocus=function(){
        t1.value=t1.value.toLowerCase();
    }
    var cb = document.getElementsByName("cb");
    document.getElementById('btn3').onclick=function(){
        for (let i = 0; i < cb.length; i++) {
            cb[i].checked = 1;
        }
    }
    document.getElementById('btn4').onclick=function(){
        for (let i = 0; i < cb.length; i++) {
            if(cb[i].checked == 0){
                cb[i].checked = 1;
            } else {
                cb[i].checked = 0;
            }
        }
    }
```

## Vue-概述

### 什么是Vue？

- Vue是一套<font color='red'>前端框架</font>，免除原生JavaScript中的DOM操作，简化书写
- 基于<font color='red'>MVVM</font>（Model-View-ViewModel）思想，实现数据的<font color='red'>双向绑定</font>，将编程的关注点放在数据上
- 官网：https://v2.cn.vuejs.org/

```vue
框架：是一个半成品软件，是一套可重用的、通用的、软件基础代码模型。
基于框架进行开发，更加快捷、更加高效
```

![image-20240604151701048](./图片/image-20240604151701048.png)

### Vue快速入门

- 新建HTML页面，引入Vue.js文件

  ```html
  <script src='js/vue.js'></script> 
  ```

- 在JS代码区域，创建Vue核心对象，定义数据模型

  ```html
  <script>
  	new Vue({
          el:"#app",
          data:{
              message:"Hello Vue!"
          }
      })
  </script>
  ```

- 编写视图

  ```vue
  <div id='app'>
      <input type="text" v-model="message">
      {{ message }}
  </div>
  ```

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Vue-快速入门</title>
      引入Vue文件
      <script src="JS/vue.js"></script>
  </head>
  <body>
  
      <div id="app">
          <input type="text" v-model='message'>
          {{message}}
      </div>
      
  </body>
  <script>
          //定义vue对象
          new Vue({
              el:'#app',//Vue接管的区域
              data:{
                  message:"Hello Vue"
              },
          })
  </script>
  </html>
  ```

运行结果：

![image-20240604155217395](./图片/image-20240604155217395.png)

### 插值表达式

- 形式：{{表达式}}
- 内容可以是：
  - 变量
  - 三元运算符
  - 函数运用
  - 算术运算

## Vue-指令-v-model-bind&v-model&v-on

### 常用指令

- 指令：HTML标签上带有v-前缀的特殊属性，不同指令具有不同含义。例如：v-if、v-for

- 常用指令

  ![image-20240604155458656](./图片/image-20240604155458656.png)

![image-20240604155643626](./图片/image-20240604155643626.png)

- v-bind

  ```vue
  <a v-bind:href="url">。。。</a>
  <a :href="url">...</a>
  ```

- v-model

  ```vue
  <input type="text" v-model="ur;"
  ```

  ```html
  <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Vue-快速入门</title>
      <script src="JS/vue.js"></script>
  </head>
  
  <body>
  
      <div id="app">
          <a v-bind:href="url">链接1</a>
          <a :href="url">链接2</a>
          <input type="text" v-model="url">
      </div>
  
  
  </body>
  <script>
      //定义Vue对象
      new Vue({
          el: "#app",
          data: {
              url: "https://www.baidu.com"
          }
      })
  </script>
  ```

  ![image-20240604170139394](./图片/image-20240604170139394.png)

**<font color='red'>注意事项</font>**

- 通过v-bind或者v-model绑定的变量，必须在数据模型中定义

![image-20240604170257602](./图片/image-20240604170257602.png)

- v-on

  ```html
  <input type="button" value="按钮" v-on:click="handle()">
  <input type="button" value="按钮" @click="handle()">
  
  <script>
  	new Vue({
          el:"#app",
          data:{
              //...
          },
          methods:{
              handle:function(){
                  alert("我被点击了");
              }
          }
      })
  </script>
  ```

  ```html
  <body>
  
      <div id="app">
         <input type="button" v-on:click="handle" value="点我">
         <input type="button" @click="handle" value="点我呀">
      </div>
  
  
  </body>
  <script>
      //定义Vue对象
      new Vue({
          el: "#app",
          data: {
              
          },
          methods: {
              handle:function(){
                  alert("我被点击了")
              }
          }
      })
  </script>
  ```

## Vue-指令-v-if&v-show&v-for

![image-20240604184921839](./图片/image-20240604184921839.png)

- `v-if`

  ```html
  年龄{{age}},经判定为:
  <span v-if="age <= 35">年轻人</span>
  <span v-else-if="age > 35 && age < 60">中年人</span>
  <span v-else>老年人</span>
  ```

- `v-show`

  ```html
  年龄{{age}},经判定为：
  <span v-show="age <= 35">年轻人</span>
  ```

  ```html
  <body>
  
      <div id="app">
         年龄<input type="text" v-model="age">经判定为：
         <span v-if="age <= 35">年轻人</span>
         <span v-else-if="age > 35 && age < 60">中年人</span>
         <span v-else>老年人</span>
         <br><br>
         年龄<input type="text" v-model="age">经判定为：
         <span v-show="age <= 35">年轻人</span>
         <span v-show="age > 35 && age < 60">中年人</span>
         <span v-show="age > 60">老年人</span>
      </div>
  
  
  </body>
  <script>
      //定义Vue对象
      new Vue({
          el: "#app",
          data: {
              age:''
          }
      })
  </script>
  ```

  ![image-20240604190457844](./图片/image-20240604190457844.png)

- `v-for`

  ```html
  <div v-for="addr in addrs">{{addr}}</div>
  <div v-for="(addr,index) in addrs">{{index + 1}} : {{addr}}</div>
  
  data:{
  	...
  	addrs:['北京','上海','广州','深圳','成都','杭州']
  }
  ```

  ```html
  <body>
      <div id="app">
          <div id="app" v-for="addr in addrs">{{addr}}</div>
          <hr>
          <div id="app" v-for="(addr, index) in addrs">
              {{index + 1}} : {{addr}}
          </div>
      </div>
  </body>
  <script>
      //定义Vue对象
      new Vue({
          el: "#app",
          data: {
              addrs: ['北京', '上海', '西安', '成都', '深圳']
          }
      })
  </script>
  ```

  ![image-20240604191712808](./图片/image-20240604191712808.png)

## Vue-指令-案例

案例：通过Vue完成表格数据的渲染展示

![image-20240604191930996](./图片/image-20240604191930996.png)

```html
<body>
    <div id="app">
        <table cellspacing="0px" border="2px">
            <tr>
                <th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>性别</th>
                <th>成绩</th>
                <th>等级</th>
            </tr>
                <tr v-for="(user, index) in users">
                    <td>{{index+1}}</td>
                    <td>{{user.name}}</td>
                    <td>{{user.age}}</td>
                    <td >
                        <span v-if="user.gender == 1">男</span>
                        <span v-else>女</span>
                    </td>
                    
                    <td>{{user.score}}</td>
                    <td>
                        <span v-if="user.score >= 85">优秀</span>
                        <span v-else-if="user.score >= 60">及格</span>
                        <span v-else style="color: red;">不及格</span>
                    </td>
                </tr>
        </table>
    </div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app",
        data: {
            users: [{
                name: 'Tom',
                age: 20,
                gender: 1,
                score: 78
            }, {
                name: 'Rose',
                age: 18,
                gender: 2,
                score: 86
            }, {
                name: 'Jerry',
                age: 26,
                gender: 1,
                score: 90
            }, {
                name: 'Tony',
                age: 30,
                gender: 1,
                score: 52
            }]
        }
    })
</script>
```

## Vue-生命周期

生命周期

- 生命周期：指一个对象从创建到销毁的整个过程
- 生命周期的八个阶段：每触发一个生命周期 事件，会自动执行一个生命周期方法（钩子）

![image-20240604201036419](./图片/image-20240604201036419.png)

![image-20240604201203048](./图片/image-20240604201203048.png)

```html
<body>
    <div id="app">

    </div>
    
</body>
<script>
    //定义Vue对象
    new Vue({
        el:"#app",
        data:{

        },
        methods: {
            
        },
        mounted() {
            alert("Vue挂载完成，发送请求到服务端")
        }
    })
</script>
```

- `mounted`：挂在完成，Vue初始化成功，HTML页面渲染成功。（发送请求到服务端，加载数据）

# Day03.Ajax-介绍

- 概念：Asynchronous JavaScript And XML，<font color='red'>异步</font>的JavaScript和XML
- 作用：
  - 数据交换：通过Ajax可以给服务器发送请求，并获取服务器响应数据
  - 异步交互：可以在<font color='red'>不重新加载整个页面</font>的情况下，与服务器交换数据并<font color='red'>更新部分网页</font>的技术，如：搜索联想、用户名是否可用的校验等等

## 同步与异步

![image-20240604211631106](./图片/image-20240604211631106.png)

## 原生Ajax

1. 准备数据地址：http://yapi.smart-xwork.cn/mock/169327/emp/list
2. 创建XMLHttpRequest对象：用于和服务器交换数据
3. 向服务器发送请求
4. 获取服务器响应数据

```html
<body>
    <input type="button" value="获取数据" onclick="getData()">
    <div id="div1"></div>
</body>

<script>
    function getData(){
        //1.创建XMLHttpRequest对象
        var xmlHttpRequest = new XMLHttpRequest();
        //2.发送异步请求
        xmlHttpRequest.open('GET','http://yapi.smart-xwork.cn/mock/169327/emp/list');
        xmlHttpRequest.send();
        //3.获取服务响应数据
        xmlHttpRequest.onreadystatechange = function(){
            if(xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200){
                document.getElementById('div1').innerHTML = xmlHttpRequest.responseText;
            }
        }
    }
</script>
```

## Axios

- 介绍：Axios对原生的Ajax进行了封装，简化书写，快速开发
- 官网：https://www.axios-http.cn/

![image-20240604213125999](./图片/image-20240604213125999.png)

### Axios入门

1. 引入Axios的js文件

   ```html
   <script src='js/axios-0.18.0.js'></script>
   ```

2. 使用Axios发送请求，并获取响应结果

   ```html
   axios({
   	method:"get",
   	url:"http://yapi.smart-xwork.cn/mock/169327/emp/list"
   }).then(result)=>{
   	console.log(result.data);
   }
   
   axios({
   	method:"post",
   	url:"http://yapi.smart-xwork.cn/mock/169327/emp/deleteById",
   	data:"id=1"
   }).then(result)=>{
   	console.log(result.data);
   }
   ```

   请求方式别名

   - `axios.get(url[,config])`
   - `axios.delete(url[,config])`
   - `axios.post(url[,data[,config]])`
   - `axios.put(url[,data[,config]])`

- **发送GET请求：**

  ```html
  axios.get("http://...").then(result) => {
  	console.log(result.data);
  }
  ```

- **发送POST请求:**

  ```
  axios.post("http://...").then((resule) => {
  	console.log(result,data);
  })
  ```


## 前端工程化

### 前后端混合开发

![image-20240604232719289](./图片/image-20240604232719289.png)

### 前后端分离开发

当前最主流的开发模式：前后端分离

需求分析→接口定义（API接口文档）→前后端并行开发（遵守规范）→测试（前端、后端）→前后端联调测试

![image-20240604233444408](./图片/image-20240604233444408.png)

### YAPI

- 介绍：YAPI是高效、易用、功能强大的API管理平台，旨在为开发、产品、测试人员提供更优雅的接口管理服务
- 地址：[YApi Pro-高效、易用、功能强大的可视化接口管理平台](https://yapi.pro/group/337077)

![image-20240604233712337](./图片/image-20240604233712337.png)

- 添加项目
- 添加分类
- 添加接口

## 前端工程化-环境准备

### 实际的前端开发

![image-20240604235513718](./图片/image-20240604235513718.png)

**前端工程化**：是指在企业级的前端项目开发中，把前端开发所需的工具、技术、流程、经验等进行规范化、标准化。

### 环境准备

#### vue-cli

- 介绍：Vue-cli是Vue官方提供的一个脚手架，用于快速生成一个Vue的项目模板。
- Vue-cli提供了如下功能：
  - 统一的目录结构
  - 本地调试
  - 热部署
  - 单元测试
  - 集成打包上线
- 依赖环境：NodeJS

## 前端工程化-Vue项目

- 命令行：

  ```html
  vue create vue-project01
  ```

- 图形化界面：

  ```html
  vue ui
  ```

### Vue项目-目录结构

- 基于Vue脚手架创建出来的工程，有标准的目录结构，如下

![image-20240605053456241](./图片/image-20240605053456241.png)

![image-20240605053517965](./图片/image-20240605053517965.png)

### Vue项目-启动

![image-20240605053743165](./图片/image-20240605053743165.png)

![image-20240605053849944](./图片/image-20240605053849944.png)

### Vue项目-配置端口

```vue
const{ defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
	transpileDependencies: true
	devServer:{
		port:7000,
	}
})
```

## 前端工程化-Vue项目开发流程

- Vue的组件以.vue结尾，每个组件由三个部分组成：`<template>`、`<script>`、`<style>`。
- 模板部分，由它生成HTML代码：

```vue
<template>
  <div>
    <h1>{{ massage }}</h1>
  </div>
</template>
```

- 控制模板的数据来源和行为：

  ```vue
  <script>
  export default {
    data() {
      return {
        massage:"Hello Vue222"
      }
    },
    methods: {
      
    } 
  }
  </script>
  ```

- CSS样式部分：

  ```vue
  <style>
  
  </style>
  
  ```

## Element-快速入门

### 什么是Element

- Element：是饿了么团队研发的，一套为开发者、设计师和产品经理准备的基于Vue2.0的桌面端<font color='red'>组件</font>库。
- 组件：组成网页的部件，例如 超链接、按钮、图片、表格、表单、分页条等等
- 官网：https://element.eleme.cn/#/zh-CNListener

### 快速入门

- 安装ElementUI组件库（在当前工程的目录下），在命令行执行命令

  ```
  npm install element -ui@2.15.3
  ```

- 引入ElementUI组件库

  ```vue
  import ElementUI from 'element-ui';
  import 'element-ui/lib/theme-chalk/index.css';
  Vue.use(ElementUI);
  								main.js
  ```

- 访问官网，复制组件代码，调整

![image-20240605135303461](./图片/image-20240605135303461.png)

```vue
//src\views\Element\ElementVue.vue
<template>
    <div>
        <el-row>
            <el-button>默认按钮</el-button>
            <el-button type="primary">主要按钮</el-button>
            <el-button type="success">成功按钮</el-button>
            <el-button type="info">信息按钮</el-button>
            <el-button type="warning">警告按钮</el-button>
            <el-button type="danger">危险按钮</el-button>
        </el-row>
    </div>
</template>

<script>
export default {

}
</script>

<style></style>
```

```vue
//src\main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
//引入ElementUI文件
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.config.productionTip = false
Vue.use(ElementUI);

new Vue({
  router,
  render: h => h(App)
}).$mount('#app')
```

```vue
//src\App.vue
<template>
  <div>
    <!-- <h1>{{ massage }}</h1> -->
    <element-vue></element-vue>
  </div>
</template>

<script>
import ElementVue from './views/Element/ElementVue.vue'
export default {
  components: { ElementVue },
  data() {
    return {
      massage: "Hello Vue222"
    }
  },
  methods: {

  }
}
</script>
<style></style>
```

## Element-组件-Table表格

- Table表格：用于展示多条结构类似的数据，可对数据进行排序、筛选、对比或其他自定义操作

![image-20240605145703466](./图片/image-20240605145703466.png)

```vue
<template>
	<div>
         <!-- Table表格 -->
        <el-table :data="tableData" border style="width: 100%">
            <el-table-column prop="date" label="日期" width="180">
            </el-table-column>
            <el-table-column prop="name" label="姓名" width="180">
            </el-table-column>
            <el-table-column prop="address" label="地址">
            </el-table-column>
        </el-table>
    </div>
</template>


<script>
export default {
    data() {
        return {
            tableData: [{
                date: '2016-05-02',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }, {
                date: '2016-05-04',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1517 弄'
            }, {
                date: '2016-05-01',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1519 弄'
            }, {
                date: '2016-05-03',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1516 弄'
            }]
        }
    }
}
</script>
```

## 组件-Pagination 分页

- Pagination分页：当数据量过多时，使用分页分解数据

![image-20240605150503507](./图片/image-20240605150503507.png)

```vue
<template>
    <div>
        <!-- Pagination分页组件 -->
        <el-pagination background layout="sizes,prev, pager, next,jumper,->,total,slot" @size-change="handleSizeChange"
            @current-change="handleCurrentChange" :total="1000">
        </el-pagination>
    </div>
</template>

<script>
export default {
    data() {
        ...
    },
    methods: {
        handleSizeChange: function (val) {
            alert(val);
        },
        handleCurrentChange:function(val){
            alert(val);
        }
    }
}
</script>
```

## Element-组件-Dialog对话框

- Dialog对话框：在保留当前页面状态的情况下，告知用户并承载相关操作

![image-20240605152450030](./图片/image-20240605152450030.png)

```vue
<template>
    <div>
        <!-- Dialog对话框--Table -->
        <el-button type="text" @click="dialogTableVisible = true">打开嵌套表格的 Dialog</el-button>

        <el-dialog title="收货地址" :visible.sync="dialogTableVisible">
            <el-table :data="gridData">
                <el-table-column property="date" label="日期" width="150"></el-table-column>
                <el-table-column property="name" label="姓名" width="200"></el-table-column>
                <el-table-column property="address" label="地址"></el-table-column>
            </el-table>
        </el-dialog>
    </div>
</template>

<script>
export default {
    data() {
        return {
            gridData: [{
                date: '2016-05-02',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }, {
                date: '2016-05-04',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }, {
                date: '2016-05-01',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }, {
                date: '2016-05-03',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }],
            dialogTableVisible: false,
            dialogFormVisible: false,
        }
    }
}
</script>
```

## Element-组件-Form表单

- Form表单：由输入框、选择器、单选框、多选框等控件组成，用以收集、校验、提交数据

![image-20240605153135557](./图片/image-20240605153135557.png)

```vue
<template>
        <!-- Dialog对话框--Form表单 -->
        <el-button type="text" @click="dialogFormVisible = true">打开Form表单的 Dialog</el-button>

        <el-dialog title="Form表单" :visible.sync="dialogFormVisible">
            <el-form ref="form" :model="form" label-width="80px">

                <el-form-item label="活动名称">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>

                <el-form-item label="活动区域">
                    <el-select v-model="form.region" placeholder="请选择活动区域">
                        <el-option label="区域一" value="shanghai"></el-option>
                        <el-option label="区域二" value="beijing"></el-option>
                    </el-select>
                </el-form-item>

                <el-form-item label="活动时间">
                    <el-col :span="11">
                        <el-date-picker type="date" placeholder="选择日期" v-model="form.date1"
                            style="width: 100%;"></el-date-picker>
                    </el-col>
                    <el-col class="line" :span="2">-</el-col>
                    <el-col :span="11">
                        <el-time-picker placeholder="选择时间" v-model="form.date2" style="width: 100%;"></el-time-picker>
                    </el-col>
                </el-form-item>

                <el-form-item>
                    <el-button type="primary" @click="onSubmit">提交</el-button>
                    <el-button>取消</el-button>
                </el-form-item>
            </el-form>

        </el-dialog>
    </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                name: '',
                region: '',
                date1: '',
                date2: '',
            }
        }
    },
    methods: {
        onSubmit() {
            alert(JSON.stringify(this.form));
        }
    }
}
</script>
```

## Element-案例-基本页面布局

根据页面原型完成员工管理页面开发，并通过Axios完成数据异步加载

步骤：

- 创建页面，完成页面的整体布局规划
- 布局中各个部分的组件实现
- 列表数据的异步加载，并渲染展示

![image-20240605155150886](./图片/image-20240605155150886.png)

## Element-案例-axios异步加载数据

Vue项目中使用Axios：

- 在项目目录下安装Axios：`npm install axios;`
- 需要使用Axios时，导入Axios：`import axios from 'axios';`

## Vue路由

前端路由：URL中的hash（#号）与组件之间的对应关系

### Vue Router

- 介绍：Vue Router是Vue官方路由
- 组成
  - Vue Router：路由器类，根据路由请求在路由视图中动态渲染选中的组件
  - `<router-link>`：请求链接组件，浏览器会解析成`<a>`
  - `<router-view>`：动态视图组件，用来渲染展示与路由路径对应的组件

- 使用：

  - 安装（创建Vue项目时可以选择）

    ```
    npm install vue-router@3.5.1
    ```

  - 定义路由

    ```vue
    index.js中
    const routes = [
    	path:'',
    	name:'',
    	component:()=>import('')
    ]
    const router = new VueRouter({
    	routers
    })
    export default router
    ```

  ```vue
  app.vue
  <template>
    <div>
      <!-- <h1>{{ massage }}</h1> -->
      <!-- <element-vue></element-vue> -->
      <!-- <EMPView></EMPView> -->
      <router-view path="/"></router-view>
    </div>
  </template>
  
  
  <script>
  // import ElementVue from './views/Element/ElementVue.vue'
  // import EMPView from './views/emp/EMPView.vue'
  export default {
    components: { /* EMPView */ },
    data() {
      return {
        massage: "Hello Vue222"
      }
    },
    methods: {
  
    }
  }
  </script>
  <style></style>
  ```

  ```vue
  EMPView.vue
  <template>
      <div style="height: 100vh; background-color: green;">
          <h1 style="margin: 0;">管阿斯顿法师打发第三方理西宫</h1>
          <div class="div1">
              <el-aside width="100%" style="background-color: rgb(238, 241, 246);border:0px;height: 100%;">
                  <el-menu :default-openeds="['1', '3']">
                      <el-submenu index="1">
                          <template slot="title"><i class="el-icon-message"></i>系统信息管理</template>
                          <el-menu-item-group>
                             
                              <el-menu-item index="1-1" > 
                                  <router-link to="/dept">
                                      部门管理
                                  </router-link>
                              </el-menu-item>
                              <el-menu-item index="1-2"> <router-link to="/emp">员工管理</router-link></el-menu-item>
                          </el-menu-item-group>
                      </el-submenu>
                  </el-menu>
              </el-aside>
          </div>
              <div class="div2">
                  <div class="div2-1">
  
    
                  <el-form :inline="true" :model="formData.formName" class="demo-form-inline">
                      <el-form-item label="姓名">
                          <el-input v-model="formData.formName" placeholder="姓名"></el-input>
                      </el-form-item>
                      <el-form-item label="性别">
                          <el-select v-model="formData.gender" placeholder="性别">
                              <el-option label="男" value="1"></el-option>
                              <el-option label="女" value="2"></el-option>
                          </el-select>
                      </el-form-item>
                      <el-form-item label="入职日期">
                          <el-date-picker v-model="formData.formDate" type="daterange" range-separator="至"
                              start-placeholder="开始日期" end-placeholder="结束日期">
                          </el-date-picker>
                      </el-form-item>
                      <el-form-item>
                          <el-button type="primary" @click="onSubmit">查询</el-button>
                      </el-form-item>
                  </el-form>
                  <el-table :data="tableData" style="width: 100%" border="1px">
                      <el-table-column prop="name" label="姓名">
                          <template slot-scope="scope">
                              <span style="margin-left: 10px">{{ scope.row.name }}</span>
                          </template>
                      </el-table-column>
                      <el-table-column prop="image" label="图像">
                          <template slot-scope="scope">
                              <!-- <span style="margin-left: 10px">{{ scope.row.image }}</span> -->
                              <img :src=scope.row.image style="width:30px;height: auto;">
                          </template>
                      </el-table-column>
                      <el-table-column prop="gender" label="性别">
                          <template slot-scope="scope">
                              <span style="margin-left: 10px">{{ scope.row.gender == 1 ? '男' : '女' }}</span>
                          </template>
                      </el-table-column>
                      <el-table-column prop="job" label="职位">
                          <template slot-scope="scope">
                              <span style="margin-left: 10px">{{ scope.row.job }}</span>
                          </template>
                      </el-table-column>
                      <el-table-column prop="startDate" label="入职日期">
                          <template slot-scope="scope">
                              <span style="margin-left: 10px">{{ scope.row.stratDate }}</span>
                          </template>
                      </el-table-column>
                      <el-table-column prop="updateTime" label="最后操作时间">
                          <template slot-scope="scope">
                              <el-popover trigger="hover" placement="top">
                                  <div slot="reference" class="name-wrapper">
                                      <el-tag size="medium">{{ scope.row.updateTime }}</el-tag>
                                  </div>
                              </el-popover>
                          </template>
                      </el-table-column>
                      <el-table-column label="操作" width="200">
                          <template slot-scope="scope">
                              <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                              <el-button size="mini" type="danger"
                                  @click="handleDelete(scope.$index, scope.row)">删除</el-button>
                          </template>
                      </el-table-column>
                  </el-table>
  
              </div>
                  <div class="block">
                      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange"
                          :current-page="currentPage4" :page-sizes="[100, 200, 300, 400]" :page-size="100"
                          layout="total, sizes, prev, pager, next, jumper" :total="400">
                      </el-pagination>
                  </div>
              </div>
      </div>
  
  </template>
  
  
  
  <script>
  
  export default {
      data() {
          return {
              formInline: {
                  user: '',
                  region: ''
              },
              formData: {
                  formName: "",
                  formGender: 1,
                  formDate: []
              },
              tableData: [],
              currentPage1: 5,
              currentPage2: 5,
              currentPage3: 5,
              currentPage4: 4,
              sizeForm: {
                  name: '',
                  region: '',
                  date1: '',
                  date2: '',
                  delivery: false,
                  type: [],
                  resource: '',
                  desc: ''
              }
          }
  
  
      },
      methods: {
          handleOpen(key, keyPath) {
              console.log(key, keyPath);
          },
          handleClose(key, keyPath) {
              console.log(key, keyPath);
          },
          onSubmit() {
              console.log('submit!');
          },
          handleEdit(index, row) {
              console.log(index, row);
          },
          handleDelete(index, row) {
              console.log(index, row);
          },
          handleSizeChange(val) {
              console.log(`每页 ${val} 条`);
          },
          handleCurrentChange(val) {
              console.log(`当前页: ${val}`);
          },
          get() {
              this.$axios.get("https://yapi.pro/mock/414183/emp/getInfo").then((result) => {
                  console.log(result.data.data);
                  this.tableData = result.data.data;
              });
          }
      },
      mounted() {
          this.get();
      }
  
  }
  
  </script>
  <style>
  /* * {
      margin: 0;
      padding: 0;
  } */
  
  body {
      margin: 0;
  }
  
  .h {
      width: 100%;
      height: 10%;
      margin: 0;
      display: inline-block;
  }
  
  .div1 {
      width: 15%;
      height: 90%;
      display: inline-flex;
  }
  
  .div2 {
      width: 80%;
      height: 90%;
      display: table;
      display: inline-flex;
  }
  
  .el-col-12 {
      width: 100% !important;
      height: 100% !important;
      border: 10px;
  }
  
  .div2-1 {
      width: 100%;
      height: 90%;
      display: table;
  }
  
  .el-tag {
      border: none !important;
      background-color: transparent !important;
  }
  
  .demo-form-inline .el-form-item:nth-child(1) {
      width: 15%;
  }
  
  .demo-form-inline .el-form-item:nth-child(2) {
      width: 20%;
  }
  
  .demo-form-inline .el-form-item:nth-child(3) {
      width: 30%;
  }
  
  .demo-form-inline .el-form-item:nth-child(1) .el-form-item__content {
      width: 60%;
  }
  
  .demo-form-inline .el-form-item:nth-child(2) .el-form-item__content {
      width: 60%;
  }
  
  .demo-form-inline .el-form-item:nth-child(3) .el-form-item__content {
      width: 80%;
  }
  
  .el-col-2 {
      text-align: center !important;
  }
  
  .tac el-row {
      height: 100%;
      border: 10px;
  }
  </style>
  
  ```

  ```vue
  DeptView.vue
  <template>
      <div>
          <el-container  style="height: 700px; border: 1px solid #eee">
              <el-header style="font-size:40px; background-color: rgb(238, 241, 246)">tlias 智能学习辅助系统</el-header>
              <el-container>
                  <el-aside width="230px" style="border: 1px solid #eee">
                      <el-menu :default-openeds="['1', '3']">
                          <el-submenu index="1">
                              <template slot="title"><i class="el-icon-message"></i>系统信息管理</template>
                              <el-menu-item index="1-1">
                                  <router-link to="/dept">
                                      部门管理
                                  </router-link>
                              </el-menu-item>
                              <el-menu-item index="1-2">
                                  <router-link to="/emp">
                                      员工管理
                                  </router-link>
                              </el-menu-item>
                          </el-submenu>
                          </el-menu>
                  </el-aside>
  
  
                  <el-main>
                      <!-- 表格 -->
                      <el-table :data="tableData" border>
                          <el-table-column prop="name" label="名称" width="250"></el-table-column>
                          <el-table-column prop="updatetime" label="最后操作时间" width="250"></el-table-column>
                          <el-table-column label="操作">
                              <el-button type="primary" size="mini">编辑</el-button>
                              <el-button type="danger" size="mini">删除</el-button>
                          </el-table-column>
                      </el-table>
                  </el-main>
              </el-container>
          </el-container>
      </div>
  </template>
  
  <script>
  export default {
      data() {
          return {
              tableData: [{
                  id:1,
                  name:"学工部",
                  updatetime:"2010-01-01 12:00:00"
              },{
                  id:2,
                  name:"教研部",
                  updatetime:"2010-01-01 12:00:00"
              },{
                  id:3,
                  name:"就业部",
                  updatetime:"2010-01-01 12:00:00"
              },{
                  id:4,
                  name:"人事部",
                  updatetime:"2010-01-01 12:00:00"
              },{
                  id:5,
                  name:"行政部",
                  updatetime:"2010-01-01 12:00:00"
              }]
          }
      },
      methods: {
          
      }
  }
  </script>
  
  <style>
  
  </style>
  ```

  ```vue
  index.js
  
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  
  Vue.use(VueRouter)
  
  const routes = [
    {
      path: '/emp',
      name: 'emp',
      component: () => import('../views/emp/EMPView.vue')
    },
    {
      path: '/dept',
      name: 'dept',
      component: () => import('../views/emp/DeptView.vue')
    },
    {
      path: '/',
      redirect: '/dept'
    }
  ]
  
  const router = new VueRouter({
    routes
  })
  
  export default router
  
  ```

  ```vue
  main.js
  
  import Vue from 'vue'
  import App from './App.vue'
  import router from './router'
  //引入ElementUI文件
  import ElementUI from 'element-ui';
  import 'element-ui/lib/theme-chalk/index.css';
  import axios from 'axios';
  
  Vue.config.productionTip = false
  Vue.use(ElementUI);
  Vue.prototype.$axios = axios;
  new Vue({
    router,
    render: h => h(App)
  }).$mount('#app')
  
  ```

## 打包部署

### 打包

![image-20240606122242323](./图片/image-20240606122242323.png)

![image-20240606122255407](./图片/image-20240606122255407.png)

### 部署

NGINX

- 介绍：NGINX是一款轻量级的Web服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器。其特点是占用内存少，并发能力强，在各大型互联网公司都有非常广泛的使用。
- 官网：https://nginx.org/

![image-20240606123304002](./图片/image-20240606123304002.png)

- 部署：将打包好的dist目录下的文件，复制到NGINX安装目录下的HTML目录下。

- 启动：双击nginx.exe文件即可，NGINX服务器默认占用80端口号

```
CMD
netstat -ano | findStr 80
查询目前哪个进程占用了80端口号
```

```
如果端口号冲突
找到NGINX安装目录下的conf文件夹中的nginx.conf文件
找到里面的默认端口号listen    80;
改成其他不冲突的端口号，重启启动NGINX
```

```
打开浏览器，localhost:80/
```

# maven

Maven是Apache旗下的一个开源项目，是一款用于管理和构建Java项目的工具

Apache软件基金会，成立于1999年7月，是目前世界上最大的最受欢迎的开源软件基金会，也是一个专门为支持开源项目而生的非营利性组织

开源项目：https://www.apache.org/index.html#projects-list

## Maven的作用

![image-20240606125035478](./图片/image-20240606125035478.png)

![image-20240606125640145](./图片/image-20240606125640145.png)

![image-20240606125657540](./图片/image-20240606125657540.png)

## Maven概述-介绍&安装

介绍：

- Apache Maven是一个项目管理和构建工具，它基于项目对象模型（POM`<Project Object Model>`）的概念，通过一小段描述信息来管理项目的构建 
- 作用
  - 方便的依赖管理
  - 统一的项目结构
  - 标准的项目构建流程
- 官网：http://maven.apache.org/

![image-20240606130250964](./图片/image-20240606130250964.png)

仓库：用于存储资源，管理各种jar包。

- 本地仓库：自己计算机上的一个目录。
- 中央仓库：由Maven团队维护的全球唯一的。仓库地址：https://repo1.maven.org/maven2/
- 远程仓库（私服）：一般由公司团队搭建的私有仓库

### 安装

- 安装步骤：

  1. 解压apache-maven-版本号-bin.zip。

  2. 配置本地仓库：修改conf/setting.xml中的`<localRepository>`为一个指定目录。

     ```
     <localRepository>路径\mvn_repo</localRepository>
     ```

  3. 配置阿里云私服：修改conf/settings.xml中的`<mirrors>`标签，为其添加如下子标签：

     ```
     <mirror>
     	<id>alimaven</id>
     	<name>ailyun maven</name>
     	<url>http://maven.aliyun.con/nexus/content/gtoups/public/</url>
     	<mirrorOf>central</mirrorOf>
     </mirror>
     ```

  4. 配置环境变量：MAVEN_HOME为Maven的解压目录，并将其bin目录加入PATH环境变量

## maven-idea集成-配置及创建maven项目

### 配置maven环境（当前工程）

- 选择IDEA中File→Settings→Build,Execution,Deployment→Buile Tools→Maven
- 设置IDEA使用本地安装的Maven，并修改配置文件及本地仓库路径

### 配置Maven环境（全局）

![image-20240606145026565](./图片/image-20240606145026565.png)

![image-20240606145122261](./图片/image-20240606145122261.png)

![image-20240606145401051](./图片/image-20240606145401051.png)

![image-20240606145407518](./图片/image-20240606145407518.png)

### IDEA创建Maven项目

1. 创建模块，选择Maven，点击next
2. 填写模块名称，坐标信息，点击finish，创建完成
3. 编写HelloWorld，并运行

![image-20240606145811943](./图片/image-20240606145811943.png)

![image-20240606145911155](./图片/image-20240606145911155.png)

### Maven坐标

- 什么是坐标？
  - Maven中的坐标是<font color='red'>资源的唯一标识，通过该左边可以唯一定位资源位置</font>
  - 使用坐标来定义项目或引入项目中需要的依赖。

- Maven坐标主要组成

  - groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.qqzj）

  - artifactId：定义当前Maven项目名称（通常是模块名称，例如：order-service）

  - version：定义当前项目版本号

    ```xml
    通过坐标定义自己的项目
    	<groupId>org.example</groupId>
        <artifactId>maven-project01</artifactId>
        <version>1.0-SNAPSHOT</version>
    ```

    ```xml
    通过坐标引入项目所需要的依赖
    <dependency>
    	<groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    ```

### Maven-IDEA集成-导入Maven项目

![image-20240606153323881](./图片/image-20240606153323881.png)

## Maven-依赖管理

### 依赖配置

- 依赖：指当前项目运行所需要的jar包，一个项目中可以引入多个依赖。
- 配置
  1. 在pom.xml中编写`<dependencies>`标签
  2. 在`<dependencies>`标签中使用`<dependency>`引入坐标
  3. 定义坐标的`groupId`，`artifactId`，`version`
  4. 点击刷新按钮，引入最新加入的坐标

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>maven-project01</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.4.12</version>
        </dependency>
    </dependencies>

</project>
```

**<font color='red'>注意事项：</font>**

- 如果引入的依赖，在本地仓库不存在，将会链接远程仓库/中央仓库，然后下载依赖。（比较耗时）
- 如果不知道依赖的坐标信息，可以到https://mavnrepository.com/中搜索

### 依赖传递

- 依赖具有传递性

  - 直接依赖：在当前项目中通过依赖配置建立的依赖关系

    ![image-20240606155633316](./图片/image-20240606155633316.png)

  - 间接依赖：被依赖的资源如果依赖其他资源，当前项目间接依赖其他资源

    ![image-20240606155726637](./图片/image-20240606155726637.png)

- 排除依赖：

  排除依赖指主动断开依赖的资源，被排除的资源无需指定版本

```xml
<dependency>
	<groupId>com.qqzj</groupId>
	<artifactId>maven-projectB</artifactId>
	<version>1.0-SNAPSHOT</version>
    <exclusions>
    	<exclusion>
        	<groupId>junIt</groupId>
        	<artifactId>junIt</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>maven-projectA</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.4.12</version>
        </dependency>

        <dependency>
            <groupId>com.itheima</groupId>
            <artifactId>maven-projectB</artifactId>
            <version>1.0-SNAPSHOT</version>
            <!--排除依赖-->
            <exclusions>
                <exclusion>
                    <groupId>junit</groupId>
                    <artifactId>junit</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>
</project>

```

### 依赖范围

- 依赖的jar包，默认情况下，可以在任何地方使用。可以通过`<scope>...</scope>`设置其作用范围。
- 作用范围：
  - 主程序范围有效。（main文件夹范围内）
  - 测试程序范围有效。（test文件范围内）
  - 是否参与打包运行。（package指令范围内）

```xml
示例
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.10</version>
    <scope>test</scope>
</dependency>
```

![image-20240606162615821](./图片/image-20240606162615821.png)

### 生命周期

Maven的生命周期就是为了对所有的Maven项目构建过程进行抽象和统一

Maven中有3套<font color='red'>相互独立</font>的生命周期：

- clean：清理工作
- default：核心工作，如：编译、测试、打包、安装、部署等。
- site：生成报告，发布站点等。

![image-20240606164435202](./图片/image-20240606164435202.png)

每套生命周期包含一些阶段（phase），阶段是有顺序的，后面的阶段依赖于前面的阶段。

![image-20240606164559200](./图片/image-20240606164559200.png)

生命周期阶段：

- <font color='red'>clean</font>：移除上一次构建生成的文件
- <font color='red'>compile</font>：编译项目源代码
- <font color='red'>test</font>：使用合适的单元测试框架运行测试(junit)
- <font color='red'>package</font>：将编译后的文件打包，如：jar、war等
- <font color='red'>install</font>：安装项目到本地仓库

**<font color='red'>注意事项</font>**

- 在同一套生命周期中，当运行后面的阶段时，前面的阶段都会运行

执行声明周期的两种方式：

- 在IDEA中，右侧的Maven工具栏，选中对应的生命周期，双击执行
- 在命令行中，通过命令执行

## 总结

1. Maven生命周期
   - clean：清理
   - compile：编译
   - test：测试
   - package：打包
   - install：安装
   - <font color='red'>这些生命周期的执行都是由插件执行的</font>

# web入门

## Spring

- 官网：https://spring.io
- Spring发展到今天已经形成了一种开发生态圈，Spring提供了若干个子项目，每个项目用于完成特定的功能

![image-20240606172521069](./图片/image-20240606172521069.png)

![image-20240606172632069](./图片/image-20240606172632069.png)

**Spring Boot可以帮助我们非常快速的构建应用程序、简化开发、提高效率**

## Spring Boot Web-快速入门

需求：使用SpringBoot开发一个web应用，浏览器发起请求/hello后，给浏览器返回字符串“Hello World~”。

1. 创建SpringBoot工程，并勾选web开发相关依赖。
2. 创建HelloController类，添加方法hello，并添加注解
3. 运行测试

![image-20240606173413718](./图片/image-20240606173413718.png)

```java
启动类：
@SpringBootApplication
public class SpringBootWebQuickStartApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootWebQuickStartApplication.class, args);
    }
}
```

```java
请求处理类
//请求处理类
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello(){
        System.out.println("Hello World~");
        return "Hello World~";
    }
}
```

## HTTP协议

### 概述

- 概念：Hyper Test Transfer Pretocol，超文本传输协议，规定了浏览器和服务器之间数据传输的规则

![image-20240606180325308](./图片/image-20240606180325308.png)

- 特点：
  1. 基于TCP协议：面向连接，安全
  2. 基于请求-响应模型的：一次请求对应一次响应
  3. HTTP协议是无状态的协议：对于事务处理没有记忆能力。每次请求-相应都是独立的。
     - 缺点：多次请求间不能共享数据
     - 优点：速度快

### 请求协议

![image-20240606180957216](./图片/image-20240606180957216.png)

![image-20240606181011294](./图片/image-20240606181011294.png)

![image-20240606181026664](./图片/image-20240606181026664.png)

![image-20240606181314345](./图片/image-20240606181314345.png)

**<font color='red'>请求方式-GET：</font>**请求参数在请求行中，没有请求体，如：/brand/findAll?name=OPPO&status=1。GET请求大小是有限制的。

**<font color='red'>请求方式-POST：</font>**请求参数在请求体中，POST请求大小是没有限制的。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>请求方式演示-GET-POST</title>
</head>
<body>
    <form action="" method="get">
        姓 名: <input type="text" name="name"> <br>
        密 码: <input type="password" name="password"><br>
        <input type="submit" value="提交表单GET"><br>
    </form>

    <br><br><br>

    <form action="" method="post">
        姓 名: <input type="text" name="name"> <br>
        密 码: <input type="password" name="password"><br>
        <input type="submit" value="提交表单GET"><br>
    </form>

</body>
</html>
```

![image-20240606182044984](./图片/image-20240606182044984.png)

![image-20240606182058475](./图片/image-20240606182058475.png)

![image-20240606182115548](./图片/image-20240606182115548.png)

![image-20240606182223124](./图片/image-20240606182223124.png)

![image-20240606182247619](./图片/image-20240606182247619.png)

![image-20240606182323517](./图片/image-20240606182323517.png)

![image-20240606182455280](./图片/image-20240606182455280.png)

### 响应协议

![image-20240606182728150](./图片/image-20240606182728150.png)

![image-20240606182827868](./图片/image-20240606182827868.png)

![image-20240606182954987](./图片/image-20240606182954987.png)

#### 一、状态码大类

| 状态码分类 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 1xx        | **响应中**——临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
| 2xx        | **成功**——表示请求已经被成功接收，处理已完成                 |
| 3xx        | **重定向**——重定向到其它地方：它让客户端再发起一个请求以完成整个处理。 |
| 4xx        | **客户端错误**——处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
| 5xx        | **服务器端错误**——处理发生错误，责任在服务端，如：服务端抛出异常，路由出错，HTTP版本不支持等 |



#### 二、常见的响应状态码

| 状态码  | 英文描述                               | 解释                                                         |
| ------- | -------------------------------------- | ------------------------------------------------------------ |
| ==200== | **`OK`**                               | 客户端请求成功，即**处理成功**，这是我们最想看到的状态码     |
| 302     | **`Found`**                            | 指示所请求的资源已移动到由`Location`响应头给定的 URL，浏览器会自动重新访问到这个页面 |
| 304     | **`Not Modified`**                     | 告诉客户端，你请求的资源至上次取得后，服务端并未更改，你直接用你本地缓存吧。隐式重定向 |
| 400     | **`Bad Request`**                      | 客户端请求有**语法错误**，不能被服务器所理解                 |
| 403     | **`Forbidden`**                        | 服务器收到请求，但是**拒绝提供服务**，比如：没有权限访问相关资源 |
| ==404== | **`Not Found`**                        | **请求资源不存在**，一般是URL输入有误，或者网站资源被删除了  |
| 405     | **`Method Not Allowed`**               | 请求方式有误，比如应该用GET请求方式的资源，用了POST          |
| 428     | **`Precondition Required`**            | **服务器要求有条件的请求**，告诉客户端要想访问该资源，必须携带特定的请求头 |
| 429     | **`Too Many Requests`**                | 指示用户在给定时间内发送了**太多请求**（“限速”），配合 Retry-After(多长时间后可以请求)响应头一起使用 |
| 431     | **` Request Header Fields Too Large`** | **请求头太大**，服务器不愿意处理请求，因为它的头部字段太大。请求可以在减少请求头域的大小后重新提交。 |
| ==500== | **`Internal Server Error`**            | **服务器发生不可预期的错误**。服务器出异常了，赶紧看日志去吧 |
| 503     | **`Service Unavailable`**              | **服务器尚未准备好处理请求**，服务器刚刚启动，还未初始化好   |

状态码大全：https://cloud.tencent.com/developer/chapter/13553 

![image-20240606182838216](./图片/image-20240606182838216.png)

### 协议解析

```java
package com.itheima;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.charset.StandardCharsets;

/*
 * 自定义web服务器
 */
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(8080); // 监听指定端口
        System.out.println("server is running...");

        while (true){
            Socket sock = ss.accept();
            System.out.println("connected from " + sock.getRemoteSocketAddress());

            //开启线程处理请求
            Thread t = new Handler(sock);
            t.start();
        }
    }
}

class Handler extends Thread {
    Socket sock;

    public Handler(Socket sock) {
        this.sock = sock;
    }

    public void run() {
        try (InputStream input = this.sock.getInputStream(); OutputStream output = this.sock.getOutputStream()) {
                handle(input, output);
        } catch (Exception e) {
            try {
                this.sock.close();
            } catch (IOException ioe) {
            }
            System.out.println("client disconnected.");
        }
    }

    private void handle(InputStream input, OutputStream output) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));

        // 读取HTTP请求:
        boolean requestOk = false;
        String first = reader.readLine();
        if (first.startsWith("GET / HTTP/1.")) {
            requestOk = true;
        }

        for (;;) {
            String header = reader.readLine();
            if (header.isEmpty()) { // 读取到空行时, HTTP Header读取完毕
                break;
            }
            System.out.println(header);
        }
        System.out.println(requestOk ? "Response OK" : "Response Error");

        if (!requestOk) {// 发送错误响应:
            writer.write("HTTP/1.0 404 Not Found\r\n");
            writer.write("Content-Length: 0\r\n");
            writer.write("\r\n");
            writer.flush();
        } else {  // 发送成功响应:
            //读取html文件，转换为字符串
            InputStream is = Server.class.getClassLoader().getResourceAsStream("html/a.html");
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            StringBuilder data = new StringBuilder();
            String line = null;
            while ((line = br.readLine()) != null){
                data.append(line);
            }
            br.close();
            int length = data.toString().getBytes(StandardCharsets.UTF_8).length;

            writer.write("HTTP/1.1 200 OK\r\n");
            writer.write("Connection: keep-alive\r\n");
            writer.write("Content-Type: text/html\r\n");
            writer.write("Content-Length: " + length + "\r\n");
            writer.write("\r\n"); // 空行标识Header和Body的分隔
            writer.write(data.toString());
            writer.flush();
        }
    }
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<table border="1" cellspacing="0" width="500">
    <tr>
        <th>序号</th>
        <th>品牌名称</th>
        <th>企业名称</th>

    </tr>
    <tr align="center">
        <td>010</td>
        <td>三只松鼠</td>
        <td>三只松鼠</td>
    </tr>

    <tr align="center">
        <td>009</td>
        <td>优衣库</td>
        <td>优衣库</td>
    </tr>

    <tr align="center">
        <td>008</td>
        <td>小米</td>
        <td>小米科技有限公司</td>
    </tr>


</table>


</body>
</html>
```

## Tomcat

### 介绍

Web服务器是一个软件程序，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是“提供网上信息浏览服务”

- 概念：Tomcat是Apache软件基金会一个核心项目，是一个开源免费的轻量级Web服务器，支持Servlet/JSP少量JavaEE规范。
- JavaEE：Java Enterprise Edition，Java企业版。指Java企业级开发的技术规范总和。包含13项技术规范：JDBC、JDNI、EJB、RMI、JSP、S二v了他、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF
- Tomcat也被称为Web容器、Servlet容器。Servlet程序需要依赖于Tomcat才能运行
- 官网：https://tomcat.apache.org/

### 基本使用

![image-20240606204409866](./图片/image-20240606204409866.png)

![image-20240606205101155](./图片/image-20240606205101155.png)

<img src="./图片/image-20240606205141446.png" alt="image-20240606205141446"  />

![image-20240606205238731](./图片/image-20240606205238731.png)

### 入门程序解析（内置Tomcat）

![image-20240606205831603](./图片/image-20240606205831603.png)

**<font color='red'>起步依赖：</font>**

- `spring-boot-starter-web`：包含了web应用开发所需要的常见依赖
- `spring-boot-starter-test`：包含了单元测试所需要的常见依赖
- 官方提供的starter：https://docs.spring.io/spring-boot/docs/2.7.4/reference/htmlsingle/#using.build-systems.starters

![image-20240606210358808](./图片/image-20240606210358808.png)

# Day05Web后端开发

## 请求响应

![image-20240606210823269](./图片/image-20240606210823269.png)

![image-20240606210939606](./图片/image-20240606210939606.png)

![image-20240606211108887](./图片/image-20240606211108887.png)

- 请求（HttpServletRequest）：获取请求数据
- 响应（HttpServletResponse）：设置响应数据
- BS架构：Brower/Server，浏览器/服务器架构模式。客户端只需要浏览器，应用程序的逻辑和数据都存储在服务端
- CS架构：Client/Server，客户端/服务器架构模式

## 请求

### Postman

Postman是一款功能强大的网页调试与发送网页HTTP请求的Chrome插件

作用：常用于进行接口测试

![image-20240606213447583](./图片/image-20240606213447583.png)

### 参数

#### 简单参数

- 原始方式

  在原始的web程序中，获取请求参数，需要通过HttpServletRequest对象手动获取

  ```Java
  @RequestMapping("/simpleParam")
  public String simpleParam(HttpServletRequest request){
      String name = request.getParameter("name");
      String ageStr = request.getParameter("age");
      int age = Integer.parseInt(ageStr);
      System.out.println(name + ":" + age);
      return "OK";
  }
  ```

  ```java
  //测试请求参数接收
  @RestController
  public class RequestController {
      //原始方式
      @RequestMapping("/simpleParam")
      public String simpleParam(HttpServletRequest request) {
          String name = request.getParameter("name");
          String ageStr = request.getParameter("age");
          int age = Integer.parseInt(ageStr);
          System.out.println(name + ":" + age);
          return "OK";
      }
  }
  ```

#### 简单参数

- SpringBoot方式

  简单参数：参数名与形参变量名相同，定义形参即可接收参数

  ```java
  @RequestMapping("/simpleParam")
  public String simpleParam(String name, Integer age) {
          System.out.println(name + ":" + age);
          return "OK";
  }
  ```

  ```java
  //测试请求参数接收
  @RestController
  public class RequestController {
      //SpringBoot方式
      @RequestMapping("/simpleParam")
      public String simpleParam(String name,Integer age) {
          System.out.println(name + ":" + age);
          return "OK";
      }
  }
  ```

![image-20240606220856895](./图片/image-20240606220856895.png)

![image-20240606220922660](./图片/image-20240606220922660.png)

```java
//测试请求参数接收
@RestController
public class RequestController {
    //SpringBoot方式,当参数不一样时
    @RequestMapping("/simpleParam")
    public String simpleParam(String userName,Integer age) {
        System.out.println(userName + ":" + age);
        return "OK";
    }
}

控制台结果
    null:10
```

简单参数：如果方法形参名与请求参数名称不匹配，可以使用`@RequestParam`完成映射

```java
//测试请求参数接收
@RestController
public class RequestController {
    //SpringBoot方式,当参数不一样时
    @RequestMapping("/simpleParam")
    public String simpleParam(@RequestParam(name="name")String userName, Integer age) {
        System.out.println(userName + ":" + age);
        return "OK";
    }
}

控制台结果
    Tom:10
```

<font color='red'>注意事项：</font>

- <font color='#edd600'>@RequestParam</font>中的reguired属性默认为true，代表该请求参数必须传递，如果不传递将报错。如果该参数是可选的，可以将required属性设置为false。

![image-20240606222051005](./图片/image-20240606222051005.png)

```java
{

  "timestamp": "2024-06-06T14:20:33.472+00:00",

  "status": 400,

  "error": "Bad Request",

  "path": "/simpleParam"

}
```

#### 小结

1. 原始方式获取请求参数
   - Controller方法形参中声明HttpServletRequest对象
   - 调用对象的getParameter（参数名）
2. SpringBoot中接收简单参数
   - 请求参数名与方法形参变量名相同
   - 会自动进行类型转换
3. @RequestParam注解
   - 方法形参名称与请求参数名称不匹配，通过该注解完成映射
   - 该注解的required属性默认是true，代表请求参数必须传递

#### 实体参数

- 简单实体对象：请求参数名与形参对象属性名相同，定义POJO接收即可

![image-20240606225806599](./图片/image-20240606225806599.png)

```java
//实体参数
@RequestMapping("/simplePOJO")
public String simplePOJO(User user){
    System.out.println(user.getName() + ":" + user.getAge());
    return "OK";
}
```

- 复杂实体对象：请求参数名与形参对象属性名相同，按照对象层次结构关系即可接收嵌套POJO属性参数

![image-20240606230705868](./图片/image-20240606230705868.png)

```java
//复杂实体参数
@RequestMapping("/simplePOJO")
public String simplePOJO(User user) {
    System.out.println(
            user.getName() + ":" + user.getAge() + ":" + user.getAddress().getProvince() + ":" + user.getAddress().getCity());
    return "OK";
}
```

#### 小结

1. 实体对象参数

   规则：请求参数名与形参对象属性名相同，即可直接通过POJO接收

#### 数组集合参数

- 数组参数：请求参数名与形参数组名称相同且请求参数为多个，定义数组类型形参即可接收全部参数

![image-20240606232807128](./图片/image-20240606232807128.png)

```java
//数组参数
@RequestMapping("/arrayParam")
public String arrayParam(String[] hobby){
    System.out.println(Arrays.toString(hobby));
    return "OK";
}
```

- 集合参数：请求参数名与形参集合名称相同且请求参数为多个，`@RequestParam`绑定参数关系

![image-20240606233513851](./图片/image-20240606233513851.png)

```java
//集合参数
@RequestMapping("/arrayParam")
public String arrayParam(@RequestParam ArrayList<String> hobby){
    System.out.println(hobby);
    return "OK";
}
```

#### 小结

1. 数组集合参数

   数组：请求参数名与形参中数组变量名相同，可以直接使用数组封装

   集合：请求参数名与形参中集合变量名相同，通过`@RequestParam`绑定参数关系

#### 日期参数

日期参数：使用`@DateTimeFormat`注解完成日期参数格式转换

![image-20240606234125363](./图片/image-20240606234125363.png)

![image-20240606234437202](./图片/image-20240606234437202.png)

```java
//日期参数
@RequestMapping("/dateParam")
public String arrayParam(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")LocalDateTime dateTime){
    System.out.println(dateTime);
    return "OK";
}
```

#### JSON参数

- JSON参数：JSON数据<font color='red'>键名</font>与形参对象<font color='red'>属性名</font>相同，定义POJO类型形参即可接收参数，需要使用`@RequestBody`标识

![image-20240606235046373](./图片/image-20240606235046373.png)

![image-20240606235647581](./图片/image-20240606235647581.png)

```java
//JSON参数
@RequestMapping("/JSONPOJO")
public String JSONPOJO(@RequestBody User user){
        System.out.println(user.toString());
        return "OK";
}
```

#### 路径参数

- 路径参数：通过请求URL直接传递参数，使用{...}来标识该路径参数，需要用到`@PathVariable`获取路径参数

![image-20240607000140345](./图片/image-20240607000140345.png)

```java
//路径参数
@RequestMapping("/path/{id}")
public String pathParam(@PathVariable Integer id){
    System.out.println(id);
    return "OK";
}
```

![image-20240607000411658](./图片/image-20240607000411658.png)请求获取多级路径时：

![image-20240607001141444](./图片/image-20240607001141444.png)

```java
//多路径参数
@RequestMapping("/path/{id}/{name}")
public String pathParam(@PathVariable Integer id,@PathVariable String name){
    System.out.println(id + name);
    return "OK";
}
```

#### 总结

1. 简单参数
   - 定义方法形参，请求参数名与形参变量名一致
   - 如果不一致，通过`@RequestParam`手动映射

2. 实体参数
   - 请求参数名，与实体对象的属性名一致，会自动接收封装
3. 数组集合参数
   - 数组：请求参数名与数组名一致，直接封装
   - 集合：请求参数名与集合名一致，``@RequestParam`绑定关系

4. 日期参数
   - `@DateTimeFormat 形参`
5. JSON参数
   - `@RequestBody 形参`
6. 路径参数
   - `@PathVariable 形参`

## 响应

![image-20240607002246973](./图片/image-20240607002246973.png)

`@ResponseBody`

- 类型：**<font color='red'>方法注解、类注解</font>**
- 位置：Controller方法上/类上
- 作用：将方法返回值直接响应，如果返回值类型是实体对象/集合，将会转换为JSON格式响应
- 说明：`@RestController = @Controller + @ResponseBody;`

```java
@RequestMapping("/hello")
public String hello(){
    System.out.println("Hello World~");
    return "Hello World~";
}
```

![image-20240607003406794](./图片/image-20240607003406794.png)

```java
@RequestMapping("/getAddr")
public Address getAddr(){
    Address addr = new Address("广东","深圳");
    System.out.println(addr);
    return addr;
}
```

![image-20240607003428356](./图片/image-20240607003428356.png)

```java
@RequestMapping("/listAddr")
public List<Address> listAddr(){
    List<Address> list = new ArrayList<>();
    Address addr1 = new Address("广东","深圳");
    Address addr2 = new Address("山西","大同");
    Collections.addAll(list,addr1,addr2);
    System.out.println(list);
    return list;
}
```

![image-20240607003453477](./图片/image-20240607003453477.png)

### 统一响应结果

![image-20240607004054688](./图片/image-20240607004054688.png)

![image-20240607004102184](./图片/image-20240607004102184.png)

```java
package com.qqzj.POJO;

/**
 * 统一响应结果封装类
 */
public class Result {
    private Integer code ;//1 成功 , 0 失败
    private String msg; //提示信息
    private Object data; //数据 data

    public Result() {
    }
    public Result(Integer code, String msg, Object data) {
        this.code = code;
        this.msg = msg;
        this.data = data;
    }
    public Integer getCode() {
        return code;
    }
    public void setCode(Integer code) {
        this.code = code;
    }
    public String getMsg() {
        return msg;
    }
    public void setMsg(String msg) {
        this.msg = msg;
    }
    public Object getData() {
        return data;
    }
    public void setData(Object data) {
        this.data = data;
    }

    public static Result success(Object data){
        return new Result(1, "success", data);
    }
    public static Result success(){
        return new Result(1, "success", null);
    }
    public static Result error(String msg){
        return new Result(0, msg, null);
    }

    @Override
    public String toString() {
        return "Result{" +
                "code=" + code +
                ", msg='" + msg + '\'' +
                ", data=" + data +
                '}';
    }
}
```

### 案例

获取员工数据，返回统一响应结果，在页面渲染展示

- 在pom.xml文件中引入dom4j的依赖，用于解析XML文件
- 引入资料中提供的解析XML的工具类XMLParserUtils、对应的实体类EMP、XML文件emp.xml
- 引入资料中提供的静态页面文件，放在resources下的static目录下
- 编写Controller程序，处理请求，响应数据

<font color='red'>SpringBoot项目的静态资源（html，css，js等前端资源）默认存放目录为：classpath:/static、classpath:/public、classpath:/resources</font>

**emp.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>员工信息</title>
</head>

<link rel="stylesheet" href="element-ui/index.css">
<script src="./js/vue.js"></script>
<script src="./element-ui/index.js"></script>
<script src="./js/axios-0.18.0.js"></script>

<body>
    <h1 align="center">员工信息列表展示</h1>
    <div id="app">
        <el-table :data="tableData" style="width: 100%"  stripe border >
            <el-table-column prop="name" label="姓名" align="center" min-width="20%">
                <template slot-scope="scope">
                    <span style="margin-left: 10px">{{ scope.row.name }}</span>
                </template>
            </el-table-column>
            <el-table-column prop="age" label="年龄" align="center" min-width="20%"></el-table-column>
            <el-table-column label="图像" align="center"  min-width="20%">
                <template slot-scope="scope">
                    <el-image :src="scope.row.image" style="width: 80px; height: 50px;"></el-image>
                </template>
            </el-table-column>
            <el-table-column prop="gender" label="性别" align="center"  min-width="20%"></el-table-column>
            <el-table-column prop="job" label="职位" align="center"  min-width="20%"></el-table-column>
        </el-table>
    </div>
</body>

<style>
    .el-table .warning-row {
        background: oldlace;
    }
    .el-table .success-row {
        background: #f0f9eb;
    }
</style>

<script>
    new Vue({
        el: "#app",
        data() {
            return {
                tableData: []
            }
        },
        mounted(){
            axios.get('emp.html/listEmp').then(res=>{
                if(res.data.code){
                    this.tableData = res.data.data;
                }
            });
        },
        methods: {
        }
    });
</script>
</html>
```

**empController.java**

```java
package com.qqzj.controller;

import com.qqzj.POJO.Result;
import com.qqzj.POJO.EMP.Emp;
import com.qqzj.POJO.EMP.XmlParserUtils;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class empController {
    @RequestMapping("emp.html/listEmp")
    public Result list(){
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        List<Emp> empList = (List<Emp>)XmlParserUtils.parse(file, Emp.class);
        //遍历集合修改性别和工作的数据
        for (Emp emp : empList) {
            String gender = emp.getGender();
            if ("1".equals(gender)){
                gender = "男";
            }else{
                gender = "女";
            }
            String job = emp.getJob();
            if ("1".equals(job)){
                job = "讲师";
            }else if("2".equals(job)){
                job = "班主任";
            }else{
                job = "就业指导";
            }
            emp.setGender(gender);
            emp.setJob(job);
        }
        System.out.println(empList);
        return Result.success(empList);
    }
}
```

**emp.xml:**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<emps>
    <emp>
        <name>金毛狮王</name>
        <age>55</age>
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/1.jpg</image>
        <!-- 1: 男, 2: 女 -->
        <gender>1</gender>
        <!-- 1: 讲师, 2: 班主任 , 3: 就业指导 -->
        <job>1</job>
    </emp>

    <emp>
        <name>白眉鹰王</name>
        <age>65</age>
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/2.jpg</image>
        <gender>1</gender>
        <job>1</job>
    </emp>

    <emp>
        <name>青翼蝠王</name>
        <age>45</age>
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/3.jpg</image>
        <gender>1</gender>
        <job>2</job>
    </emp>

    <emp>
        <name>紫衫龙王</name>
        <age>38</age>
        <image>https://web-framework.oss-cn-hangzhou.aliyuncs.com/web/4.jpg</image>
        <gender>2</gender>
        <job>3</job>
    </emp>
</emps>
```

**XmlParserUtils.java**

```java
package com.qqzj.POJO.EMP;

import org.dom4j.Document;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;
import java.io.File;
import java.lang.reflect.Constructor;
import java.util.ArrayList;
import java.util.List;

public class XmlParserUtils {

    public static <T> List<T> parse(String file , Class<T> targetClass)  {
        ArrayList<T> list = new ArrayList<T>(); //封装解析出来的数据
        try {
            //1.获取一个解析器对象
            SAXReader saxReader = new SAXReader();
            //2.利用解析器把xml文件加载到内存中,并返回一个文档对象
            Document document = saxReader.read(new File(file));
            //3.获取到根标签
            Element rootElement = document.getRootElement();
            //4.通过根标签来获取 user 标签
            List<Element> elements = rootElement.elements("emp");

            //5.遍历集合,得到每一个 user 标签
            for (Element element : elements) {
                //获取 name 属性
                String name = element.element("name").getText();
                //获取 age 属性
                String age = element.element("age").getText();
                //获取 image 属性
                String image = element.element("image").getText();
                //获取 gender 属性
                String gender = element.element("gender").getText();
                //获取 job 属性
                String job = element.element("job").getText();

                //组装数据
                Constructor<T> constructor = targetClass.getDeclaredConstructor(String.class, Integer.class, String.class, String.class, String.class);
                constructor.setAccessible(true);
                T object = constructor.newInstance(name, Integer.parseInt(age), image, gender, job);

                list.add(object);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        return list;
    }

}
```

**Emp.java:**

```java
package com.qqzj.POJO.EMP;

public class Emp {
    private String name;
    private Integer age;
    private String image;
    private String gender;
    private String job;

    public Emp() {
    }

    public Emp(String name, Integer age, String image, String gender, String job) {
        this.name = name;
        this.age = age;
        this.image = image;
        this.gender = gender;
        this.job = job;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getImage() {
        return image;
    }

    public void setImage(String image) {
        this.image = image;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getJob() {
        return job;
    }

    public void setJob(String job) {
        this.job = job;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", image='" + image + '\'' +
                ", gender='" + gender + '\'' +
                ", job='" + job + '\'' +
                '}';
    }
}
```

## 分层解耦

### 三层架构

![image-20240607155443775](./图片/image-20240607155443775.png)

- controller：控制层，接收前端发送的请求，对请求进行处理，并响应数据。
- service：业务逻辑层，处理具体的业务逻辑
- dao：数据访问层（Data Access Object）（持久层），负责数据访问操作，包括数据的增删改查

改写上一个案例：

Dao层：

Dao接口：

```java
package com.qqzj.dao;

import com.qqzj.POJO.EMP.Emp;

import java.util.List;

public interface EmpDao {
    //获取员工列表数据
    public List<Emp> listEmp();
}

```

Dao实现类：

```java
package com.qqzj.dao.impl;

import com.qqzj.POJO.EMP.Emp;
import com.qqzj.POJO.EMP.XmlParserUtils;

import java.util.List;

public class EmpDao implements com.qqzj.dao.EmpDao {
    @Override
    public List<Emp> listEmp() {
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        return (List<Emp>) XmlParserUtils.parse(file, Emp.class);
    }
}
```

Service层：

接口：

```java
package com.qqzj.service;

import com.qqzj.POJO.EMP.Emp;

import java.util.List;

public interface EmpService {
    public List<Emp> listEmp();
}
```

实现类：

```java
package com.qqzj.service.impl;

import com.qqzj.POJO.EMP.Emp;
import com.qqzj.dao.impl.EmpDao;

import java.util.List;

public class EmpService implements com.qqzj.service.EmpService {
    private EmpDao empDao = new EmpDao();

    @Override
    public List<Emp> listEmp() {
        List<Emp> emps = empDao.listEmp();
        //遍历集合修改性别和工作的数据
        for (Emp emp : emps) {
            String gender = emp.getGender();
            if ("1".equals(gender)) {
                gender = "男";
            } else {
                gender = "女";
            }
            String job = emp.getJob();
            if ("1".equals(job)) {
                job = "讲师";
            } else if ("2".equals(job)) {
                job = "班主任";
            } else {
                job = "就业指导";
            }
            emp.setGender(gender);
            emp.setJob(job);
        }
        return emps;
    }
}
```

Controller层

```java
package com.qqzj.controller;

import com.qqzj.POJO.Result;
import com.qqzj.service.impl.EmpService;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class empController {
    //接收请求
    @RequestMapping("emp.html/listEmp")
    public Result list(){
        //响应数据
        return Result.success(new EmpService().listEmp());
    }
}
```

![image-20240607162050483](./图片/image-20240607162050483.png)

### 分层解耦

- 内聚：软件中各个功能模块内部的功能联系
- 耦合：衡量软件中各个层/模块之间的依赖、关联的程度
- 软件设计原则：高内聚低耦合

![image-20240607162933826](./图片/image-20240607162933826.png)

**控制反转：**Inversion Of Control，简称<font color='red'>IOC</font>。对象的创建控制权由程序自身转移到外部（容器），这种思想称为控制反转。

**依赖注入：**Dependency Injection，简称<font color='red'>DI</font>。容器为应用程序提供运行时，所依赖的资源，称之为依赖注入

**Bean对象：**IOC容器中创建、管理的对象，称之为<font color='red'>bean</font>

### IOC&DI入门

1. Service层及Dao层的实现类，交给IOC容器管理

   ```java
   //EmpService实现类
   @Component//将当前类交给IOC容器管理，成为IOC容器中的bean
   public class EmpService implements com.qqzj.service.EmpService
   
   //EmpDao实现类
   @Component//将当前类交给IOC容器管理，成为IOC容器中的bean
   public class EmpDao implements com.qqzj.dao.EmpDao 
   ```

2. 为Controller及Service注入运行时，依赖的对象。

   ```java
   //请求接收类
   @RestController
   public class empController {
   
       @Autowired//代表程序在运行时，IOC容器会提供该类型的bean对象，并赋值给该变量 - 依赖注入
       private EmpService empService;
       
   //EmpService实现类    
   @Component//将当前类交给IOC容器管理，成为IOC容器中的bean
   public class EmpService implements com.qqzj.service.EmpService {
       @Autowired//代表程序在运行时，IOC容器会提供该类型的bean对象，并赋值给该变量 - 依赖注入
       private EmpDao empDao;
   ```

3. 运行测试

### IOC详解

#### Bean的声明

要把某个对象交给IOC容器管理，需要在对应的类上加上如下注解之一：

![image-20240607164753715](./图片/image-20240607164753715.png)

<font color='red'>注意事项：</font>

- 声明`bean`的时候，可以通过`value`属性指定`bean`的名字，如果没有指定，默认为类名首字母小写。
- 使用以上四个注解都可以声明bean，但是在springboot集成web开发中，声明控制器bean只能用`@Controller`。

#### bean组件扫描

- 前面声明bean的四大注解，要想生效，还需要被组件扫描注解`@ComponentScan`扫描
- `@ComponentScan`注解虽然没有显式配置，但是实际上已经包含在了启动类声明注解`@SpringBootApplication`中，默认扫描的范围时启动类所在包及其子包

### DI详解

#### Bean注入

- `@Autowired`注解，默认是按照**<font color='red'>类型</font>**进行，如果存在多个相同类型的bean，将会报出如下错误：

  ![image-20240607171007877](./图片/image-20240607171007877.png)

- 通过以下几种方案来解决：

  -  `@Primary`

    ![image-20240607171404160](./图片/image-20240607171404160.png)

  - `@Qualifier`

    ![image-20240607171425360](./图片/image-20240607171425360.png)

  - `@Resource`

    ![image-20240607171449828](./图片/image-20240607171449828.png)

#### 小结

1. 依赖注入的注解
   1. `@Autowired`：默认按照类型自动装配
   2. 如果同类型的bean存在多个：
      - `@Primary`
      - `@Autowired + @Qualifier("bean的名称")`
      - `@Resource(name="bean的名称")`

2. `@Resoutse`与`@Autowired`区别
   - `@Autowired`是spring框架提供的注解，而`@Resource`是JDK提供的注解
   - `@Autowired`默认是按照类型注入，而`@Resource`默认是按照名称注入

# Day06-MySQL

## 什么是数据库？

- 数据库：DataBase(DB)，是存储和管理数据的仓库
- 数据库管理系统：DataBase Management System(DBMS)。操纵和管理数据库的大型软件。
- SQL：Structured Query Language，操作关系型数据库的编程语言，定义了一套操作关系型数据库唯一<font color='red'>标准</font>。

## 数据库产品

![image-20240607172649020](./图片/image-20240607172649020.png)

![image-20240607172958294](./图片/image-20240607172958294.png)

## 概述

### MySQL连接

- 语法：

  ```cmd
  mysql -u用户名 -p密码 [-h数据库服务器IP地址(默认127.0.0.1) -P端口号(默认3306)]
  ```

## 数据模型

- 关系型数据库（RDBMS）：建立在关系模型基础上，由多张相互连接的<font color='red'>二维表</font>组成的数据库

- 特点：
  - 使用表存储数据，格式统一，便于维护
  - 使用SQL语言操作，标准统一，使用方便，可用于复杂查询

```cmd
创建数据库
create database db01;
```

## SQL简介

- SQL：一门操作关系型数据库的编程语言，定义操作所有关系型数据库的<font color='red'>唯一标准</font>

### 通用语法

- SQL语句可以单行或多行书写，以分号结尾。

  ```sql
  show databases;
  show
  	databases;
  ```

- SQL语句可以使用空格/缩进来增强语句的可读性

  ```sql
  show databases;
  	show databases;
  ```

- MySQL数据库的SQL语句不区分大小写

  ```SQL
  SHOW DATABASES;
  ```

- 注释：

  - 单行注释：--注释内容 或 #注释内容（MySQL特有）

    ```sql
    -- show databases;
    ```

    ```mysql
    # show databases;
    ```

  - 多行注释：/\*注释内容\*/

    ```sql
    /* show databases */
    ```

### SQL分类

SQL语句通常被分为四大类：

![image-20240607183315465](./图片/image-20240607183315465.png)

![image-20240607183800324](./图片/image-20240607183800324.png)

## 数据库设计-DDL

DDL英文全称是Data Definition Language，数据定义语言，用来定义数据库对象（数据库、表）

### 数据库操作

- 查询

  - 查询所有数据库：

    ```sql
    show databases;
    ```

  - 查询当前数据库：

    ```sql
    select database();
    ```

- 使用

  - 使用数据库：

    ```sql
    use 数据库名;
    ```

- 创建

  - 创建数据库：

    ```sql
    create database[if not exists] 数据库名;
    ```

- 删除

  - 删除数据库：

    ```sql
    drop database[if exists] 数据库名;
    ```

  ```sql
  mysql> creat database db01 ;
  ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'creat database db01' at line 1
  mysql> create database db01;
  Query OK, 1 row affected (0.01 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | db01               |
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  5 rows in set (0.02 sec)
  
  mysql> create database[if not exists] db01;
  ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '[if not exists] db01' at line 1
  mysql> create database db02;
  Query OK, 1 row affected (0.01 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | db01               |
  | db02               |
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  6 rows in set (0.00 sec)
  
  mysql> create database if not exists db02;
  Query OK, 1 row affected, 1 warning (0.00 sec)
  
  mysql> create database if not exists db03;
  Query OK, 1 row affected (0.01 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | db01               |
  | db02               |
  | db03               |
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  7 rows in set (0.00 sec)
  
  mysql> use db01;
  Database changed
  mysql> use db02;
  Database changed
  mysql> select database();
  +------------+
  | database() |
  +------------+
  | db02       |
  +------------+
  1 row in set (0.00 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | db01               |
  | db02               |
  | db03               |
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  7 rows in set (0.00 sec)
  
  mysql> drop database db03;
  Query OK, 0 rows affected (0.02 sec)
  
  mysql> drop database db03;
  ERROR 1008 (HY000): Can't drop database 'db03'; database doesn't exist
  mysql> drop database if exists db03;
  Query OK, 0 rows affected, 1 warning (0.01 sec)
  
  mysql> drop database if exists db02;
  Query OK, 0 rows affected (0.01 sec)
  
  mysql> show databases;
  +--------------------+
  | Database           |
  +--------------------+
  | db01               |
  | information_schema |
  | mysql              |
  | performance_schema |
  | sys                |
  +--------------------+
  5 rows in set (0.00 sec)
  ```

- 上述语法中的database，也可以替换成schema。如：

  ```sql
  create schema db01;
  ```

### 表结构操作

#### 创建

```sql
create table 表名(
	字段1 字段类型 [约束] [comment 字段1注释],
	...
	字段n 字段类型 [约束] [comment 字段n注释]
)[comment 表注释];
```

```mysql
create table tb_user(
    id int comment 'ID 唯一标识',
    userName varchar(20) comment '用户名',
    name varchar(10) comment '姓名',
    age int comment '年龄',
    gender char(1) comment '性别'
)comment '用户表';
```

#### 约束

- 概念：约束是作用于表中字段上的规则，用于限制存储在表中的数据。
- 目的：保证数据库中数据的正确性、有效性和完整性

![image-20240607230026905](./图片/image-20240607230026905.png)

#### 数据类型

- 数据类型

  MySQL中的数据类型有很多，主要分为三类：数值类型、字符串类型、日期时间类型

  参考：D:\AAAStudy\02-Javaweb\MySQL数据类型.xlsx

#### 创建-案例

根据页面原型/需求创建表（设计合理的数据类型、长度、约束）

设计员工管理模块的表结构（不考虑所属部门字段）

```MySQL
create table tb_emp
(
    id          int primary key auto_increment comment '员工ID，唯一标识',
    userName    varchar(20)                  null comment '用户名',
    password    varchar(32) default '123456' null comment '密码',
    name        varchar(10)                  not null comment '姓名',
    gender      tinyint unsigned             not null comment '性别  1：男  2：女',
    image       varchar(300)                 null comment '图片url',
    job         tinyint unsigned             null comment '职位  1：班主任  2：讲师  3：学工主管  4：教师主管',
    entryData   date                         null comment '入职日期',
    create_date datetime                     not null comment '创建时间',
    update_time datetime                     not null comment '修改时间',
    constraint userName
        unique (userName)
)
    comment '员工表';
```

![image-20240608000709501](./图片/image-20240608000709501.png)

**<font color='red'>注意事项</font>**

- create_time：记录的是当前这条数据插入的时间。
- update_time：记录当前这条数据最后更新的时间

#### 查询、修改、删除

##### 查询

- 查询当前数据库所有表：

  ```sql
  show tables;
  ```

- 查询表结构：

  ```sql
  desc 表名;
  ```

- 查询建表语句：

  ```sql
  show create table 表名;
  ```

  ```mysql
  -- DDL:查看表结构
  -- 查看：当前数据库下的表
  show tables;
  
  -- 查看：指定表结构
  desc tb_emp;
  
  -- 查看：数据库的建表语句
  show create table tb_emp;
  ```

##### 修改

- 添加字段：

  ```sql
  alter table 表名 add 字段名 类型(长度) [comment 注释] [约束];
  ```

- 修改字段类型：

  ```sql
  alter table 表名 modify 字段名 新数据类型(长度);
  ```

- 修改字段名和字段类型：

  ```sql
  alter table 表名 change 旧字段名 新字段名 类型(长度) [comment 注释] [约束];
  ```

- 删除字段：

  ```sql
  alter table 表名 drop column 字段名;
  ```

- 修改表名：

  ```sql
  rename table 表名 to 新表名;
  ```

##### 删除

- 删除表：

  ```sql
  drop table [if exists] 表名;
  ```

<font color='red'>注意事项</font>

- 在删除表时，表中的全部数据也会被删除。

## DML

DML英文全称是Data Manipulation Language(数据操作语言)，用来对数据库中表的数据记录进行增、删、改操作

- 添加数据（<font color='red'>INSERT</font>）
- 修改数据（<font color='red'>UPDATE</font>）
- 删除数据（<font color='red'>DELETE</font>）

### 添加数据insert

- 指定字段添加数据：

  ```sql
  insert into 表名(字段名1,字段名2) values(值1,值2);
  ```

- 全部字段添加数据：

  ```sql
  insert into 表名 values(值1,值2,...);
  ```

- 批量添加数据（指定字段）：

  ```sql
  insert into 表名(字段名1，字段名2) values(值1,值2),(值1,值2);
  ```

- 批量添加数据（全部字段）：

  ```sql
  insert into 表名 values(值1,值2,...),(值1,值2,...),...;
  ```

  ```mysql
  -- DML：数据操作语言
  -- DML：插入数据 -insert
  -- 1. 为tb_emp 表的username,name,gender 字段插入值
  insert into tb_emp(userName, name, gender, create_date, update_time) value ('111', '张三', 1, '2020-02-02', '2020-02-02');
  -- 2. 为 tb_emp 表的所有字段插入值
  insert into tb_emp value (2, '222', '123456', '李四', 2, 'ad', 3, '2020-08-08', '2020-03-02', '2020-05-02');
  -- 3.批量为tb_emp 表的username.name,gender 字段插入值
  insert into tb_emp(userName, name, gender, create_date, update_time)
  values ('333', '张三', 1, '2020-02-02', '2020-02-02'),
         ('444', '张三', 1, '2020-02-02', '2020-02-02');
  ```

<font color='red'>注意事项</font>

- 插入数据时，指定的字段顺序需要与值的顺序一一对应
- 字符串和日期型数据应该包含在引号中
- 插入的数据大小，应该在字段的规定范围内

### 更新数据UPDATE

- 修改数据：

- ```sql
  update 表名 set 字段名1=值1,字段名2=值2,...[where 条件];
  ```

<font color='red'>修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据</font>

### 删除数据DELETE

- 删除数据：

- ```sql
  delete from 表名 [where 条件];
  ```

<font color='red'>注意事项</font>

1. DELETE语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据。
2. DELETE语句不能删除某一个字段的值（如果要操作，可以使用UPDATE，将该字段的值改为null）。

# Day07-查询&多表设计

## 查询DQL

- DQL英文全称是Data Query Language（数据查询语言），用来查询数据库表中的记录
- 关键字：<font color='red'>SELECT</font>

语法：

```sql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数
```

![image-20240608012259585](./图片/image-20240608012259585.png)

### 基本查询

语法：

- 查询多个字段：

- ```sql
  select 字段1,字段2,字段3 from 表名;
  ```

- 查询所有字段（通配符）：

- ```sql
  select * from 表名;
  ```

- 设置别名：

- ```sql
  select 字段1 [as 别名1], 字段2[as 别名2] from 表名;
  ```

- 去除重复记录：

- ```sql
  select distinct 字段列表 from 表名;
  ```

  ```mysql
  -- ====DQL：基本查询====
  -- 1.查询指定字段 name,entrydate 并返回
  select name,entrydate from tb_emp;
  
  -- 2.查询返回所有字段
  -- 推荐
  select id, username, password, name, gender, image, job, entrydate, create_time, update_time from tb_emp;
  -- 不推荐 (不直观,性能低)
  select * from tb_emp;
  
  -- 3.查询所有员工的name,entrydate,并起别名（姓名，入职日期）
  select name as '姓名',entrydate as '入职日期' from tb_emp;
  
  -- 4.查询已有的员工关联了哪几种职位（不要重复）
  select distinct job from tb_emp;
  ```

<font color='red'>\*号代表查询所有字段,在实际开发中尽量少用(**不直观、影响效率**)</font>

### 条件查询

- 条件查询:

- ```sql
  select 字段列表 from 表名 where 条件列表;
  ```

  ![image-20240608014820122](./图片/image-20240608014820122.png)

```mysql
-- 1.查询 姓名 为 杨逍 的员工
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where name = '杨逍';
-- 2.查询 id小于等于5 的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where id <= 5;
-- 3.查询 没有分配职位 的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where job is null;
-- 4.查询 有职位 的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where job is not null;
-- 5.查询 密码不等于"123456" 的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where password <> '123456';
-- 6.查询 入职日期 在'2000-01-01'(包含) 到 '2010-01-01'(包含) 之间的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where entrydate between '2000-01-01' and '2010-01-01';
-- 7.查询 入职时间 在'2000-01-01'(包含) 到 '2010-01-01'(包含) 之间 且 性别为女 的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where entrydate between '2000-01-01' and '2010-01-01' and gender=2;
-- 8.查询 职位是2(讲师),3(学工主管),4(校验主管) 的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where job in(2,3,4);
-- 9.查询 姓名 为两个字的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where name like '__';
-- 10.查询 姓"张"的员工信息
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where name like '张%';
```

### 分组查询

#### 聚合函数

- 介绍:将一列数据作为一个整体,进行纵向计算.

- 语法:

- ```sql
  select 聚合函数(字段列表) from 表名;
  ```

  ![image-20240608112840730](./图片/image-20240608112840730.png)

```mysql
-- 聚合函数
-- 聚合函数:不对null值进行运算
-- 1.统计该企业员工数量
-- A.count(字段)
select count(name)
from tb_emp;
-- B.count(常量)
select count(0/*任意不为null的常量*/)
from tb_emp;
-- C.count(*) 推荐使用
select count(*)
from tb_emp;
-- 2.统计该企业最早入职的员工
select min(entrydate)
from tb_emp;
-- 3.统计该企业最晚入职的员工
select max(entrydate)
from tb_emp;
-- 4.统计该企业员工ID的平均值
select avg(id)
from tb_emp;
-- 5.统计该企业员工的ID之和
select sum(id)
from tb_emp;
```

<font color='red'>注意事项:</font>

- null值不参与所有聚合函数运算
- 统计数量可以使用:count(\*)   count(字段)   count(常量), 推荐使用count(*).

#### 分组查询

语法:

```sql
select 字段列表 from 表名 [where 条件] group by 分组字段名 [having 分组后过滤条件];
```

```mysql
-- 分组
-- 1. 根据性别分组,统计男性和女性员工的数量
select gender, count(*)
from tb_emp
group by gender;
-- 2.先查询入职时间在 '2015-01-01'(包含) 以前的员工,并对结果根据职位分组,获取员工数量大于等于2的职位
select job, count(*),max(entrydate)
from tb_emp
where entrydate<='2015-01-01'
group by job
having count(*)>=2;
```

- where与having的区别
  1. 执行时机不同:where是分组之前进行过滤,不满足where条件,不参与分组;而having是分组之后对结果进行过滤.
  2. 判断条件不同:where不能对聚合函数进行判断,而having可以

<font color='red'>注意事项:</font>

- 分组之后,查询的字段一般为聚合函数和分组字段,查询其他字段无任何意义
- 执行顺序:where>聚合函数>having

### 排序查询

语法:

```
select 字段列表 from 表名 [where 条件列表] [group by 分组字段] order by 字段1 排序方式1,字段2 排序方式2...;
```

排序方式:

- ASC: 升序(默认值)
- DESC: 降序

```mysql
-- 排序查询
-- 1. 根据入职时间,对员工进行升序排序
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
order by entrydate;
-- 2.根据入职时间,对员工进行降序排序
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
order by entrydate desc ;
-- 3.根据 入职时间 对公司的员工进行 升序排序, 入职时间相同,再按照 更新时间 进行降序排序
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
order by entrydate,update_time desc ;
```

<font color='red'>注意事项:</font>

- 如果是多字段排序,当第一个字段值相同时,才会根据第二个字段进行排序.

### 分页查询

语法:

```sql
select 字段列表 from 表名 limit 起始索引,查询记录数;
```

```mysql
-- 分页查询
-- 1.从 起始索引0 开始查询员工数据,每页展示5条记录
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
limit 0, 5;
-- 2.查询 第一页 员工数据,每一页展示5条记录
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
limit 0, 5;
-- 3.查询 第2页 员工数据,每页展示5条数据
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
limit 5, 5;
-- 4.查询 第三页 员工数据,每页展示5条数据
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
limit 10, 5;
```

<font color='red'>注意事项:</font>

1. 起始索引从0开始, `起始索引 = (查询页码 - 1) \* 每页展示记录数`.
2. 分页查询i数据库的方言,不同的数据库有不同的实现, MySQL中是`LIMIT`
3. 如果查询的是第一页的数据,起始索引可以省略,直接简写为`limit 10`;

### 案例

根据需求完成员工管理的条件分页查询

```mysql
-- 案例1: 根据输入条件,查询第一页数据,每页展示10条数据
-- 输入条件
-- 姓名: 张
-- 性别: 男
-- 入职时间: 2000-01-01 ~ 2015-12-31
select id,
       username,
       password,
       name,
       gender,
       image,
       job,
       entrydate,
       create_time,
       update_time
from tb_emp
where name like '%张%'
  and gender = 1
  and (entrydate between '2000-01-01' and '2015-12-31')
order by update_time desc
limit 10;
```

根据需求, 完成员工信息的统计

![image-20240608125254462](./图片/image-20240608125254462.png)

```mysql
-- 案例2-1: 根据需求,完成员工新别信息的统计
-- 流程控制函数 if(条件表达式, true取值, false取值)
select if(gender = 1, '男性员工', '女性员工') 性别, count(gender)
from tb_emp
group by gender;

-- 案例2-2: 根据需求,完成员工职位信息的统计
-- case 表达式 when 值1 then 结果1 when 值2 then 结果2...else...end
select (case job
    when 1 then '班主任'
    when 2 then '讲师'
    when 3 then '学工主管'
    when 4 then '教研主管'
    else '无职位' end) 职位,
       count(*)
from tb_emp
group by job;
```

函数:

- if(表达式, tvalue, fvalue); 当表达式为true时, 取tvalue, 否则,取fvalue
- <font color='red'>case </font>expr <font color='red'>when </font>value1 <font color='red'>then </font>result1 [<font color='red'>when </font>value2 <font color='red'>then </font>result2...] [<font color='red'>else </font>result] <font color='red'>end</font>;

## 多表设计

项目开发中,在进行数据库表结构设计时,会根据业务需求及业务模块之间的关系,分析并设计表结构,由于业务之间相互关联,所以各个表结构之间也存在着各种联系,基本上分为三种:

- 一对多(多对一)
- 多对多
- 一对一

### 一对多

- 根据页面原型及需求文档,完成部门及员工模块的表结构设计

```mysql
-- 员工管理(带约束)
create table tb_emp
(
    id          int unsigned primary key auto_increment comment 'ID',
    username    varchar(20)      not null unique comment '用户名',
    password    varchar(32) default '123456' comment '密码',
    name        varchar(10)      not null comment '姓名',
    gender      tinyint unsigned not null comment '性别, 说明: 1 男, 2 女',
    image       varchar(300) comment '图像',
    job         tinyint unsigned comment '职位, 说明: 1 班主任,2 讲师, 3 学工主管, 4 教研主管',
    entrydate   date comment '入职时间',
    dept_id     int unsigned comment '归属部门id',
    create_time datetime         not null comment '创建时间',
    update_time datetime         not null comment '修改时间'
) comment '员工表';

create table tb_dept
(
    id          int unsigned primary key comment '部门ID',
    deptName    varchar(10) not null unique comment '部门名称',
    creat_time  datetime    not null comment '创建时间',
    update_time datetime    not null comment '上次更新时间'
) comment '部门表';
```

一对多关系实现: 在数据库表中多的一方, 添加字段, 来关联一的一方的主键

![image-20240608133835971](./图片/image-20240608133835971.png)

### 外键约束

外键语法:

```sql
-- 创建表时指定
create table 表名(
	字段名 数据类型.
    ...
    [constraint] [外键名称] foreign key(外键字段名) references 主表(字段名)
);

-- 建完表后,添加外键
alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(字段名);
```

物理外键:

- 概念: 使用foreign key定义外键关联另外一张表
- 缺点:
  - 影响增,删,改的效率(需要检查外键关系)
  - 仅用于单节点数据库, 不适用于分布式、集群场景
  - 容易引发数据库的死锁问题, 消耗性能.

<font color='red'>逻辑外键(推荐):</font>

- 概念: 在业务逻辑中, 解决外键关联.
- 通过逻辑外键, 就可以很方便的解决上述问题

### 一对一

- 案例:用户 与 身份证信息 的关系
- 关系: 一对一关系, 多用于单表拆分, 将一张表的基础字段放到一张表中, 其他字段放在另一张表中, 以提升操作效率

- 实现:<font color='red'>在任意一方加入外键.关联另外一方的主键,并且设置外键为唯一的(unique)</font>

![image-20240608140438727](./图片/image-20240608140438727.png)

### 多对多

- 案例:学生 与 课程的关系
- 关系: 一个学生可以选修多门课程, 一门课程也可以供多个学生选择

- 实现:<font color='red'>建立第三张中间表, 中间表至少包含两个外键, 分别关联两方主键</font>

![image-20240608140836053](./图片/image-20240608140836053.png)

### 案例-关系分析

参考页面原型及需求,设计合理的表结构

- 参考苍穹外卖管理后台页面原型,设计<font color='red'>分类管理、菜品管理、套餐管理</font>模块的表结构

步骤:

- 阅读页面原型及需求文档, 分析各个模块涉及到的表结构, 及表结构之间的关系
- 根据页面原型及需求文档, 分析各个表结构中具体的字段及约束

# Day08-多表查询&事务&索引&Mybatis

- 多表查询: 指从多张表中查询数据

![image-20240608165039510](./图片/image-20240608165039510.png)

分类:

- 连接查询
  - 内连接: 相当于查询A、B、交集部分数据
  - 外连接:
    - 左外连接: 查询<font color='red'>左表</font>所有数据(包括两张表交集部分数据)
    - 右外连接: 查询<font color='red'>右表</font>所有数据(包括两张表交集部分数据)
- 子查询

## 内连接

语法:

- 隐式内连接:

  ```sql
  select 字段列表 from 表1,表2 where 条件...;
  ```

- 显式内连接:

  ```sql
  select 字段列表 from 表1 [inner] join 表2 on 连接条件...;
  ```

  ```mysql
  -- 内连接
  -- A.查询员工的姓名, 及所属的部门名称(隐式内连接实现)
  select tb_emp.name, tb_dept.name
  from tb_emp,
       tb_dept
  where tb_emp.dept_id = tb_dept.id;
  
  -- 起别名
  select *
  from tb_emp e,
       tb_dept d
  where e.dept_id = d.id;
  
  -- B.查询员工的姓名, 及所属的部门名称(显式内连接实现)
  select tb_emp.name, tb_dept.name
  from tb_emp
           inner join tb_dept
                      on tb_emp.dept_id = tb_dept.id;
  ```

## 外连接

语法:

- 左外连接:

  ```sql
  select 字段列表 from 表1 left [outer] join 表2 on 连接条件...;
  ```

- 右外连接:

  ```sql
  select 字段列表 from 表1 right [outer] join 表2 on 连接条件...;
  ```

  ```mysql
  -- 外连接
  -- A.查询员工表所有 员工的姓名,和相对应的部门名称(左外连接)
  select tb_emp.name, tb_dept.name
  from tb_emp
           left outer join tb_dept on tb_emp.dept_id = tb_dept.id;
  -- B.查询部门表所有 部门的名称,和相应的员工名称(右外连接)
  select tb_dept.name, tb_emp.name
  from tb_emp
           right join tb_dept  on tb_emp.dept_id = tb_dept.id;
  ```

## 子查询

- 介绍:SQL语句中嵌套select语句,称为嵌套查询,又称子查询

- 形式:

  ```sql
  select * from t1 where column1 = (select column1 from t2...);
  ```

- 子查询外部的雨具可以是insert/update/delete/select的任何一个,最常见的是select.

分类:

- 标量子查询: 子查询返回的结果为单个值
- 列子查询: 子查询返回的结果为一列
- 行子查询: 子查询返回的结果为一行
- 表子查询: 子查询返回的结果为多行多列

### 标量子查询

- 子查询返回的结果是单个值(数字、字符串、日期等),最简单的形式
- 常用的操作符: =| <>| >| >= |< |<=

```mysql
-- 子查询
-- 标量子查询
-- A.查询"教研部"的所有员工信息
select *
from tb_emp
where dept_id = (select id from tb_dept where name = '教研部');
-- B.查询在'方东白'入职之后的员工信息
select *
from tb_emp
where entrydate > (select entrydate from tb_emp where name = '方东白');
```

### 列子查询

- 子查询返回的结果是一列(可以是多行)
- 常用的操作符: in、not in等

```mysql
-- 列子查询
-- A.查询"教研部"和"咨询部"的所有员工信息
select *
from tb_emp
where dept_id in (select id from tb_dept where name in ('教研部', '咨询部'))
```

### 行子查询

- 子查询返回的结果是一行(可以是多列)
- 常用的操作符:=、<>、in、not in

```mysql
-- 行子查询
-- A.查询与"韦一笑"的入职日期及职位都相同的员工信息
select entrydate
from tb_emp
where name = '韦一笑';
select job
from tb_emp
where name = '韦一笑';
select *
from tb_emp
where entrydate = (select entrydate from tb_emp where name = '韦一笑')
  and job = (select job from tb_emp where name = '韦一笑');
-- 改进
select *
from tb_emp
where (entrydate, job) in (select entrydate, job from tb_emp where name = '韦一笑');
```

### 表子查询

- 子查询返回的结果是多行多列, 常作为临时表
- 常用的操作符: in

```mysql
-- 表子查询
-- A.查询入职日期是"2006-01-01" 之后的员工信息, 及其部门名称
select temp.*, tb_dept.name
from (select * from tb_emp where entrydate > '2006-01-01') temp,
     tb_dept where temp.dept_id = tb_dept.id;
```

### 案例

根据需求,完成多表查询的SQL语句编写

- 将资料中准备好的多表查询的数据准备的SQL脚本导入到数据库中

```mysql
-- 1. 查询价格低于 10元 的菜品的名称 、价格 及其 菜品的分类名称 .
select dish.name, dish.price, category.name
from dish,
     category
where price < 10
  and dish.category_id = category.id;
-- 2. 查询所有价格在 10元(含)到50元(含)之间 且 状态为'起售'的菜品名称、价格 及其 菜品的分类名称 (即使菜品没有分类 , 也需要将菜品查询出来).
select dish.name, price, category.name
from dish
         left join category on category_id = category.id
where price between 10 and 50
  and dish.status = 1;
-- 3. 查询每个分类下最贵的菜品, 展示出分类的名称、最贵的菜品的价格 .
select category.name, max(dish.price)
from category,
     dish
where category.id = dish.category_id
group by category.name;
-- 4. 查询各个分类下 状态为 '起售' , 并且 该分类下菜品总数量大于等于3 的 分类名称 .
select category.name
from category,
     dish
where category.status = 1
  and category_id = category.id
group by category.name
having count(*) >= 3;
-- 5. 查询出 "商务套餐A" 中包含了哪些菜品 （展示出套餐名称、价格, 包含的菜品名称、价格、份数）.
select setmeal.name, setmeal.price, dish.name, dish.price, setmeal_dish.copies
from dish,
     setmeal_dish,
     setmeal
where dish.id = setmeal_dish.dish_id
  and setmeal.name = '商务套餐A'
  and setmeal_dish.setmeal_id = setmeal.id;
-- 6. 查询出低于菜品平均价格的菜品信息 (展示出菜品名称、菜品价格).
select dish.name, dish.price
from dish
where price < (select avg(price) from dish)
```

## 事务

### 介绍

概念: **<font color='red'>事务</font>**是一组操作的集合, 它是一个不可分割的工作单位. 事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求, 即这些操作<font color='red'>要么同时成功, 要么同时失败</font>.

<font color='red'>注意事项</font>

- 默认MySQL的事务是自动提交的, 也就是说, 当执行一条DML语句, MySQL会立即隐式的提交事务

### 事务控制

- 开启事务: `start transaction; / begin;`
- 提交事务: `commit;`
- 回滚事务: `rollback;`

### 四大特性(ACID)

- **原子性(Atomicity): 事务是不可分割的最小单元, 要么全部成功, 要么全部失败**
- **一致性(Consistency): 事务完成时, 必须使所有的数据都保持一致状态**
- **隔离性(Isolation): 数据库系统提供的隔离机制, 保证事务在不受外部并发操作影响的独立环境下运行**
- **持久性(Durability): 事务一旦提交或回滚, 它对数据库中的数据的改变就是永久的**

## 索引

### 介绍

概念:

**<font color='red'>索引(index)</font>**是帮助数据库 <font color='red'>高效获取数据</font> 的 <font color='red'>数据结构</font> .

![image-20240609004354904](./图片/image-20240609004354904.png)

优点:

- 提高数据查询的效率, 降低数据库的IO成本
- 通过索引列对数据进行排序, 降低数据排序的成本, 降低CPU消耗

缺点:

- 索引会占用存储空间.
- 索引大大提高了查询效率, 同时却也降低了insert、update、delete的效率.

### 结构

MySQL数据库支持的索引结构有很多, 如: Hash索引、B+Tree索引、Full-Text索引等. 我们平时所说的索引, 如果没有特别指明, 都是指默认的<font color='red'>B+Tree</font>结构组织的索引

- B+Tree(多路平衡搜索树)

![image-20240609005351394](./图片/image-20240609005351394.png)

- 每一个节点, 可以存储多个key(有n多个key, 就有n个指针).
- 所有的数据都存储在叶子节点, 非叶子节点仅用于索引数据.
- 叶子节点形成了一颗双向链表, 便于数据的排序及区间范围查询

### 语法

- 创建索引:

  ```sql
  create [unique] index 索引名 on 表名(字段名...);
  ```

- 查看索引:

  ```sql
  show index from 表名;
  ```

- 删除索引:

  ```sql
  drop index 索引名 on 表名;
  ```

```mysql
-- 创建索引
create index index_dish on dish(name);
-- 查询索引
show index from dish;
-- 删除索引
drop index index_dish on dish;
```

<font color='red'>注意事项</font>

- 主键字段, 在建表时, 会自动创建主键索引,
- 添加唯一约束时, 数据库实际上会添加唯一索引.

## Mybatis入门

- Mybatis是一款优秀的<font color='red'>持久层</font>框架, 用于简化JDBC的开发

![image-20240609011255107](./图片/image-20240609011255107.png)

- Mybatis本是Apache的一个开源项目iBatis, 2010年这个项目由Apache迁移到了Google code, 并且改名为Mybatis. 2013年11月迁移到Github.
- 官网: https://mybatis.org/mybatis-3/zh/index.html

### 快速入门

使用Mybatis查询所有用户数据

```mysql
-- MySQL方式查询所有用户信息
select id, name, age, gender, phone
from user;
```

1. 准备工作(创建SpringBoot工程、数据库表user、实体类User)

   ![image-20240609025913976](./图片/image-20240609025913976.png)

   ```java
   package com.qqzj.pojo;
   
   public class User {
       private Integer id;
       private String name;
       private Short age;
       private Short gender;
       private String phone;
   
       public User() {
       }
   
       public User(Integer id, String name, Short age, Short gender, String phone) {
           this.id = id;
           this.name = name;
           this.age = age;
           this.gender = gender;
           this.phone = phone;
       }
   
       /**
        * 获取
        *
        * @return id
        */
       public Integer getId() {
           return id;
       }
   
       /**
        * 设置
        *
        * @param id
        */
       public void setId(Integer id) {
           this.id = id;
       }
   
       /**
        * 获取
        *
        * @return name
        */
       public String getName() {
           return name;
       }
   
       /**
        * 设置
        *
        * @param name
        */
       public void setName(String name) {
           this.name = name;
       }
   
       /**
        * 获取
        *
        * @return age
        */
       public Short getAge() {
           return age;
       }
   
       /**
        * 设置
        *
        * @param age
        */
       public void setAge(Short age) {
           this.age = age;
       }
   
       /**
        * 获取
        *
        * @return gender
        */
       public Short getGender() {
           return gender;
       }
   
       /**
        * 设置
        *
        * @param gender
        */
       public void setGender(Short gender) {
           this.gender = gender;
       }
   
       /**
        * 获取
        *
        * @return phone
        */
       public String getPhone() {
           return phone;
       }
   
       /**
        * 设置
        *
        * @param phone
        */
       public void setPhone(String phone) {
           this.phone = phone;
       }
   
       public String toString() {
           return "User{id = " + id + ", name = " + name + ", age = " + age + ", gender = " + gender + ", phone = " + phone + "}";
       }
   }
   
   ```

2. 引入Mybatis的相关依赖, 配置Mybatis(数据库连接信息)

   ![image-20240609025932793](./图片/image-20240609025932793.png)

   ```
   application.properties中
   #配置数据库连接信息
   spring.application.name=Project01
   #驱动类名称
   spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
   #数据库连接的url
   spring.datasource.url=jdbc:mysql://localhost:3306/db07
   #连接数据库的用户名
   spring.datasource.username=root
   #连接数据库的密码
   spring.datasource.password=1234
   ```

3. 编写SQL语句(注解/XML)

   ```java
   UserMapper接口中
   @Mapper//在运行时, 会自动生成该接口的实现类对象(代理对象), 并且将该对象交给IOC容器管理
   public interface UserMapper {
       //查询全部用户信息
       @Select("select * from user")
       public List<User> list();
   }
   ```

![image-20240609013046051](./图片/image-20240609013046051.png)

4. 单元测试

   ```java
   @SpringBootTest//springboot整合单元测试的注解
   class Project01ApplicationTests {
       @Autowired
       private UserMapper userMapper;
   
       @Test
       public void testListUser(){
           List<User> list = userMapper.list();
           list.stream().forEach(System.out::println);
       }
   
   }
   ```

### 配置SQL提示

- 默认在mybatis中编写SQL语句是不识别的,可以做如下配置:

  ![image-20240609030402552](./图片/image-20240609030402552.png)

### JDBC介绍

- JDBC: (Java DataBase Connectivity), 就是使用Java语言操作关系型数据库的一套API.

![image-20240609031202498](./图片/image-20240609031202498.png)

本质:

- sun 公司官方定义的一套操作所有关系型数据库的规范, 即接口
- 各个数据库厂商去实现这套接口, 提供数据库<font color='red'>驱动jar包</font>.
- 我们可以使用这套接口(JDBC)编程, 真正执行的代码是驱动jar包中的实现类

![image-20240609035439632](./图片/image-20240609035439632.png)

![image-20240609035631585](./图片/image-20240609035631585.png)

### 数据库连接池

- <font color='red'>数据库连接池</font>是个容器, 负责分配、管理数据库连接(Connection)
- 它允许应用程序重复使用一个现有的数据库连接, 而不是再重新建立一个
- 释放空闲时间超过最大空闲时间的连接, 来避免因为没有释放连接而引起的数据库连接遗漏

优势:

- 资源重用
- 提升系统响应速度
- 避免数据库连接遗漏

标准接口: DataSource

- 官方(sun)提供的数据库连接池接口, 由第三方组织实现此接口

- 功能: 获取连接

  ```java
  Connection getConnection() throws SQLException;
  ```

常见产品

![image-20240609044219972](./图片/image-20240609044219972.png)

- Druid(德鲁伊)
  - Druid连接池是阿里巴巴开源的数据库连接池项目
  - 功能强大, 性能优秀, 是Java语言最好的数据库连接池之一

- 切换Druid数据库连接池

  官方地址: https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter

  ```xml
  <dependency>
  	<groupId>com.alibaba</groupId>
  	<artifactId>druid-spring-boot-starter</artifactId>
  	<version>1.2.8</version>
  </dependency>
  ```

  ```properties
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
  spring.datasource.username=root
  spring.datasource.password=1234
  ```

  ```properties
  spring.datasource.druid.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.druid.url=jdbc:mysql://localhost:3306/mybatis
  spring.datasource.druid.username=root
  spring.datasource.druid.password=1234
  ```


### lombok工具包

- lombok是一个实用的Java类库, 能通过注解的形式自动生成构造器、getter/setter、equals、hashCode、toString等方法, 并可以自动化生成日志变量, 简化Java开发、提高效率

  ![image-20240609052207447](./图片/image-20240609052207447.png)

<font color='red'>注意事项</font>

- Lombok会在编译时, 自动生成对应的Java代码. 我们使用lombok时, 还需要安装一个lombok的插件(IDEA自带)

# Day09-Mybatis基础操作&XML映射文件

需求说明:

完成员工管理的需求开发

功能列表:

- 查询
  - 根据主键ID查询
  - 条件查询
- 新增
- 更新
- 删除
  - 根据主键ID删除
  - 根据主键ID批量删除

### 环境准备

- 准备数据库表emp
- 创建一个新的SpringBoot工程,选择引入对应的起步依赖(Mybatis、MySQL驱动、lombok)
- application.properties中引入数据库连接信息
- 创建对应的实体类Emp(实体类属性采用驼峰命名)
- 准备Mapper接口EmpMapper

### 删除

根据主键删除

- SQL语句

  ```mysql
  delete from emp where id=17;
  ```

- 接口方法:

  ```java
  @Delete("delete from emp where id = #{id}")
  public void delete(Integer id);
  ```

```java
@Mapper
public interface EmpMapper {
    @Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp")
    public List<Emp> list();

    @Delete("delete from emp where id = #{chooseID}")
    //public void delete(Integer chooseID);
    //返回此次操作影响的记录数
    public Integer delete(Integer chooseID);
}

```

**<font color='red'>注意事项</font>**

- 如果mapper接口方法形参只有一个普通类型的参数, #{}里面的属性名可以随便写.

### 预编译SQL

#### 日志输出

- 可以在`application.properties`中, 打开Mybatis的日志, 并指定输出到控制台

  ```properties
  #指定Mybatis输出日志的位置, 输出到控制台
  mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
  ```

![image-20240609115834119](./图片/image-20240609115834119.png)

#### 优势:

- 性能更高
- 更安全(防止SQL注入)

![image-20240609120223885](./图片/image-20240609120223885.png)

#### SQL注入

- **SQL注入**是通过操作输入的数据来修改事先定义好的SQL语句, 已达到执行代码对服务器进行<font color='red'>攻击</font>的方法

#### 参数占位符

`#{...}`

- 执行SQL时, 会将`#{...}`替换为? , 生成预编译SQL, 会自动设置参数值
- 使用时机: 参数传递, 都使用`#{...}`

`${...}`

- 拼接SQL, 直接将参数拼接在SQL语句中, 存在SQL注入问题.
- 使用时机: 如果对表名、列表进行动态设置时使用

### 新增

- SQL语句:

  ```mysql
  insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) value("songyuanqiao","松原桥",2,"1.a",1,'2020-10-09',2,now(),now());
  ```

- 接口方法:

```java
@Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time)" +
            " value(#{userName},#{name},#{gender},#{image},#{job},#{entryDate},#{deptId},#{createTime},#{updateTime})")
    public void insert(Emp emp);
```

测试类

```java
@Test
public void testDelete(){
    Emp emp = new Emp();
    emp.setUserName("zhangsan");
    emp.setName("张三");
    emp.setGender((short) 1);
    emp.setImage("111");
    emp.setJob((short) 1);
    emp.setEntryDate(LocalDate.of(2022,5,14));
    emp.setDeptId(1);
    emp.setCreateTime(LocalDateTime.now());
    emp.setUpdateTime(LocalDateTime.now());
    empMapper.insert(emp);
}
```

#### 主键返回

- 描述: 在数据添加成功后, 需要获取插入数据库数据的主键.

实现:

```java
@Option(keyProperty = "id", useGenerateKeys = true)//会自动将生成的主键值, 赋值给Emp对象的id属性
@Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time)" +
            " value(#{userName},#{name},#{gender},#{image},#{job},#{entryDate},#{deptId},#{createTime},#{updateTime})")
    public void insert(Emp emp);
```

```java
@Options(useGeneratedKeys = true,keyProperty = "id")
@Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time)" +
        " value(#{userName},#{name},#{gender},#{image},#{job},#{entryDate},#{deptId},#{createTime},#{updateTime})")
public void insert(Emp emp);
```

### 更新

- SQL语句(根据ID更新员工信息)

  ```mysql
  update emp
  set username='lisi2',
      name='李四',
      gender=2,
      image='123.3',
      job=1,
      entrydate='2000-12-18',
      dept_id=2,
      update_time=now()
  where id = 19;
  ```

- 接口方法

  ```java
      @Update("update emp set username=#{userName}, name=#{name}, gender=#{gender}, image=#{image},job=#{job}, entrydate=#{entryDate}, dept_id=#{deptId},  update_time=#{updateTime} where id = #{id}")
      public void update(Emp emp);
  ```

### 查询

#### 根据ID查询

- SQL语句

  ```mysql
  select id,
         username,
         password,
         name,
         gender,
         image,
         job,
         entrydate,
         dept_id,
         create_time,
         update_time
  from emp
  where id=19;
  ```

- 接口方法

  ```java
      @Select("select id,  username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where id=#{id};")
      public Emp select(Integer id);
  ```

- 运行结果

  ```java
  Emp(id=19, userName=wangwu, password=123456, name=王五, gender=1, image=111, job=1, entryDate=2022-05-14, deptId=null, createTime=null, updateTime=null)
  ```

#### 数据封装

- 实体类属性名 和 数据库表查询返回的字段名一致, Mybatis会自动封装
- 如果实体类属性名和数据库表查询返回的字段名不一致, 不能自动封装

解决方案:

- **起别名**: 在SQL语句中, 对不一样的列名起别名, 别名和实体类属性名一致

  ```java
   @Select("select id,  username, password, name, gender, image, job, entrydate, dept_id deptID, create_time createTime, update_time updateTime from emp where id=#{id};")
      public Emp select(Integer id);
  ```

- **手动结果映射**:通过`@Results` 和 `@Result` 进行手动结果映射

  ```java
  @Results({
              @Result(column = "dept_id", property = "deptId"),
              @Result(column = "create_time", property = "createTime"),
              @Result(column = "update_time", property = "updateTime")
      })
      @Select("select id,  username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where id=#{id};")
      public Emp select(Integer id);
  ```

- **开启驼峰命名**:如果字段名与属性名符合驼峰命名规则, Mybatis会自动通过驼峰命名规则映射

  ```properties
  #开启Mybatis的驼峰命名自动映射开关 a_column --> aColumn
  mybatis.configuration.map-underscore-to-camel-case=true
  ```

#### 条件查询

- SQL语句

  ```mysql
  select id,
         username,
         password,
         name,
         gender,
         image,
         job,
         entrydate,
         dept_id,
         create_time,
         update_time
  from emp
  where name like '%张%'
    and gender = 1
    and entrydate between '2010-01-01' and '2020-01-01'
  order by update_time desc ;
  ```

- 接口方法(性能低,不安全,存在SQL注入问题)

  ```java
      @Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where name like '%${name}%' and gender = #{gender} and entrydate between #{startEntryDate} and #{endEntryDate} order by update_time desc")
      public Emp select(String name, Short gender, LocalDate startEntryDate, LocalDate endEntryDate);
  ```

- concat() 字符串拼接函数(推荐)

  ```java
      @Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where name like concat('%',#{name},'%') and gender = #{gender} and entrydate between #{startEntryDate} and #{endEntryDate} order by update_time desc")
      public Emp select(String name, Short gender, LocalDate startEntryDate, LocalDate endEntryDate);
  ```

参数名说明

- 在SpringBoot的<font color='red'>2.X版本</font>

  参数名与#{}中的名称对应即可

- 在SpringBoot的<font color='red'>1.X版本</font>/单独使用Mybatis

  需要在方法形参前加`@Param(名)`对应#{}中的名称 

## XML映射文件

规范:

- XML映射文件的名称与Mapper接口名称一致, 并且将XML映射文件和Mapper接口放置在相同包下<font color='red'>(同包同名)</font>.
- XML映射文件的namespace属性与Mapper接口全限定名一致
- XML映射文件中sql语句的id与Mapper接口中的方法名一致, 并保持返回类型一致

![image-20240609152041963](./图片/image-20240609152041963.png)

插件

- <font color='red'>MybatisX</font>是一块基于IDEA的快速开发Mybatis的插件, 为<font color='red'>效率</font>而生.

使用Mybatis的注解, 主要是来完成一些简单的增删改查功能, 如果需要实现复杂的SQL功能, 建议使用XML来配置映射语句.

官方说明: https://mybatis.net.cn/getting-started.html

## 动态SQL

随着用户的输入或外部条件的变化而变化的SQL语句,我们称为**<font color='red'>动态SQL</font>**.

### `<if>`

- **<font color='red'>`<if>`</font>**:用于判断条件是否成立. 使用test属性进行条件判断, 如果条件为true, 则拼接SQL.

- **<font color='red'>\<where></font>**:where元素只会在子元素有内容的情况才插入where子句. 而且会自动去除子句的开头的`and`或者`or`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.qqzj.mapper.EmpMapper">
    <!--resultType: 单条记录所封装的类型-->
    <select id="select" resultType="com.qqzj.pojo.Emp">
        select id,
        username,
        password,
        name,
        gender,
        image,
        job,
        entrydate,
        dept_id,
        create_time,
        update_time
        from emp
        <where>
            <if test="name != null">
                name like concat('%', #{name}, '%')
            </if>
            <if test="gender != null">
                and gender = #{gender}
            </if>
            <if test="startEntryDate != null and endEntryDate != null">
                and entrydate between #{startEntryDate} and #{endEntryDate}
            </if>
        </where>
        order by update_time desc
    </select>
</mapper>
```

#### 案例

完善更新员工功能, 修改为动态更新员工数据信息

需求:

- 动态更新员工信息, 若更新时传递有值, 则更新, 若更新时没有传递值,则不更新

结局方案:

- 动态SQL

```xml
<update id="update">
    update emp
    <set>
        <if test="userName!=null">
            username=#{userName},
        </if>
        <if test="name != null">
            name=#{name},
        </if>
        <if test="gender != null">
            gender=#{gender},
        </if>
        <if test="image != null">
            image=#{image},
        </if>
        <if test="job != null">
            job=#{job},
        </if>
        <if test="entryDate != null">
            entrydate=#{entryDate},
        </if>
        <if test="deptId != null">
            dept_id=#{deptId},
        </if>
        <if test="updateTime != null">
            update_time=#{updateTime}
        </if>
    </set>
    where id = #{id}
</update>
```

**<font color='red'>`<set>`</font>**: 动态地在行首插入SET关键字, 并会删掉额外的逗号. (用在UPDATE语句中)

#### 小结

1. `<if>`

   - 用于判断条件是否成立, 如果条件为true, 则拼接SQL

   - 形式: 

     ```xml
     <if test="name != null">...</if>
     ```

2. `<where>`
   - where元素只会在子元素有内容的情况下才插入where子句, 而且会自动去除子句的开头的`and` 或者 `or`
3. `<set>`
   - 动态的在行首插入`set`关键字, 并会删掉额外的逗号. (用在UPDATE语句中)

### `<foreach>`

- SQL语句

  ```sql
  delete from emp where id in(1,2,3);
  ```

- 接口方法:

  ```java
  public void deleteByIds(List<Integer> ids);
  ```

- XML 映射文件:

  ```xml
      <!--批量删除员工(18, 19, 20)-->
      <!--
          collection: 遍历的集合
          item: 遍历出来的元素
          separator: 分隔符
          open: 遍历开始前拼接的SQL片段
          close: 遍历结束后拼接的SQL片段
      -->
      <delete id="deleteByIds">
          delete
          from emp
          where id in
          <foreach collection="ids" item="id" separator="," open="(" close=")">
              #{id}
          </foreach>;
      </delete>
  ```

属性:

- collection: 遍历的集合
- item: 遍历出来的元素
- separator: 分隔符
- open: 遍历开始前拼接的SQL片段
- close: 遍历结束后拼接的SQL片段

### `<sql><include>`

- `<sql>`:定义可重用的SQL片段
- `<include>`:通过属性`refid`, 指向包含的SQL片段

![image-20240609204853355](./图片/image-20240609204853355.png)

# Day10-案例

## 准备工作

### 需求说明

![image-20240609205613410](./图片/image-20240609205613410.png)

### 环境搭建

- 准备数据库表(dqpt、emp)
- 创建SpringBoot工程, 引入对应的起步依赖(web、Mybatis、MySQL驱动、lombok)
- 配置文件application.properties中引入Mybatis的配置信息, 准备对应的实体类
- 准备对应的Mapper、Service(接口、实现类)、Controller基础结构

### 开发规范

案例基于当前最为主流的前后端分离模式进行开发

#### Restful

- REST(REpresentaional State Transfer), 表述性状态转换, 它是一种软件架构风格

![image-20240609215313778](./图片/image-20240609215313778.png)

![image-20240609215534335](./图片/image-20240609215534335.png)

**<font color='red'>注意事项</font>**

- REST是风格, 是约定方式, 约定不是规定, 可以打破
- 描述模块的功能通常使用复数, 也就是加s的格式来描述, 表示此类资源, 而非单个资源

#### 统一响应结果

- 前后端交互统一响应结果Result

### ![image-20240609215753713](./图片/image-20240609215753713.png)开发流程

![image-20240609215925677](./图片/image-20240609215925677.png)

## 部门管理

### 查询

![image-20240609220355224](./图片/image-20240609220355224.png)

![image-20240609225247922](./图片/image-20240609225247922.png)

**DeptController类**

```java
package com.qqzj.controller;

import com.qqzj.pojo.Dept;
import com.qqzj.pojo.Result;
import com.qqzj.service.DeptService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Slf4j
@RestController
@RequestMapping("/depts")
public class DeptController {

    @Autowired
    DeptService deptService;

    //查询所有部门
    //@RequestMapping(value = "/depts",method = RequestMethod.GET)
    @GetMapping//限定请求方式为GET
    public Result list(){
        // 查询所有部门
        log.info("查询所有部门");
        List<Dept> depts = deptService.list();
        return Result.success(depts);
    }
    //根据id删除部门
    @DeleteMapping("/{id}")
    public Result delete(@PathVariable("id") Integer id){
        deptService.delete(id);
        return Result.success();
    }
    //添加部门
    @PostMapping
    public Result add(@RequestBody Dept dept){
        log.info("添加部门");
        deptService.add(dept.getName());
        return Result.success();
    }
    //根据ID查询部门
    @GetMapping("/{id}")
    public Result getDeptById(@PathVariable("id")Integer id){
        log.info("根据ID查询部门");
        Dept dept = deptService.getDeptById(id);
        return Result.success(dept);
    }
    //根据ID和name修改部门名称
    @PutMapping
    public Result updateById(@RequestBody Dept dept){
        log.info("根据ID和name属性修改部门名称");
        String name = dept.getName();
        if (name == null || name.isEmpty()){
            return Result.error("name不能为空");
        }
        deptService.updateById(dept.getId(),name);
        return Result.success();
    }
}
```

**DeptService接口**

```java
package com.qqzj.service;

import com.qqzj.pojo.Dept;

import java.util.List;


public interface DeptService {
    //查询所有部门
    public List<Dept> list();

    //按照id删除指定部门
    public void delete(Integer id);

    //添加部门
    public void add(String name);
    
    //根据id查询部门
    public Dept getDeptById(Integer id);

    //根据id和name属性修改部门名称
    public void updateById(Integer id,String name);
}
```

**DeptServiceImpl类**

```java
package com.qqzj.service.impl;

import com.qqzj.mapper.DeptMapper;
import com.qqzj.pojo.Dept;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DeptServiceImpl implements com.qqzj.service.DeptService {
    @Autowired
    DeptMapper deptMapper;

    @Override
    public List<Dept> list() {
        // 查询所有部门
        return deptMapper.list();
    }

    @Override
    public void delete(Integer id) {
        //根据id删除部门
        deptMapper.delete(id);
    }

    @Override
    public void add(String name) {
        //添加部门
        deptMapper.add(name);
    }
    
   @Override
    public Dept getDeptById(Integer id) {
        //根据ID查询部门
        return deptMapper.getDeptById(id);
    }

    @Override
    public void updateById(Integer id, String name) {
        //根据id和name属性修改部门名称
        deptMapper.updateById(id,name);
    }
}
```

**DeptMapper类**

```java
package com.qqzj.mapper;

import com.qqzj.pojo.Dept;
import org.apache.ibatis.annotations.Mapper;

import java.util.List;

@Mapper
public interface DeptMapper {
    //查询全部部门
    public List<Dept> list();

    //根据id删除部门
    public void delete(Integer id);

    //添加部门
    public void add(String name);
    
    //根据id查询部门
    public Dept getDeptById(Integer id);

    //根据id和name属性修改部门名称
    public void updateById(Integer id, String name);
}
```

DeptMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.qqzj.mapper.DeptMapper">
    <!--添加部门-->
    <insert id="add">
        insert into  dept values(null,#{name},now(),now());
    </insert>
    <!--根据ID删除部门-->
    <delete id="delete">
        delete from dept where id = #{id}
    </delete>
    <!--查询所有部门-->
    <select id="list" resultType="com.qqzj.pojo.Dept">
        select id, name, create_time, update_time from dept;
    </select>
    <!--根据ID查询部门-->
    <select id="getDeptById" resultType="com.qqzj.pojo.Dept">
        select id, name, create_time, update_time
        from dept
        where id=#{id};
    </select>
    <!--添加部门-->
    <insert id="add">
        insert into  dept values(null,#{name},now(),now());
    </insert>
</mapper>
```

**<font color='red'>注意事项</font>**

- 一个完整的请求路径, 应该是类上的`@RequestMapping`的`value`属性+方法上的`@RequestMapping`的`value`属性.

# Day11-员工管理&文件上传

- `@RequestParam` 的属性defaultValue可以用来设置参数的默认值

- 分页插件PageHelper

  - 引入依赖

    ```xml
            <!--PageHelper分页插件依赖-->
            <dependency>
                <groupId>com.github.pagehelper</groupId>
                <artifactId>pagehelper-spring-boot-starter</artifactId>
                <version>1.4.7</version>
            </dependency>
    ```

  - 使用:

    ```java
     //1.设置分页参数
            PageHelper.startPage(page, pageSize);
            //执行查询
            List<Emp> empList = empMapper.list();
            Page<Emp> p = (Page<Emp>) empList;
            //封装PageBean对象
            return new PageBean(p.getTotal(),p.getResult());
    ```


## 文件上传

### 简介

- 文件上传, 是指将本地图片、视频、音频等文件上传到服务器, 供其他用户浏览或者下载的过程
- 文件上传在项目中应用非常广泛, 我们经常发微博、发微信朋友圈都用到了文件上传功能

![image-20240610220748114](./图片/image-20240610220748114.png)

![image-20240610220813322](./图片/image-20240610220813322.png)

### 本地存储

在服务端, 接收到上传上来的文件之后, 将文件存储在本地服务器磁盘中

```java
@Slf4j
@RestController
public class UploadController {
    @PostMapping("/upload")
    public Result upLoad(String username, Integer age, MultipartFile image) throws IOException {
        log.info("文件上传:{},{},{}",username,age,image);

        //获取原始文件名
        String originalFilename = image.getOriginalFilename();

        //构造唯一的文件名(不能重复) - uuid(通用唯一识别码)
        int i = originalFilename.lastIndexOf('.');
        String extname = originalFilename.substring(i);
        String newFileName = UUID.randomUUID() + extname;

        //将文件存储在服务器的磁盘目录下D:\AAAStudy\02-Javaweb\ServiceLocalhost
        image.transferTo(new File("D:\\AAAStudy\\02-Javaweb\\ServiceLocalhost\\test\\"+newFileName));

        return Result.success();
    }
}
```

![image-20240610224429037](./图片/image-20240610224429037.png)

在SpringBoot中, 文件上传, 默认单个文件允许最大大小为1M. 如果需要上传大文件, 可以进行如下配置:

```properties
#配置单个文件最大上传大小
spring.servlet.multipart.max-file-size=10MB

#配置单个请求最大上传大小(一次请求可以上传多个文件)
spring.servlet.multipart.max-request-size=100MB
```

![image-20240610225724788](./图片/image-20240610225724788.png)

### 阿里云OSS

阿里云对象存储OSS(Object Storage Service) , 是一款海量、安全、低成本、高可靠的云存储服务. 使用OSS, 您可以通过网络随时存储和调用包括文件、图片、音频和视频等在内的各种文件

#### 第三方服务-通用思路

![image-20240610230422219](./图片/image-20240610230422219.png)

**SDK**:Software Development Kit的缩写, 软件开发工具包, 包括辅助软件开发的依赖(jar包)、代码示例等, 都可以叫做SDK

#### 使用步骤

![image-20240610230656444](./图片/image-20240610230656444.png)

**Bucket**:存储空间是用户用于存储对象(Object, 就是文件)的容器, 所有的对象都必须隶属于某个存储空间

#### 集成

![image-20240611031452196](./图片/image-20240611031452196.png)

步骤:

- 引入阿里云OSS上传文件工具类(由官方的示例代码改造而来)
- 上传图片接口开发

```java
package com.qqzj.Utils;

import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.io.InputStream;
import java.util.UUID;

/**
 * 阿里云 OSS 工具类
 */
@Component
public class AliOSSUtils {

    private final String endpoint = "https://oss-cn-beijing.aliyuncs.com";
    private final String accessKeyId = "阿里云accesskey";
    private final String accessKeySecret = "阿里云";
    private final String bucketName = "web-tlias";

    /**
     * 实现上传图片到OSS
     */
    public String upload(MultipartFile file) throws IOException {
        // 获取上传的文件的输入流
        InputStream inputStream = file.getInputStream();

        // 避免文件覆盖
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID().toString() + originalFilename.substring(originalFilename.lastIndexOf("."));

        //上传文件到 OSS
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        ossClient.putObject(bucketName, fileName, inputStream);

        //文件访问路径
        String url = endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + fileName;
        // 关闭ossClient
        ossClient.shutdown();
        return url;// 把上传到oss的路径返回
    }

}
```

```java
@Autowired
    AliOSSUtils aliOSSUtils;
@PostMapping("/upload")
public Result upload(MultipartFile image) throws IOException {
    log.info("文件上传,文件名:{}",image.getOriginalFilename());
    //调用阿里云OSS工具类进行文件上传
    String url = aliOSSUtils.upload(image);
    log.info("文件上传完成, 文件访问路径:{}",url);
    return Result.success(url);
}
```

## 配置文件

![image-20240611104449811](./图片/image-20240611104449811.png)

- **<font color='red'>`@Value`注解通常用于外部配置的属性注入, 具体用法为: `@Value(${配置文件中的key})`</font>**

**![image-20240611105356163](./图片/image-20240611105356163.png)**

### yml配置文件

- SpringBoot提供了多种属性配置方式

  - application.properties

    ```properties
    格式
    server.port=8080
    ```

  - application.yml

    ```yaml
    格式:
    server:
    	port:8080
    	address:127.0.0.1
    ```

  - application.yaml

    ```yaml
    格式:
    server:
    	port:8080
    	address:127.0.0.1
    ```

![image-20240611110957261](./图片/image-20240611110957261.png)

### yml

基本语法:

- 大小写敏感
- 数值前边必须有空格, 作为分隔符
- 使用缩进表示层级关系, 缩进时, 不允许使用Tab键, 只能用空格(idea中会自动将Tab转换为空格)
- 锁紧的空格树木不重要, 只要相同层级的元素左侧对齐即可
- \# 表示注释, 从这个字符一直到行尾, 都会被解析器忽略

![image-20240611111351017](./图片/image-20240611111351017.png)

### 数据格式

- 对象/Map集合:

  ```yaml
  user:
  	name: zhangsan
  	age: 18
  	password: 123456
  ```

- 数组/List/Set集合

  ```yaml
  hobby:
  	- java
  	- game
  	- sport
  ```

**<font color='red'>`@ConfigurationProperties`与`@Value`</font>**

相同点:

- 都是用来注入外部配置的属性的

不同点

- `@Value` 注解只能一个一个的进行外部属性的注入
- `@ConfigurationProperties`可以批量的将外部的属性配置注入到bean对象的属性中

## 完整代码

** [application.yml](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\resources\application.yml) **

```yaml
spring:
  application:
    name: Tlias
#数据库连接信息
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/tlias
    username: root
    password: 1234
  servlet.multipart:
    #上传单个文件大小限制
    max-file-size: 1TB
    #单次上传文件大小限制(一次可上传多个文件)
    max-request-size: 10TB
#Mybatis配置
mybatis:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    map-underscore-to-camel-case: true
#自定义的阿里云OSS配置信息
aliyun:
  oss:
    endpoint: https://oss-cn-beijing.aliyuncs.com
    accessKeyId: 阿里云accesskey
    accessKeySecret: 阿里云
    bucketName: myoss1107

```

**[upload.html](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\resources\static\upload.html) **

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>上传文件</title>
</head>
<body>

    <form action="/upload" method="post" enctype="multipart/form-data">
        姓名: <input type="text" name="username"><br>
        年龄: <input type="text" name="age"><br>
        头像: <input type="file" name="image"><br>
        <input type="submit" value="提交">
    </form>

</body>
</html>
```

** [EmpMapper.xml](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\resources\com\qqzj\mapper\EmpMapper.xml) **

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.qqzj.mapper.EmpMapper">
    <insert id="add">
        insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time)
        values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})
    </insert>
    <update id="update">
        update emp
        <set>
            <if test="username!=null">
                username=#{username},
            </if>
            <if test="name!=null">
                name=#{name},
            </if>
            <if test="gender!=null">
                gender=#{gender},
            </if>
            <if test="image!=null">
                image=#{image},
            </if>
            <if test="job!=null">
                job=#{job},
            </if>
            <if test="entrydate!=null">
                entrydate=#{entrydate},
            </if>
            <if test="deptId!=null">
                dept_id=#{deptId},
            </if>
            update_time=#{updateTime}
        </set>
        where id=#{id}
    </update>
    <delete id="delete">
        delete from emp where id in <foreach collection="ids" item="id" separator="," open="(" close=")">
        #{id}
    </foreach>
    </delete>
    <!--&lt;!&ndash;获取总数据数&ndash;&gt;-->
    <!--<select id="total" resultType="java.lang.Long">-->
    <!--    select count(*) from emp;-->
    <!--</select>-->
    <!--<select id="rows" resultType="com.qqzj.pojo.Emp">-->
    <!--    select id,-->
    <!--           username,-->
    <!--           password,-->
    <!--           name,-->
    <!--           gender,-->
    <!--           image,-->
    <!--           job,-->
    <!--           entrydate,-->
    <!--           dept_id,-->
    <!--           create_time,-->
    <!--           update_time-->
    <!--    from emp-->
    <!--    limit #{page},#{pageSize};-->
    <!--</select>-->
    <select id="getByConditions" resultType="com.qqzj.pojo.Emp">
        select id,
        username,
        password,
        name,
        gender,
        image,
        job,
        entrydate,
        dept_id,
        create_time,
        update_time
        from emp
        <where>
            <if test = "name != null and name != ''">
                name like concat('%', #{name}, '%')
            </if>
            <if test="gender != null">
                and gender = #{gender}
            </if>
            <if test="begin != null and end != null">
                and entrydate between #{begin} and #{end}
            </if>
        </where>
        order by update_time desc
    </select>
    <select id="queryById" resultType="com.qqzj.pojo.Emp">
        select id,
               username,
               password,
               name,
               gender,
               image,
               job,
               entrydate,
               dept_id,
               create_time,
               update_time
        from emp
        where id=#{id}
    </select>
</mapper>
```

**[AliOSSproperties.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\Utils\AliOSSproperties.java)**

```java
package com.qqzj.Utils;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Data
@Component
@ConfigurationProperties(prefix = "aliyun.oss")
public class AliOSSproperties {
    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;
}

```

**[AliOSSUtils.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\Utils\AliOSSUtils.java)**

```java
package com.qqzj.Utils;

import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.io.InputStream;
import java.util.UUID;

/**
 * 阿里云 OSS 工具类
 */
@Component
public class AliOSSUtils {
    //@Value("${aliyun.oss.endpoint}")
    //private String endpoint;
    //@Value("${aliyun.oss.accessKeyId}")
    //private String accessKeyId;
    //@Value("${aliyun.oss.accessKeySecret}")
    //private String accessKeySecret;
    //@Value("${aliyun.oss.bucketName}")
    //private String bucketName;

    @Autowired
    private AliOSSproperties aliOSSproperties;

    /**
     * 实现上传图片到OSS
     */
    public String upload(MultipartFile file) throws IOException {
        //获取阿里云OSS参数
        String endpoint = aliOSSproperties.getEndpoint();
        String accessKeyId = aliOSSproperties.getAccessKeyId();
        String accessKeySecret = aliOSSproperties.getAccessKeySecret();
        String bucketName = aliOSSproperties.getBucketName();
        // 获取上传的文件的输入流
        InputStream inputStream = file.getInputStream();

        // 避免文件覆盖
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID().toString() + originalFilename.substring(originalFilename.lastIndexOf("."));

        //上传文件到 OSS
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        ossClient.putObject(bucketName, fileName, inputStream);

        //文件访问路径
        String url = endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + fileName;
        // 关闭ossClient
        ossClient.shutdown();
        return url;// 把上传到oss的路径返回
    }
}
```

**[EmpService.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\service\EmpService.java)**

```java
package com.qqzj.service;

import com.qqzj.pojo.Emp;
import com.qqzj.pojo.PageBean;

import java.time.LocalDate;
import java.util.List;

public interface EmpService {
    //public PageBean pageBean(Integer page,Integer pageSize);

    public PageBean pageByConditions(String name, Short gender, LocalDate begin, LocalDate end, Integer page, Integer pageSize);

    public void delete(List<Integer> ids);

    void add(Emp emp);

    Emp queryById(Integer id);

    void update(Emp emp);
}
```

**[EmpServiceImpl.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\service\impl\EmpServiceImpl.java)**

```java
package com.qqzj.service.impl;

import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.qqzj.mapper.EmpMapper;
import com.qqzj.pojo.Emp;
import com.qqzj.pojo.PageBean;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.List;

@Service
public class EmpServiceImpl implements com.qqzj.service.EmpService {
    @Autowired
    EmpMapper empMapper;

    //@Override
    //public PageBean pageBean(Integer page,Integer pageSize) {
    //    //Integer start = (page - 1) * pageSize;
    //    //return new PageBean(empMapper.total(),empMapper.rows(start,pageSize));
    //
    //    //1.设置分页参数
    //    PageHelper.startPage(page, pageSize);
    //    //执行查询
    //    List<Emp> empList = empMapper.list();
    //    Page<Emp> p = (Page<Emp>) empList;
    //    //封装PageBean对象
    //    return new PageBean(p.getTotal(),p.getResult());
    //
    //}

    @Override
    public PageBean pageByConditions(String name, Short gender, LocalDate begin, LocalDate end, Integer page, Integer pageSize) {
        //1.设置分页参数
        PageHelper.startPage(page, pageSize);
        //执行查询
        List<Emp> empList = empMapper.getByConditions(name,gender,begin,end);
        Page<Emp> p = (Page<Emp>) empList;
        //封装PageBean对象
        return new PageBean(p.getTotal(),p.getResult());
    }

    @Override
    public void delete(List<Integer> ids) {
        empMapper.delete(ids);
    }

    @Override
    public void add(Emp emp) {
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        empMapper.add(emp);
    }

    @Override
    public Emp queryById(Integer id) {
        return empMapper.queryById(id);
    }

    @Override
    public void update(Emp emp) {
        emp.setUpdateTime(LocalDateTime.now());
        empMapper.update(emp);
    }
}
```

**[Emp.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\pojo\Emp.java)**

```java
package com.qqzj.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.time.LocalDate;
import java.time.LocalDateTime;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class Emp {
    private Integer id;
    private String username;
    private String password;
    private String name;
    private Short gender;
    private String image;
    private Short job;
    private LocalDate entrydate;
    private Integer deptId;
    private LocalDateTime createTime;
    private LocalDateTime updateTime;
}
```

**[PageBean.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\pojo\PageBean.java)**

```java
package com.qqzj.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

import java.util.List;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class PageBean {
    private Long total;//总记录数
    private List<Emp> rows;//当前页数据列表
}
```

**[EmpMapper.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\mapper\EmpMapper.java)**

```java
package com.qqzj.mapper;

import com.qqzj.pojo.Emp;
import org.apache.ibatis.annotations.Mapper;

import java.time.LocalDate;
import java.util.List;

@Mapper
public interface EmpMapper {

    //public Long total();
    //
    //public List<Emp> rows(Integer page, Integer pageSize);

    //查询所有员工
    //@Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp")
    //public List<Emp> list();

    //条件查询员工
    public List<Emp> getByConditions(String name, Short gender, LocalDate begin, LocalDate end);

    public void delete(List<Integer> ids);

    void add(Emp emp);

    public Emp queryById(Integer id);

    void update(Emp emp);
}
```

**[UploadController.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\controller\UploadController.java)**

```java
package com.qqzj.controller;

import com.qqzj.Utils.AliOSSUtils;
import com.qqzj.pojo.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;

@Slf4j
@RestController
public class UploadController {
    @Autowired
    AliOSSUtils aliOSSUtils;
    //@PostMapping("/upload")
    //public Result upLoad(String username, Integer age, MultipartFile image) throws IOException {
    //    log.info("文件上传:{},{},{}",username,age,image);
    //
    //    //获取原始文件名
    //    String originalFilename = image.getOriginalFilename();
    //
    //    //构造唯一的文件名(不能重复) - uuid(通用唯一识别码)
    //    int i = originalFilename.lastIndexOf('.');
    //    String extname = originalFilename.substring(i);
    //    String newFileName = UUID.randomUUID() + extname;
    //
    //    //将文件存储在服务器的磁盘目录下D:\AAAStudy\02-Javaweb\ServiceLocalhost
    //    image.transferTo(new File("D:\\AAAStudy\\02-Javaweb\\ServiceLocalhost\\test\\"+newFileName));
    //
    //    return Result.success();
    //}

    @PostMapping("/upload")
    public Result upload(MultipartFile image) throws IOException {
        log.info("文件上传,文件名:{}",image.getOriginalFilename());
        //调用阿里云OSS工具类进行文件上传
        String url = aliOSSUtils.upload(image);
        log.info("文件上传完成, 文件访问路径:{}",url);
        return Result.success(url);
    }
}
```

**[EmpController.java](..\AAAStudy\02-Javaweb\Day10\Code\Tlias\src\main\java\com\qqzj\controller\EmpController.java)**

```java
package com.qqzj.controller;

import com.qqzj.pojo.Emp;
import com.qqzj.pojo.Result;
import com.qqzj.service.EmpService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.format.annotation.DateTimeFormat;
import org.springframework.web.bind.annotation.*;

import java.time.LocalDate;
import java.util.List;

@Slf4j
@RestController
@RequestMapping("/emps")
public class EmpController {
    @Autowired
    EmpService empService;

    //// 分页查询所有数据
    //@GetMapping
    //public Result list(@RequestParam(defaultValue = "1") Integer page,
    //                   @RequestParam(defaultValue = "10") Integer pageSize) {
    //    log.info("分页查询,参数:{},{}", page, pageSize);
    //    return Result.success(empService.pageBean(page, pageSize));
    //}

    // 条件分页查询
    @GetMapping
    public Result list(String name, Short gender,
                       @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate begin,
                       @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate end,
                       @RequestParam(defaultValue = "1") Integer page,
                       @RequestParam(defaultValue = "10") Integer pageSize) {
        log.info("条件分页查询,参数:name{},gender{},begin{},end{},page{},pageSize{}", name, gender, begin, end, page, pageSize);
        return Result.success(empService.pageByConditions(name, gender, begin, end, page, pageSize));
    }

    //删除员工
    @DeleteMapping("/{ids}")
    public Result delete(@PathVariable List<Integer> ids){
        log.info("批量删除");
        empService.delete(ids);
        return Result.success();
    }

    //添加员工
    @PostMapping
    public Result add(@RequestBody Emp emp){
        log.info("添加员工");
        empService.add(emp);
        return Result.success();
    }

    //根据ID查询员工
    @GetMapping("/{id}")
    public Result queryById(@PathVariable Integer id){
        log.info("查询ID为{}的员工",id);
        Emp emp = empService.queryById(id);
        if (emp == null){
            return Result.error("没有查询到Id为" + id + "的员工");
        }
        return Result.success(emp);
    }

    //根据id修改员工信息
    @PutMapping
    public Result update(@RequestBody Emp emp){
        log.info("修改id为{}的员工信息",emp.getId());
        empService.update(emp);
        return Result.success();
    }
}
```

# Day12-登录认证

## 基础登录功能

在未登录状态下也是可以直接访问部门管理和员工管理等功能

## **<font color='red'>登录校验</font>**

登录标记

- 用户登录成功之后, 每一次请求中, 都可以获取到该标记--会话技术

统一拦截

- 过滤器 Filter
- 拦截器 Interceptor

![image-20240611123524721](./图片/image-20240611123524721.png)

### 会话技术

- 会话: 用户打开浏览器, 访问web服务器的资源, 会话建立, 直到有一方断开连接, 会话结束. 在一次会话中可以包含<font color='red'>多次</font>请求和响应.

- 会话跟踪: 一种维护浏览器状态的方法, 服务器需要识别多次请求是否来自于同一浏览器, 以便同一次会话的多次请求间<font color='red'>共享数据</font>

- 会话跟踪方案:
  - 客户端会话跟踪技术: Cookie
  - 服务端会话跟踪技术: Session
  - 令牌技术

### Cookie

![image-20240611125749568](./图片/image-20240611125749568.png)

- 优点: HTTP协议中支持的技术

- **缺点:**
  - **移动端APP无法使用Cookie**
  - **不安全, 用户可以自己禁用Cookie**
  - **Cookie不能跨域**

![image-20240611130245443](./图片/image-20240611130245443.png)

### Session

![image-20240611131426617](./图片/image-20240611131426617.png)

- 优点: 存储在服务端. 安全

- **<font color='red'>缺点:</font>**

  - **<font color='red'>服务器集群环境下无法直接使用Session</font>**

  - **<font color='red'>Cookie的缺点</font>**

![image-20240611131447614](./图片/image-20240611131447614.png)

### 令牌技术

![image-20240611131603813](./图片/image-20240611131603813.png)

- 优点:
  - 支持PC端、移动端
  - 解决集群环境下的认证问题
  - 减轻服务器端存储压力

- 缺点: 需要自己实现

![image-20240611131752176](./图片/image-20240611131752176.png)

### JWT令牌

#### 简介

- 全称: JSON Web Token(https://jwt.io/)
- 定义了一种简洁的、自包含的格式, 用于在通信双方以json数据格式安全的传输信息. 由于数字签名的存在, 这些信息是可靠的

- 组成:

  - 第一部分: Header(头), 记录令牌类型、签名算法等. 例如: 

    ![image-20240611132614633](./图片/image-20240611132614633.png)

    **Base64:**是一种基于64个可打印字符(A-Z a-z 0-9 + /)来表示二进制数据的编码格式

  - 第二部分: Payload(有效载荷), 携带一些自定义信息、默认信息等. 例如:

    ![image-20240611133450064](./图片/image-20240611133450064.png)

  - 第三部分: Signature(签名), 防止Token被篡改、确保安全性. 将header、payload, 并加入指定秘钥, 通过指定签名算法计算而来

    ![image-20240611133652181](./图片/image-20240611133652181.png)

![image-20240611133625424](./图片/image-20240611133625424.png)

- 场景: 登录认证
  - 登陆成功后, 生成令牌
  - 后续每个请求, 都要携带JWT令牌, 系统在每次请求处理之前, 先校验令牌, 通过后, 再处理

#### 生成

![image-20240611133954362](./图片/image-20240611133954362.png)

![image-20240611134003709](./图片/image-20240611134003709.png)

```java
//生成令牌
public void testGenJwt() {
    Map<String, Object> claims = new HashMap<>();
    claims.put("id",1);
    claims.put("name","tom");
    String jwt = Jwts.builder()
            .signWith(SignatureAlgorithm.HS256, "qqzj")//签名算法
            .setClaims(claims)//设置自定义内容(载荷)
            .setExpiration(new Date(System.currentTimeMillis() + 3600 * 1000))//设置有效期为一个小时
            .compact();
    System.out.println(jwt);
}
```

结果:

```java
eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoidG9tIiwiaWQiOjEsImV4cCI6MTcxODA4ODUwMX0.Osjdcskw0V7rbSdhYwLoh0twzMYpdJgpYrYwGHFkibU
```

![image-20240611135012082](./图片/image-20240611135012082.png)

```java
//JWT解析
@Test
public void testParseJwt(){
    Map<String, Object> claims = Jwts.parser()
            .setSigningKey("qqzj")
            .parseClaimsJws("eyJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoidG9tIiwiaWQiOjEsImV4cCI6MTcxODA4NTI5Mn0.R1FACiqtoZry_nvCiNXRRZtVoUxzXt64D9htvICuJZw")
            .getBody();
    System.out.println(claims);
}
```

**<font color='red'>注意事项</font>**

- JWT校验时使用的签名秘钥, 必须和生成JWT令牌时使用的秘钥是配套的.
- 如果JWT令牌解析校验时报错, 则说明JWT令牌被篡改或失效了, 令牌非法

### 登录后下发令牌

思路:

- 令牌生成: 登陆成功后, 生成JWT令牌, 并返回给前端
- 令牌校验: 在请求到达服务端后, 对令牌进行统一拦截、校验

#### 生成令牌

步骤

- 引入JWT令牌操作工具类

  ```java
  package com.qqzj.Utils;
  
  import io.jsonwebtoken.Claims;
  import io.jsonwebtoken.Jwts;
  import io.jsonwebtoken.SignatureAlgorithm;
  import java.util.Date;
  import java.util.Map;
  
  public class JwtUtils {
  
      private static String signKey = "qqzj";
      private static Long expire = 43200000L;
  
      /**
       * 生成JWT令牌
       * @param claims JWT第二部分负载 payload 中存储的内容
       * @return
       */
      public static String generateJwt(Map<String, Object> claims){
          String jwt = Jwts.builder()
                  .addClaims(claims)
                  .signWith(SignatureAlgorithm.HS256, signKey)
                  .setExpiration(new Date(System.currentTimeMillis() + expire))
                  .compact();
          return jwt;
      }
  
      /**
       * 解析JWT令牌
       * @param jwt JWT令牌
       * @return JWT第二部分负载 payload 中存储的内容
       */
      public static Claims parseJWT(String jwt){
          Claims claims = Jwts.parser()
                  .setSigningKey(signKey)
                  .parseClaimsJws(jwt)
                  .getBody();
          return claims;
      }
  }
  
  ```

- 登录完成后, 调用工具类生成JWT令牌, 并返回

  ```java
  @Slf4j
  @RestController
  @RequestMapping("/login")
  public class LoginController {
      @Autowired
      EmpService empService;
      @PostMapping
      public Result login(@RequestBody Emp emp){
          log.info("登录,用户名:{},密码:{}",emp.getUsername(),emp.getPassword());
          Emp e = empService.getByUsernameAndPassword(emp);
          if (e != null){
              Map<String, Object> claims = new HashMap<>();
              claims.put("id",e.getId());
              claims.put("username",e.getUsername());
              claims.put("name",e.getName());
              String jwt = JwtUtils.generateJwt(claims);//jwt包含了当前登录的员工信息
              return Result.success(jwt);
          }else{
              return Result.error("用户名或密码错误");
          }
      }
  }
  ```

### 过滤器Filter

概述:

- 概念: <font color='red'>Filter过滤器</font>,是Javaweb三大组件(Servlet、Filter、Listener)之一.
- 过滤器可以把对资源的请求<font color='red'>拦截</font>下来, 从而实现一些特殊的功能
- 过滤器一般完成一些<font color='red'>通用</font>的操作, 比如: 登录校验、统一编码处理、敏感字符处理等.

#### 快速入门

1. 定义Filter: 定义一个类, 实现Filter接口, 并重写其所有方法

   ![image-20240611143027802](./图片/image-20240611143027802.png)

2. 配置Filter: Filter类上加`@WebFilter`注解, 配置拦截资源的路径. 引导类(启动类)上加`@ServletComponentScan`开启Servlet组件支持

   ![image-20240611143101747](./图片/image-20240611143101747.png)

#### 详解

提问:

- 放行后访问对应资源, 资源访问完成后, 还会回到Filter中吗? **<font color='red'>会</font>**

- 如果回到Filter中, 是重新执行还是执行放行后的逻辑呢?**<font color='red'>执行放行后的逻辑</font>**

##### Filter拦截路径

- Filter可以根据需求, 配置不同的拦截资源路径:

  ```java
  @WebFilter(urlPatterns = "/")
  ```

  ![image-20240611144947655](./图片/image-20240611144947655.png)

##### 过滤器链

- 介绍: 一个web应用中, 可以配置多个过滤器, 这多个过滤器就形成了一个<font color='red'>过滤器链</font>

- 顺序: 注解配置的Filter, 优先级是按照过滤器类名(字符串)的自然排序

![image-20240611145934208](./图片/image-20240611145934208.png)

### 登录校验-Filter

- 所有的请求, 拦截到了之后, 都需要检验令牌吗?
  - 登录请求不需要
- 拦截到请求后, 什么情况下才可以放行, 执行业务操作?
  - 有令牌, 且令牌校验通过(合法); 否则都返回未登录错误结果

![image-20240611150602776](./图片/image-20240611150602776.png)

![image-20240611150614029](./图片/image-20240611150614029.png)

```java
@Slf4j
@WebFilter(urlPatterns = "/*")
public class LoginCheckFilter implements Filter {
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest)servletRequest;
        HttpServletResponse httpServletResponse = (HttpServletResponse) servletResponse;
        //1.获取请求url
        String requestURI = httpServletRequest.getRequestURI();
        log.info("请求的url:{}",requestURI);
        //2.判断请求url中是否包含login, 如果包含,说明是登录操作,放行
        if (requestURI.contains("login")){
            log.info("登录操作, 放行");
            filterChain.doFilter(servletRequest,servletResponse);
            return;
        }
        //3.获取请求头中的令牌(token)
        String jwt = httpServletRequest.getHeader("token");
        //4.判断令牌是否存在,如果不存在,返回错误结果(NOT_LOGIN)
        if (!StringUtils.hasLength(jwt)){
            log.info("请求头token为空,返回未登录信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换  对象--json------>阿里巴巴fastJSON工具包
            String notLogin = JSONObject.toJSONString(error);
            httpServletResponse.getWriter().write(notLogin);
            return;
        }
        //5.解析token,如果解析失败,返回错误结果(NOT_LOGIN)
        try {
            JwtUtils.parseJWT(jwt);
        }catch (Exception e){//解析失败
            log.info("解析令牌失败,返回未登录的错误信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换  对象--json------>阿里巴巴fastJSON工具包
            String notLogin = JSONObject.toJSONString(error);
            httpServletResponse.getWriter().write(notLogin);
            return;
        }
        //6.放行
        log.info("令牌合法,放行");
        filterChain.doFilter(servletRequest,servletResponse);
    }
}
```

### 拦截器Interceptor

#### 概述

- 概念: 是一种动态拦截方法调用的机制, 类似于过滤器. Spring框架中提供的, 用来动态拦截控制器方法的执行
- 作用: 拦截请求, 在指定的方法调用前后, 根据业务需要执行预先设定的代码

#### 快速入门

1. 定义拦截器, 实现HandlerInterceptor接口, 并重写其所有方法
2. 注册拦截器

![image-20240611154127395](./图片/image-20240611154127395.png)

![image-20240611154210637](./图片/image-20240611154210637.png)

```java
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    @Override//目标资源方法运行前运行, 返回true:放行  返回false:不放行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle 方法执行了");
        //return HandlerInterceptor.super.preHandle(request, response, handler);
        return false;
    }

    @Override//目标资源方法运行后运行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle  方法执行了");
        //HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }

    @Override//视图渲染完毕后运行,最后运行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion  方法执行了");
        //HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
    }
}
```

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Autowired
    LoginCheckInterceptor loginCheckInterceptor;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**");
    }
}
```

#### 详解

##### 拦截路径

- 拦截器可以根据需求, 配置不同的拦截路径:

  ```java
  @Override
  public void addInterceptors(InterceptorRegistry registry){
      registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**")//需要拦截哪些资源
          .excludePathPatterns("/login")//不需要拦截哪些资源;
  }
  ```

![image-20240611155537407](./图片/image-20240611155537407.png)

##### 执行流程

![image-20240611165045141](./图片/image-20240611165045141.png)

**<font color='red'>Filter与Interceptor</font>**

- 接口规范不同: 过滤器需要实现`Filter`接口, 而拦截器需要实现`HandlerInterceptor`接口
- 拦截范围不同: 过滤器`Filter`会拦截所有的资源, 而`Interceptor`只会拦截`Spring`环境中的资源

### 登录校验-Interceptor

```java
@Slf4j
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    @Override//目标资源方法运行前运行, 返回true:放行  返回false:不放行
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle 方法执行了");
        //1.获取请求url
        String url = request.getRequestURI();
        log.info("请求的url:{}",url);
        //2.获取请求头中的令牌(token)
        String jwt = request.getHeader("token");
        //3.判断令牌是否存在,如果不存在,返回错误结果(NOT_LOGIN)
        if (!StringUtils.hasLength(jwt)){
            log.info("请求头token为空,返回未登录信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换  对象--json------>阿里巴巴fastJSON工具包
            String notLogin = JSONObject.toJSONString(error);
            response.getWriter().write(notLogin);
            return false;
        }
        //4.解析token,如果解析失败,返回错误结果(NOT_LOGIN)
        try {
            JwtUtils.parseJWT(jwt);
        }catch (Exception e){//解析失败
            log.info("解析令牌失败,返回未登录的错误信息");
            Result error = Result.error("NOT_LOGIN");
            //手动转换  对象--json------>阿里巴巴fastJSON工具包
            String notLogin = JSONObject.toJSONString(error);
            response.getWriter().write(notLogin);
            return false;
        }
        //6.放行
        log.info("令牌合法,放行");
        //return HandlerInterceptor.super.preHandle(request, response, handler);
        return true;
    }

    @Override//目标资源方法运行后运行
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle  方法执行了");
        //HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }

    @Override//视图渲染完毕后运行,最后运行
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion  方法执行了");
        //HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
    }
}
```

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Autowired
    LoginCheckInterceptor loginCheckInterceptor;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**")
                .excludePathPatterns("/login");
    }
}
```

## 异常处理

![image-20240611173448336](./图片/image-20240611173448336.png)

- 出现异常, 该如何处理?
  - 方案一: 在Controller的方法中进行try...catch处理//不推荐,代码臃肿
  - 方案二: 全局异常处理器//简单优雅推荐

![image-20240611173531406](./图片/image-20240611173531406.png)

### 全局异常处理器

```java
@RestControllerAdvice
public class GlobalExceptionHandler{
	@ExceptionHandler(Exception.class)
	public Result ex(Exception ex){
		ex.printStackTrace();
		return Result.error("操作失败, 请联系管理员");
	}
}
```

`@RestControllerAdvice` = `@ControllerAdvice` + `@ResponseBody`

```java
//全局异常处理器
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(Exception.class)//捕获所有异常
    public Result ex(Exception ex){
        ex.printStackTrace();
        return Result.error("对不起,操作失败,请联系管理员");
    }
}
```

# 事务管理&AOP

## 事务回顾

**<font color='red'>事务</font>**是一组操作的集合, 它是一个不可分割的工作单位, 这些操作<font color='red'>要么同时成功, 要么同时失败</font>

操作:

- 开启事务(一组操作开始前, 开启事务): `start transaction`/`begin`
- 提交事务(这组操作全部成功后, 提交事务): `commit`
- 回滚事务(中间任何一个操作出现异常, 回滚事务): `rollback`

![image-20240611200053928](./图片/image-20240611200053928.png)

## Spring事务管理

### 注解

- 注解:`@Transactional`
- 位置: 业务(service)层的方法上、类上、接口上
- 作用:将当前方法交给Spring进行事务管理, 方法执行前, 开启事务; 成功执行完毕, 提交事务; 出现异常, 回滚事务

![image-20240611200311687](./图片/image-20240611200311687.png)

```yaml
#开启事务管理日志
logging:
	level:
		org.springframework.jdbc.support.jdbcTransactionManager: debug
```

### 事务进阶

#### 事务属性-回滚

**`rollbackFor`**:

- 默认情况下, 只有出现了`RuntimeException`才回滚异常. `rollbackFor`属性用于控制出现何种异常类型, 回滚事务

```java
@Transactional(rollbackFor = Exception.class)//当前方法交给Spring进行事务管理
@Override
public void delete(Integer id) {
    //根据id删除部门
    deptMapper.delete(id);
    //int i =1 / 0;
    if (true){
        throw new Exception("出错啦");
    }
    //根据部门ID删除对应员工的信息
    empMapper.deleteByDeptId(id);
}
```

**`propagation`**

- 事务传播行为: 指的就是当一个事务被另一个事务方法调用时, 这个事务应该如何进行事务控制

![image-20240612085256440](./图片/image-20240612085256440.png)

![image-20240612085340777](./图片/image-20240612085340777.png)

![image-20240612090559689](./图片/image-20240612090559689.png)

#### 事务属性-传播行为

场景

- REQUIRED: 大部分情况下都是用该传播行为即可
- REQUIRES_NEW: 当我们不希望事务之间相互影响时, 可以使用该传播行为. 比如: 下订单前需要记录日志, 不论订单保存成功与否, 都需要保证日志记录能够记录成功

## AOP

### 基础

#### 概述

**<font color='red'>AOP:</font>**Aspect Oriented Programming(<font color='red'>面相切面编程、面相方面编程</font>),其实就是面相特定方法编程.

![image-20240612091329684](./图片/image-20240612091329684.png)

- **实现**
  - 动态代理是面向切面编程最主流的实现. 而SpringAOP是Spring框架的高级技术, 旨在管理bean对象的过程中, 主要通过底层的动态代理机制, 对特定的方法进行编程

### 快速入门

统计各个业务层方法执行耗时

步骤:

- 导入依赖: 在pom.xml中导入AOP的依赖

  ```xml
  <dependency>
  	<groupId>org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter-aop</artifactId>
  </dependency>
  ```

- 编写AOP程序: 针对特定方法根据业务需要进行编程

![image-20240612092138007](./图片/image-20240612092138007.png)

```java
@Slf4j
@Component
@Aspect//AOP类
public class TimeAspect {
    @Around("execution(* com.qqzj.service.*.*(..))")//切入点表达式
    public Object recordTime(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        //1.记录开始时间
        long start = System.currentTimeMillis();
        //2.执行原始方法
        Object proceed = proceedingJoinPoint.proceed();
        //3.记录结束时间,计算执行耗时
        long end = System.currentTimeMillis();
        log.info("{}执行耗时:{}ms",
                proceedingJoinPoint.getSignature(),//原始方法的签名
                end - start);
        return proceed;
    }
}
```

![image-20240612093702547](./图片/image-20240612093702547.png)

![image-20240612093715962](./图片/image-20240612093715962.png)

### 核心概念

- **连接点**: `JoinPoint`, 可以被AOP控制的方法(暗含方法执行时的相关信息)

![image-20240612093935147](./图片/image-20240612093935147.png)

- **通知**: Advice, 指哪些重复的逻辑, 也就是共性功能(最终体现为一个方法)

  ![image-20240612094042751](./图片/image-20240612094042751.png)

- **切入点**: PointCut, 匹配连接点的条件, 通知仅会在切入点方法执行时被应用

  ![image-20240612094213045](./图片/image-20240612094213045.png)

![image-20240612094234315](./图片/image-20240612094234315.png)

- **切面**: Aspect, 描述通知与切入点的对应关系(通知+切入点)

  ![image-20240612094350973](./图片/image-20240612094350973.png)

- **目标对象**: Target, 通知所应用的对象

  ![image-20240612094423908](./图片/image-20240612094423908.png)

### 执行流程

![image-20240612094630062](./图片/image-20240612094630062.png)

### AOP进阶

#### 通知类型

1. `@Around`: 环绕通知, 此注解标注的通知方法在目标方法前、后都被执行
2. `@Before`: 前置通知, 此注解标注的通知方法在目标方法前被执行
3. `@After`: 后置通知, 此注解标注的通知方法在目标方法后被执行, 无论是否有异常都会执行
4. `@AfterReturning`: 返回后通知, 此注解标注的通知方法在目标方法后被执行, 有异常不会执行
5. `@AfterThrowing`: 异常后通知, 此注解标注的通知方法在目标方法发生异常后执行

**<font color='red'>注意事项</font>**

- `@Around`环绕通知需要自己调用`ProceedingJoinPoint.proceed()`来让原始方法执行, 其他通知不需要考虑目标方法执行.
- `@Around`环绕通知方法的返回值, 必须指定为Object, 来接收原始方法的返回值

```java
public class MyAspect1 {
    @Before("execution(* com.qqzj.service.impl.DeptServiceImpl.*(..))")
    public void before() {
        log.info("before...");
    }

    @Around("execution(* com.qqzj.service.impl.DeptServiceImpl.*(..))")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before...");

        //调用目标对象的原始方法执行
        Object proceed = proceedingJoinPoint.proceed();

        log.info("around after...");
        return proceed;
    }

    @After("execution(* com.qqzj.service.impl.DeptServiceImpl.*(..))")
    public void after() {
        log.info("after...");
    }

    @AfterReturning("execution(* com.qqzj.service.impl.DeptServiceImpl.*(..))")
    public void afterReturning() {
        log.info("afterReturning...");
    }

    @AfterThrowing("execution(* com.qqzj.service.impl.DeptServiceImpl.*(..))")
    public void afterThrowing(){
        log.info("afterThrowing...");
    }
}
```

执行结果

```java
没有异常:
around before...
before...

afterReturning...
after...
around after...

出现异常:
around before...
before...

afterThrowing...
after...
```

##### `@PointCut`

- 该注解的作用是将公共的切点表达式抽取出来, 需要用到时引用该切点表达式即可.

  ```java
  @Pointcut("execution(* com.qqzj.service.impl.DeptServiceImpl.*(..))")
  public void pt(){}
  
  @Before("pt()")
  public void before() {
      log.info("before...");
  }
  ```

![image-20240612102305217](./图片/image-20240612102305217.png)

#### 通知顺序

当有多个切面的切入点都匹配到了目标方法, 目标方法运行时, 多个通知方法都会执行

执行顺序

1. 不同切面类中, 默认按照切面类的<font color='red'>类名字母排序</font>:
   - 目标方法前的通知方法: 字母排名靠前的先执行
   - 目标方法后的通知方法: 字母排名靠前的后执行

2. 用<font color='red'>`@Order(数字)`加在切面类上来控制顺序</font>
   - 目标方法前的通知方法: 数字小的先执行
   - 目标方法后的通知方法: 数字小的后执行

#### 切入点表达式

- 切入点表达式: 描述切入点方法的一种表达式
- 作用:主要用来决定项目中的哪些方法需要加入通知
- 常见形式:
  - `execution(...)`: 根据方法的签名来匹配
  - `@annotation(...)`: 根据注解匹配

![image-20240612103651411](./图片/image-20240612103651411.png)

##### 切入点表达式-`execution`

`execution`主要根据方法的返回值、包名、类名、方法名、方法参数等信息来匹配, 语法为:

```java
execution(访问修饰符? 返回值    包名.类名.?方法名(方法参数) throws 异常?)
```

- 其中带<font color='red'>?</font>的标识可以省略的部分
  - 访问修饰符: 可省略(比如: public、protected)
  - 包名.类名: 可省略(不建议省略)
  - throws  异常:可省略(注意是方法上声明抛出的异常, 不是实际抛出的异常)

```java
@Before("execution(public void com.qqzj.service.impl.DeptServiceImpl.delete(java.lang.Integer))")
public void before(JoinPoint joinPoint){}
```

- 可以使用通配符描述切入点

  - `*`: 单个独立的任意符号, 可以通配任意返回值、包名、类名、方法名、任意类型的一个参数, 也可以通配包、类、方法名的一部分

    ```java
    execution(* com.*.service.*.update*(*))
    ```

  - `..`: 多个连续的任意符号, 可以通配任意层级的包、或者任意类型、任意个数的参数

    ```java
    execution(* com.qqzj..DeptService.*(..))
    ```

```java
@Pointcut("execution(public void com.itheima.service.impl.DeptServiceImpl.delete(java.lang.Integer))")
@Pointcut("execution(void com.itheima.service.impl.DeptServiceImpl.delete(java.lang.Integer))")
@Pointcut("execution(void delete(java.lang.Integer))") //包名.类名不建议省略
@Pointcut("execution(void com.itheima.service.DeptService.delete(java.lang.Integer))")

@Pointcut("execution(void com.itheima.service.DeptService.*(java.lang.Integer))")
@Pointcut("execution(* com.*.service.DeptService.*(*))")
@Pointcut("execution(* com.itheima.service.*Service.delete*(*))")

@Pointcut("execution(* com.itheima.service.DeptService.*(..))")
@Pointcut("execution(* com..DeptService.*(..))")
@Pointcut("execution(* com..*.*(..))")
@Pointcut("execution(* *(..))") //慎用
```

**<font color='red'>注意事项</font>**

- 根据业务需要, 可以使用 且(&&)、或(||)、非(!)来组合比较复杂的切入点表达式

- 书写建议
  - 所有业务<font color='red'>方法名</font>在<font color='red'>命名</font>时尽量<font color='red'>规范</font>,方便切入点表达式快速匹配. 如: 查询类方法都是find开头、更新类方法都是update开头.
  - 描述切入点方法通常<font color='red'>基于接口描述</font>,而不是直接描述实现类,<font color='red'>增强拓展性</font>.
  - 在满足业务需要的前提下, <font color='red'>尽量缩小切入点的匹配范围</font>.如: 包名匹配尽量不使用`..`,使用`*`匹配单个包

切入点表达式-`@annotation`

- `@annotation`切入点表达式, 用于匹配标识有特定注解的方法

- ```java
  @annotation(com.qqzj.anno.Log)
  ```

  ![image-20240612135246392](./图片/image-20240612135246392.png)

```java
自定义注解
package com.itheima.aop;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyLog {
}
```

#### 连接点

- 在Spring中用<font color='red'>`JoinPoint`</font>抽象了连接点, 用它可以获得方法执行时的相关信息, 如目标类名、方法名、方法参数等.
  - 对于`@Around`通知, 获取连接点信息只能使用:`ProceedingJoinPoint`
  - 对于其他四种通知, 获取连接点信息只能使用: `JoinPoint`, 它是`ProceedingJoinPoint` 的父类型

![image-20240612140316691](./图片/image-20240612140316691.png)

![image-20240612140354841](./图片/image-20240612140354841.png)

```java
@Around("pt()")
public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    log.info("around before...");

    //1.获取 目标对象类名                      获取类名     获取类     获取名
    String className = proceedingJoinPoint.getTarget().getClass().getName();
    log.info("目标对象类名:{}",className);
    //2.获取 目标方法的方法名                     获取方法签名    获取方法名
    String methodName = proceedingJoinPoint.getSignature().getName();
    log.info("目标方法名:{}",methodName);
    //3.获取 目标方法运行时传入的参数
    Object[] args = proceedingJoinPoint.getArgs();
    log.info("目标方法参数:{}", Arrays.toString(args));
    //4.放行 目标方法执行
    Object proceed = proceedingJoinPoint.proceed();
    //5.获取 目标方法运行的返回值
    log.info("目标方法返回值:{}",proceed);
    log.info("around after...");
    return proceed;
}
```

### 案例

将案例中增删改相关接口的操作日志记录到数据库表中

操作日志:

日志信息包含: 操作人、操作时间、执行方法的全类名、执行方法名、方法运行时参数、返回值、方法执行时长

步骤

- 准备:
  - 在案例工程中引入AOP的起步依赖
  - 导入资料中准备好的数据表结构, 并引入对应的实体类
- 编码:
  - 自定义注解`@Log`
  - 定义切面类, 完成记录操作日志的逻辑

获取当前登录用户

- 获取request对象, 从请求头中获取到jwt令牌, 解析令牌取出当前用户的id

```java
@Slf4j
@Component
@Aspect
public class log {
    @Autowired
    OperateLogMapper operateLogMapper;
    @Autowired
    HttpServletRequest httpServletRequest;
    @Around("@annotation(com.qqzj.AOP.MyLog)")
    public Object logInto(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        OperateLog operateLog = new OperateLog();
        operateLog.setId(null);
        //操作人
        //获取请求头中的jwt令牌解析令牌
        String token = httpServletRequest.getHeader("token");
        Claims claims = JwtUtils.parseJWT(token);
        Integer id = (Integer) claims.get("id");
        operateLog.setOperateUser(id);
        //操作时间
        operateLog.setOperateTime(LocalDateTime.now());
        //执行方法的全类名
        operateLog.setClassName(proceedingJoinPoint.getTarget().getClass().getName());
        //执行方法名
        operateLog.setMethodName(proceedingJoinPoint.getSignature().getName());
        //方法运行时参数
        operateLog.setMethodParams(Arrays.toString(proceedingJoinPoint.getArgs()));
        //调用原始方法执行
        long begin = System.currentTimeMillis();
        Object proceed = proceedingJoinPoint.proceed();
        long end = System.currentTimeMillis();
        //返回值
        operateLog.setReturnValue(JSONObject.toJSONString(proceed));
        //方法执行时长
        operateLog.setCostTime(end - begin);
        operateLogMapper.insert(operateLog);
        return proceed;
    }
}
```

# SpringBoot

## 配置优先级

- SpringBoot中支持三种格式的配置文件:

  ![image-20240612160710665](./图片/image-20240612160710665.png)

`properties `> `yml `> `yaml`

**<font color='red'>注意事项</font>**

- 虽然SpringBoot支持多种格式配置文件, 但是在项目开发中, 推荐统一使用一种格式的配置 <font color='red'>(yml是主流)</font>

---

- SpringBoot除了支持配置文件属性配置, 还支持<font color='red'>Java系统属性</font>和<font color='red'>命令行参数</font>的方式进行属性配置

  - Java系统属性

    ```java
    -Dserver.port=9000
    ```

  - 命令行参数

    ```cmd
    --server.port=10010
    ```

![image-20240612163907538](./图片/image-20240612163907538.png)

命令行参数 > Java系统属性

打包后配置属性:

1. 执行Maven打包指令package

2. 执行Java指令, 运行jar包

   ```
   java -Dserver.port=9000 -jar jar包名称 --server.port=10010
   ```

**<font color='red'>注意事项</font>**

- SpringBoot项目进行打包时, 需要引入插件<font color='red'>spring-boot-maven-plugin</font>(基于官网骨架创建项目, 会自动添加该插件)

**<font color='red'>优先级(高→低)</font>**

- 命令行参数(--xxx=xxx)
- ↓
- Java系统属性(-xxx=xxx)
- ↓
- application.properties
- ↓
- application.yml
- ↓
- application.yaml



## Bean管理

- 默认情况下, Spring项目启动时, 会把Bean都创建好放在IOC容器中, 如果想要主动获取这些Bean, 可以通过如下方式:

  - 根据name获取Bean:

    ```java
    Object getBean(String name)
    ```

  - 根据类型获取Bean:

    ```java
    <T> T getBean(Class<T> requiredType)
    ```

  - 根据name获取Bean(带类型转换):

    ```java
    <T> T getBean(String name, Class<T> requiredType)
    ```

```java
@Test
void contextLoads() {
    //根据Bean的名称获取
    DeptController bean1 = (DeptController) applicationContext.getBean("deptController");
    System.out.println(bean1);
    //根据Bean的类型获取
    DeptController bean2 = applicationContext.getBean(DeptController.class);
    System.out.println(bean2);
    //根据Bean的名称和类型获取
    DeptController bean3 = applicationContext.getBean("deptController", DeptController.class);
    System.out.println(bean3);
}
```

```java
运行结果
com.qqzj.controller.DeptController@211a9647
com.qqzj.controller.DeptController@211a9647
com.qqzj.controller.DeptController@211a9647
```

**<font color='red'>注意事项</font>**

上述所说的[Spring项目启动时, 会把其中的Bean都创建好] 还会受到作用于及延迟初始化影响, 这里主要针对于 默认单例非延迟加载的bean而言

### bean作用域

- Spring支持五种作用域, 后三种在web环境才生效

  ![image-20240612190333490](./图片/image-20240612190333490.png)

- 可以通过`@Scope`注解来进行配置作用域

```java
@Scope("prototype")
@RestController
@RequestMapping("/depts")
public class DeptController {
```

**<font color='red'>注意事项</font>**

- 默认`singleton`的bean, 在容器启动时被创建, 可以使用`@Lazy`注解来延迟初始化(延迟到第一次使用时)
- `prototype`的bean, 每一次使用该bean的时候都会创建一个新的实例.
- **实际开发当中, 绝大部分的bean是单例的, 也就是说绝大部分bean不需要配置Scope属性**

## 第三方bean

`@Bean`

- 如果要管理的Bean对象来自于第三方(不是自定义的), 是无法用`@Component`及衍生注解声明bean的, 就需要用到`@Bean`注解

![image-20240612192142778](./图片/image-20240612192142778.png)

```java
//声明第三方bean
@Bean//将当前方法的返回值对象交给IOC容器管理, 成为IOC容器的bean
public SAXReader saxReader(){
    return new SAXReader();
}
```

一般将此类代码放到单独的一个配置类集中管理

```java
@Configuration
public class CommonConfig{
	@Bean
	public SAXReader saxReader(){
		return new SAXReader;
	}
}
```

```java
@Configuration
public class CommonConfig {
    //声明第三方bean
    @Bean//将当前方法的返回值对象交给IOC容器管理, 成为IOC容器的bean
         //通过@Bean注解的name/value属性指定bean的名称, 如果没有指定,默认是方法名
    public SAXReader reader(){
        return new SAXReader();
    }
}
```

**<font color='red'>注意事项</font>**

- 通过`@Bean`注解的`name`或`value`属性可以声明bean的名称, 如果不指定, 默认bean的名称就是方法名
- 如果第三方bean需要依赖其他bean对象, 直接在bean定义方法中设置形参即可, 容器会根据类型自动装配

## SpringBoot原理

## 起步依赖

起步依赖的原理: 依赖传递

## 自动配置

- SpringBoot的自动配置就是当spring容器启动后, 一些配置类、bean对象就自动存入到了IOC容器中, 不需要我们手动去声明, 从而简化了开发, 省去了繁琐的配置操作

### 原理

![image-20240612201634250](./图片/image-20240612201634250.png)

```xml
<!--引入第三方提供的依赖-->
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>itheima-utils</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
```

![image-20240612201713166](./图片/image-20240612201713166.png)

```java
//获取TokenParser
@Test
public void testTokenParser(){
    System.out.println(applicationContext.getBean(TokenParser.class));
}

//获取HeaderParser
@Test
public void testHeaderParser(){
    System.out.println(applicationContext.getBean(HeaderParser.class));
}

//获取HeaderGenerator
@Test
public void testHeaderGenerator(){
    System.out.println(applicationContext.getBean(HeaderGenerator.class));
}
```

运行结果:

```java
org.springframework.beans.factory.NoSuchBeanDefinitionException
```

- 解决方案一: `@ComponentScan`组件扫描//<font color='red'>使用繁琐, 性能低</font>

```java
@ComponentScan({"com.example", "com.qqzj"})
@SpringBootApplication
public class SpringbootWebConfigApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootWebConfigApplication.class, args);
    }
}
```

- j解决方案二: `@Import`导入. 使用`@Import`导入的类会被Spring加载到IOC容器中, 成为IOC容器中的bean对象, 导入形式主要有以下几种:
  - 导入普通类
  - 导入配置类
  - 导入 `ImportSelector` 接口实现类
  - `@EnableXxx`注解, 封装`@Import`注解

```java
@Import({TokenParser.class, HeaderConfig.class})
```

```java
//@Import({TokenParser.class})//导入普通类, 交给IOC容器管理
//@Import({HeaderConfig.class})//导入配置类, 交给IOC容器管理
//@Import({MyImportSelector.class})//导入MyImportSelector接口的实现类, 交给IOC容器管理
@EnableHeaderConfig
@SpringBootApplication
public class SpringbootWebConfigApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootWebConfigApplication.class, args);
    }
}
```

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(MyImportSelector.class)
public @interface EnableHeaderConfig {
}
```

```java
public class MyImportSelector implements ImportSelector {
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{"com.example.HeaderConfig"};
    }
}
```

![image-20240612204929936](./图片/image-20240612204929936.png)

**<font color='red'>`@SpringBootApplication`</font>**

- 该注解标识在SpringBoot工程引导类上, 是SpringBoot中<font color='red'>最最最</font>重要的注解. 该注解由三个部分组成:
  - `@SpringBootConfiguration`:该注解与`@Configuration`注解作用相同, 用来声明当前也是一个配置类
  - `@ComponentScan`:组件扫描, 默认扫描当前引导类所在包及其子包
  - `@EnableAutoConfiguration`:SpringBoot实现自动化配置的核心注解

![image-20240612205242802](./图片/image-20240612205242802.png)

![image-20240612205553568](./图片/image-20240612205553568.png)

### `@Conditional`

- 作用: 按照一定的条件进行判断, 在满足给定条件后才会注册对应的bean对象到Spring IOC容器中
- 位置: 方法、类
- `@Conditional`本身是一个父注解, 派生出大量的子注解:
  - `@ConditionOnClass`: 判断环境中是否有对应字节码文件, 才注册bean到IOC容器
  - `@COnditionalOnMissingBean`: 判断环境中没有对应的bean(类型 或 名称), 才注册bean到IOC容器
  - `@ConditionalOnProperty`: 判断配置文件中有对应的属性和值, 才注册bean到IOC容器

```java
@Bean
//@ConditionalOnClass(name = "io.jsonwebtoken.Jwts")//环境中存在指定的这个类, 才会将该bean加入IOC容器中
//@ConditionalOnMissingBean//如果不存在该类型的bean, 才会将该bean加入IOC容器中
//@ConditionalOnMissingBean//指定类型(Value属性)  或  名称(name属性)
@ConditionalOnProperty(name = "name", havingValue = "qqzj")//判断当前配置文件中是否存在指定的属性与值
public HeaderParser headerParser(){
    return new HeaderParser();
}
```

### 案例(自定义starter)

- 在实际开发中, 经常会定义一些公共组件, 提供给各个项目团队使用. 而在SpringBoot的项目中, 一般会将这些公共组件封装成SpringBoot的starter

![image-20240613053136970](./图片/image-20240613053136970.png)

需求:

- 自定义aliyun-oss-spring-boot-starter, 完成阿里云操作工具类AliyunOSSUtils的自动配置
- 目标: 引入起步依赖之后, 要想使用阿里云OSS, 注入AliyunOSSUtils直接使用即可

步骤:

- 创建aliyun-oss-spting-boot-starter模块
- 创建aliyun-oss-spring-boot-autoconfigure模块, 在starter中引入该模块
- 在aliyun-oss-spring-boot-autoconfigure模块中定义自动配置功能, 并定义自动配置文件META-INF/spring/xxx.imports



生成iml文件:

```
按两下ctrl，在弹出的窗口右上角点击project，在下拉列表中选择需要生成.iml文件的模块，左边运行写 : mvn idea:module ，然后回车运行，即可生成.iml文件
```

![image-20240613063653671](./图片/image-20240613063653671.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-oss-spring-boot-starter</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-oss-spring-boot-autoconfigure</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
    </dependencies>

</project>
```

![image-20240613063825659](./图片/image-20240613063825659.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-parent</artifactId>
       <version>3.3.0</version>
       <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-oss-spring-boot-autoconfigure</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
       <java.version>17</java.version>
    </properties>
    <dependencies>
       <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter</artifactId>
       </dependency>
       <dependency>
          <groupId>com.aliyun.oss</groupId>
          <artifactId>aliyun-sdk-oss</artifactId>
          <version>3.15.1</version>
       </dependency>
       <dependency>
          <groupId>javax.xml.bind</groupId>
          <artifactId>jaxb-api</artifactId>
          <version>2.3.1</version>
       </dependency>
       <dependency>
          <groupId>javax.activation</groupId>
          <artifactId>activation</artifactId>
          <version>1.1.1</version>
       </dependency>
       <!-- no more than 2.3.3-->
       <dependency>
          <groupId>org.glassfish.jaxb</groupId>
          <artifactId>jaxb-runtime</artifactId>
          <version>2.3.3</version>
       </dependency>
       <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
    </dependencies>

</project>
```

```java
package com.aliyun.oss;

import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableConfigurationProperties(AliOSSproperties.class)
public class AliOSSAutoConfiguration {

    @Bean
    public AliOSSUtils aliOSSUtils(AliOSSproperties aliOSSproperties) {
        AliOSSUtils aliOSSUtils = new AliOSSUtils();// 创建一个AliOSSUtils实例
        aliOSSUtils.setAliOSSproperties(aliOSSproperties);
        return aliOSSUtils;
    }
}
```

```java
package com.aliyun.oss;

import org.springframework.boot.context.properties.ConfigurationProperties;


@ConfigurationProperties(prefix = "aliyun.oss")
public class AliOSSproperties {
    private String endpoint;
    private String accessKeyId;
    private String accessKeySecret;
    private String bucketName;

    public String getEndpoint() {
        return endpoint;
    }

    public void setEndpoint(String endpoint) {
        this.endpoint = endpoint;
    }

    public String getAccessKeyId() {
        return accessKeyId;
    }

    public void setAccessKeyId(String accessKeyId) {
        this.accessKeyId = accessKeyId;
    }

    public String getAccessKeySecret() {
        return accessKeySecret;
    }

    public void setAccessKeySecret(String accessKeySecret) {
        this.accessKeySecret = accessKeySecret;
    }

    public String getBucketName() {
        return bucketName;
    }

    public void setBucketName(String bucketName) {
        this.bucketName = bucketName;
    }
}
```

```java
package com.aliyun.oss;

import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.io.InputStream;
import java.util.UUID;

/**
 * 阿里云 OSS 工具类
 */
public class AliOSSUtils {
    //@Value("${aliyun.oss.endpoint}")
    //private String endpoint;
    //@Value("${aliyun.oss.accessKeyId}")
    //private String accessKeyId;
    //@Value("${aliyun.oss.accessKeySecret}")
    //private String accessKeySecret;
    //@Value("${aliyun.oss.bucketName}")
    //private String bucketName;

    private AliOSSproperties aliOSSproperties;

    public AliOSSproperties getAliOSSproperties() {
        return aliOSSproperties;
    }

    public void setAliOSSproperties(AliOSSproperties aliOSSproperties) {
        this.aliOSSproperties = aliOSSproperties;
    }

    /**
     * 实现上传图片到OSS
     */
    public String upload(MultipartFile file) throws IOException {
        //获取阿里云OSS参数
        String endpoint = aliOSSproperties.getEndpoint();
        String accessKeyId = aliOSSproperties.getAccessKeyId();
        String accessKeySecret = aliOSSproperties.getAccessKeySecret();
        String bucketName = aliOSSproperties.getBucketName();
        // 获取上传的文件的输入流
        InputStream inputStream = file.getInputStream();

        // 避免文件覆盖
        String originalFilename = file.getOriginalFilename();
        String fileName = UUID.randomUUID().toString() + originalFilename.substring(originalFilename.lastIndexOf("."));

        //上传文件到 OSS
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        ossClient.putObject(bucketName, fileName, inputStream);

        //文件访问路径
        String url = endpoint.split("//")[0] + "//" + bucketName + "." + endpoint.split("//")[1] + "/" + fileName;
        // 关闭ossClient
        ossClient.shutdown();
        return url;// 把上传到oss的路径返回
    }

}
```

 [org.springframework.boot.autoconfigure.AutoConfiguration.imports](..\AAAStudy\02-Javaweb\Day14\Code\aliyun-oss-spring-boot-autoconfigure\src\main\resources\META-INF\spring\org.springframework.boot.autoconfigure.AutoConfiguration.imports) 

```improts
com.aliyun.oss.AliOSSAutoConfiguration
```

## 总结

![image-20240613064245702](./图片/image-20240613064245702.png)

![image-20240613064320306](./图片/image-20240613064320306.png)

![image-20240613064439840](./图片/image-20240613064439840.png)

# Maven高级

## 分模块设计与开发

- 将项目按照功能拆分成若干个子模块, 方便项目的管理维护、扩展, 也方便模块间的相互调用, 资源共享

## 实践

### 将tlias系统分模块

步骤:

- 创建maven模块 tlias-pojo,存放实体类
- 创建maven模块 tlias-utils,存放相关工具类

**<font color='red'>注意事项</font>**

- 分模块开发需要先针对模块功能进行设计, 再进行编码, 不会先将工程开发完毕, 然后进行拆分

###  [完整项目文件](..\AAAStudy\02-Javaweb\Day15) 

### 总结

1. 什么是分模块设计?
   - 将项目按照功能拆分成若干个子模块
2. 为什么要分模块设计?
   - 方便项目的管理维护、扩展、也方便模块间的相互调用, 资源共享
3. <font color='red'>注意事项</font>
   - <font color='red'>分模块设计需要先针对模块功能进行设计, 再进行编码. 不会先将工程开发完毕, 然后进行拆分</font>

## 继承与聚合

### 继承

- 概念: <font color='red'>继承</font>描述的是两个工程间的关系, 与java中的继承相似, 子工程可以继承父工程中的配置信息, 常见于依赖关系的继承
- 作用: 简化依赖配置、统一管理依赖
- 实现: **<font color='red'>`<parent>...</parent>`</font>**

###  继承关系实现

1. 创建maven模块 tlias-parent, 该工程为<font color='red'>父工程</font>, 设置<font color='red'>打包方式pom</font>(默认jar)

   ```java
   <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>3.2.6</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
   
       <groupId>com.qqzj</groupId>
       <artifactId>tilas-parent</artifactId>
       <version>1.0-SNAPSHOT</version>
       <packaging>pom</packaging>
   ```

打包方式:

- jar: 普通模块打包, SpringBoot项目基本都是jar包(内嵌Tomcat运行)
- war: 普通web程序打包, 需要部署在外部的Tomcat服务器中运行
- pom: 父工程或聚合工程, 该模块不写代码, 仅进行依赖管理

2. 在<font color='red'>子工程</font>的pom.xml文件中, 配置继承关系

   ```java
   <parent>
           <groupId>com.qqzj</groupId>
           <artifactId>tilas-parent</artifactId>
           <version>1.0-SNAPSHOT</version>
           <relativePath>../tilas-parent/pom.xml</relativePath>
       </parent>
   ```

3. 在<font color='red'>父工程</font>中配置各个工程共有的依赖(子工程会自动继承父工程的依赖)

**<font color='red'>注意事项</font>**

- 在子工程中, 配置了继承关系之后, 坐标中的`groupId`是可以省略的, 因为会自动继承父工程的.
- `relativePath`指定父工程的pom文件的相对位置(如果不指定, 将从本地仓库/远程仓库查找该工程)
- 若父子工程都配置了同一个依赖的不同版本, 以子工程的为准

### 小结

maven项目父子工程结构说明

![image-20240613150403677](./图片/image-20240613150403677.png)

## 版本锁定

- 在maven中, 可以在父工程的pom文件中通过`<dependencyManagement>`来统一管理依赖版本

![image-20240613150809553](./图片/image-20240613150809553.png)

![image-20240613150856445](./图片/image-20240613150856445.png)

**<font color='red'>注意事项</font>**

- 子工程引入依赖时, 无序指定`<version>`版本号, 父工程统一管理. 变更依赖版本, 只需在父工程中统一变更

### 自定义属性/引用属性

![image-20240613151944027](./图片/image-20240613151944027.png)

### 小结

1. `<dependencyManagement>`与`<dependencies>`的区别
   - `<dependencies>`是直接依赖, 在父工程配置了依赖, 子工程会直接继承下来
   - `<dependencyManagement>`是统一管理依赖版本, 不会直接依赖, 还需要在子工程中引入所需依赖(无需指定版本)

## 聚合

- 将多个模块组织成一个整体, 同时进行项目的构建

- 聚合工程
  - 一个不具有业务功能的“空”工程(有且只有一个pom文件)

![image-20240613155451829](./图片/image-20240613155451829.png)

- 作用
  - 快速构建项目(无需根据依赖关系手动构建, 直接在聚合工程上构建即可)

步骤

- maven中可以通过`<modules>`设置当前聚合工程所包含的子模块名称

![image-20240613155710888](./图片/image-20240613155710888.png)

**<font color='red'>注意事项</font>**

- 聚合工程中所包含的模块, 在构建时, 会自动根据模块间的依赖关系设置构建顺序, 与聚合工程中模块的配置书写位置无关

## 继承与聚合

- 作用
  - 聚合用于快速构建项目
  - 继承用于简化依赖配置、统一管理依赖
- 相同点:
  - 聚合与继承的pom.xml文件打包方式均为pom, 可以将两种关系制作到同一个pom文件中
  - 聚合与继承均属于设计性模块, 并无实际的模块内容
- 不同点:
  - 聚合是在聚合工程中配置关系, 聚合可以感知到参与聚合的模块有哪些
  - 继承是在子模块中配置关系, 父模块无法感知哪些子模块继承了自己

## 私服

- 私服是一种特殊的远程仓库, 它是架设在局域网内的仓库服务, 用来代理位于外部的中央仓库, 用于解决团队内部的资源共享与资源同步问题
- 依赖查找顺序
  - 本地仓库
  - 私服
  - 中央仓库

**<font color='red'>注意事项</font>**

- 私服在企业项目开发中, 一个项目/公司, 只需要一台即可(无需我们自己搭建, 会使用即可)

### 资源上传与下载

#### 上传

![image-20240613164048610](./图片/image-20240613164048610.png)

#### 下载

![image-20240613164159994](./图片/image-20240613164159994.png)

项目版本:

- RELEASE(发行版本): 功能趋于稳定、当前更新停止, 可以用于发行的版本, 存储在私服中的RELEASE仓库中
- SNAPSHOT(快照版本): 功能不稳定、尚处于开发中的版本, 即快照版本, 存储在私服的SNAPSHOT仓库中

#### 资源上传与下载

![image-20240613165156687](./图片/image-20240613165156687.png)

1. 设置私服的访问用户名/密码(settings.xml中的servers中配置)

   ```xml
   <server>
       <id>maven-releases</id>
       <username>adamin</username>
       <password>admin</password>
   </server>
   <server>
       <id>maven-snapshots</id>
       <username>admin</username>
       <password>admin</password>
   </server>
   ```

![image-20240613165212516](./图片/image-20240613165212516.png)

2. IDEA的maven工程的pom文件中配置上传(发布)地址

   ```xml
   <distributionManagement>
       <repository>
           <id>maven-releases</id>
           <url>http://.../repository/maven-releases</url>
       </repository>
       <snapshotRepository>
           <id>maven-releases</id>
           <url>http://.../repository/maven-snapshots</url>
       </snapshotRepository>
   </distributionManagement>
   ```

![image-20240613165450137](./图片/image-20240613165450137.png)

3. 设置私服依赖下载的仓库组地址(settings.xml中的mirrors、profiles中配置)

   ```xml
   <mirror>
       <id>maven-public</id>
       <mirrorOf>*</mirrorOf>
       <url>http://.../repository/maven-public/</url>
   </mirror>
   ```

   ```xml
   <profile>
       <id>allow-snapshots</id>
       <activation>
           <avtiveByDefault>true</avtiveByDefault>
       </activation>
       <repositories>
           <repository>
               <id>maven-public</id>
       	<url>http://.../repository/maven-public/</url>
               <releases>
                   <enabled>true</enabled>
               </releases>
               <snapshots>
                   <enabled>true</enabled>
               </snapshots>
           </repository>
       </repositories>
   </profile>
   ```

   

