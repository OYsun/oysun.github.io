---
title: SASS学习笔记
date: 2016-09-20
tags: web开发 
categories:  web开发
---

{% blockquote %}
SASS和Less一样都是css预处理器。之前一直是学习Less,现在学习SASS，发现SASS在很多地方比Less强大很多。因为有Less的基础，所以学习SASS特别快，先将SASS的学习记录下来当作笔记，以便查阅
{% endblockquote %}

<!--more-->

---------------------------------------------------------------------------------------------------




## SASS的安装

安装SASS必须先安装Ruby,安装好了Ruby后使用Ruby的命令行工具输入

```
gem install sass
```

## SASS的基础用法

#### SASS和SCSS的区别
sass有两种后缀名文件：一种后缀名为sass，不使用大括号和分号；另一种就是我们这里使用的scss文件，这种和我们平时写的css文件格式差不多，使用大括号和分号

```css
//文件后缀名为sass的语法
body
  background: #eee
  font-size:12px
p
  background: #0982c1

//文件后缀名为scss的语法  
body {
  background: #eee;
  font-size:12px;
}
p{
  background: #0982c1;
} 
```
#### 注释风格
Sass 支持标准的CSS多行注释以/* */以及单行注释 //。在尽可能的情况下，多行注释会被保留在输出的CSS中，而单行注释会被删除

```css
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

#### 变量（Variables）

（1）局部变量和全局变量:变量可以在css规则块定义之外存在。当变量定义在css规则块内，那么该变量只能在此规则块内使用。如果它们出现在任何形式的{...}块中（如@media或者@font-face块），情况也是如此

```css
$nav-color: #F90;//全局变量
nav {
  $width: 100px;//局部变量，只能在nav中使用这意味着是你可以在样式表的其他地方定义和使用$width变量，不会对这里造成影响
  width: $width;
  color: $nav-color;
}
//编译后

nav {
  width: 100px;
  color: #F90;
}
```

不过也可以在定义变量的时候可以后面带上!global标志，在这种情况下，视为全局变量

```css
#main {
  $width: 5em !global;//全局变量
  width: $width;
}
```
（2）在声明变量时，变量值也可以引用其他变量。当你通过粒度区分，为不同的值取不同名字时，这相当有用。下例在独立的颜色值粒度上定义了一个变量，且在另一个更复杂的边框值粒度上也定义了一个变量
```css
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
```

（3）一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值，但是有`!default`用于变量，含义是：如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

#### 嵌套规则 (Nested Rules)
Sass 允许将一个 CSS 样式嵌套进另一个样式中，内层样式仅适用于外层样式的选择器范围

```css
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}

 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }




#content aside {
  color: red;
  body.ie & { color: green }
}

/*编译后*/
#content aside {color: red};
body.ie #content aside { color: green }




nav, aside {
  a {color: blue}
}
/*编译后*/
nav a, aside a {color: blue}




article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}

/*编译后*/
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```
在sass中，除了CSS选择器，属性也可以进行嵌套。

```css
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
/*编译后*/
nav {
  
border-style: solid;
  
border-width: 1px;
  
border-color: #ccc;

}



nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}

/*编译后*/
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}

```
#### 混合 (Mixin)

（1）混合器使用@mixin标识符定义。看上去很像其他的CSS @标识符，比如说@media或者@font-face。这个标识符给一大段样式赋予一个名字，这样你就可以轻易地通过引用这个名字重用这段样式

```css
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}

//sass最终生成：

.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```
（2）混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性

```css
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
ul.plain {
  color: #444;
  @include no-bullets;
}
//sass最终生成：
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

（3）混合传参

```css
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
/*
当你@include混合器时，有时候可能会很难区分每个参数是什么意思，参数之间是一个什么样的顺序。为了解决这个问题，sass允许通过语法$name: value的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可
*/
a {
  @include link-colors(blue, red, green);//参数顺序要一致
}
a {
  @include link-colors($normal: blue,$visited: green,$hover: red);//参数顺序可以不一致
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```

（4）参数默认值
```css
@mixin link-colors($normal,$hover: $normal,$visited: $normal)
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

如果像下边这样调用：@include link-colors(red) , $hover和$visited也会被自动赋值为red。还有一种是需要参数作为字符串变量

```css
@mixin firefox-message($selector) {
      body.firefox #{$selector}:before {
    content: "Hi, Firefox users!";
  }
}
@include firefox-message(".header");
编译为：
body.firefox .header:before {
  content: "Hi, Firefox users!"; 
}
```
#### 继承（extend）

> @extend通过在样式表中出现被扩展选择器（例如.error）的地方插入扩展选择器

```css
.error {
  border: 1px red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
//编译为
.error, .seriousError {
  
border: 1px red;
 
background-color: #fdd;

}


.seriousError {
  
border-width: 3px;

}
```

此外，@extend通过在样式表中出现被扩展选择器的地方插入扩展选择器

```css
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.error.intrusion {
  background-image: url("/image/hacked.png");
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
编译为：

.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd;
}

.error.intrusion, .seriousError.intrusion {
  background-image: url("/image/hacked.png");
}

.seriousError {
  border-width: 3px;
}
```

类（class）选择，并不是唯一可以扩展。她可以扩展任何定义给单个元素的选择器，如.special.cool, a:hover, 或 a.user[href^="http://"]
```css
.hoverlink {
  @extend a:hover;
}
a:hover {
  text-decoration: underline;
}
编译为：

a:hover, .hoverlink {
  text-decoration: underline; 
}

```

```css
.hoverlink {
  @extend a:hover;
}
.comment a.user:hover {
  font-weight: bold;
}
编译为：

.comment a.user:hover, .comment .user.hoverlink {
  font-weight: bold; 
}
```

多重扩展 (Multiple Extends)：同一个选择器可以扩展多个选择器。这意味着，它继承了被扩展选择器的所有样式

```css
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.attention {
  font-size: 3em;
  background-color: #ff0;
}
.seriousError {
  @extend .error;
  @extend .attention;
  border-width: 3px;
}
编译为：

.error, .seriousError {
  border: 1px #f00;
  background-color: #fdd; 
}

.attention, .seriousError {
  font-size: 3em;
  background-color: #ff0; 
}

.seriousError {
  border-width: 3px; 
}
```

链式扩展（Chaining Extends）：一个选择器可以扩展另一个选择器，另一个选择器又扩展的第三选择器选择
```css
.error {
  border: 1px #f00;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
.criticalError {
  @extend .seriousError;
  position: fixed;
  top: 10%;
  bottom: 10%;
  left: 10%;
  right: 10%;
}
//编译为
.error, .seriousError, .criticalError {
  border: 1px #f00;
  background-color: #fdd; 
}

.seriousError, .criticalError {
  border-width: 3px; 
}

.criticalError {
  position: fixed;
  top: 10%;
  bottom: 10%;
  left: 10%;
  right: 10%; 
}
```


选择器序列 (Selector Sequences)：选择器序列，比如.foo .bar 或 .foo + .bar，目前还不能作为扩展。但是，选择器序列本身可以使用@extend
```css
#fake-links .link {
  @extend a;
}

a {
  color: blue;
  &:hover {
    text-decoration: underline;
  }
}
将被编译为：

a, #fake-links .link {
  color: blue; 
}
  a:hover, #fake-links .link:hover {
    text-decoration: underline; 
}
```

#### 表达式
表达式是SASS最为强大的特性之一

##### @if

@if 指令需要一个SassScript表达和嵌套在它下面要使用的样式，如果表达式返回值不为 false 或者 null ，那么后面花括号中的内容就会返回：

```css
p {
  @if 1 + 1 == 2 { border: 1px solid;  }
  @if 5 < 3      { border: 2px dotted; }
  @if null       { border: 3px double; }
}
编译为：

p {
  border: 1px solid; 
}
```

@if 语句后面可以跟多个@else if语句和一个 @else 语句。 如果@if语句失败，Sass 将逐条尝试@else if 语句，直到有一个成功，或如果全部失败，那么会执行@else语句。 例如：

```css
$type: monster;
    p {
      @if $type == ocean {
        color: blue;
      } @else if $type == matador {
        color: red;
      } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
编译为：

p {
  color: green; 
}

```

##### @for
- @for指令重复输出一组样式。对于每次重复，计数器变量用于调整输出结果。该指令有两种形式：@for $var from <start> through <end> 和 @for $var from <start> to <end>。注意关键字through 和 to的区别。$var可以是任何变量名，比如$i;<start> 和 <end>是应该返回整数的SassScript表达式。当<start>比<end>大的时候，计数器将递减，而不是增量
- @for语句将设置$var为指定的范围内每个连续的数值，并且每一次输出的嵌套样式中使用$var的值。对于from ... through的形式，范围包括<start>和<end>的值，但from ... to的形式从<start>开始运行，但不包括<end>的值

```css
@for $i from 1 through 3 {
      .item-#{$i} { width: 2em * $i; }
}
编译为：

.item-1 {
  width: 2em; 
}
.item-2 {
  width: 4em; 
}
.item-3 {
  width: 6em; 
}
```

##### @each

- @each指令通常格式是@each $var in <list or map>。$var可以是任何变量名，像$length 或者 $name，和<list or map>是一个返回列表（list）或 map 的 SassScript 表达式。

- @each 规则将$var设置为列表（list）或 map 中的每个项目，输出样式中包含使用$var的值。 例如：

```css
@each $animal in puma, sea-slug, egret, salamander {
      .#{$animal}-icon {
        background-image: url('/images/#{$animal}.png');
  }
}
编译为：

.puma-icon {
  background-image: url('/images/puma.png'); 
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png');
}
.egret-icon {
  background-image: url('/images/egret.png'); 
}
.salamander-icon {
  background-image: url('/images/salamander.png'); 
}
```

@each指令也可以使用多个变量，格式为@each $var1,$var2, ... in <list>。如果<list>是列表（list）中的列表，子列表中的每个元素被分配给各自的变量。例如：

```css
@each $animal, $color, $cursor in (puma, black, default),
                                      (sea-slug, blue, pointer),
                                      (egret, white, move) {
      .#{$animal}-icon {
        background-image: url('/images/#{$animal}.png');
        border: 2px solid $color;
        cursor: $cursor;
  }
}
编译为：

.puma-icon {
  background-image: url('/images/puma.png');
  border: 2px solid black;
  cursor: default; 
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png');
  border: 2px solid blue;
  cursor: pointer; 
}
.egret-icon {
  background-image: url('/images/egret.png');
  border: 2px solid white;
  cursor: move; 
}
```

```css
@each $header, $size in (h1: 2em, h2: 1.5em, h3: 1.2em) {
  #{$header} {
        font-size: $size;
  }
}
编译为：

h1 {
  font-size: 2em; 
}
h2 {
  font-size: 1.5em; 
}
h3 {
  font-size: 1.2em; 
}

```
##### @while

@while 指令重复输出嵌套样式，直到SassScript表达式返回结果为false。这可用于实现比@for语句更复杂的循环，只是很少会用到例如：

```css
$i: 6;
    @while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
      $i: $i - 2;
}
编译为：

.item-6 {
  width: 12em; 
}

.item-4 {
  width: 8em; 
}

.item-2 {
  width: 4em; 
}
```


## 参考资料
- http://sass-lang.com/
- http://www.css88.com/doc/sass/
- http://www.ruanyifeng.com/blog/2012/06/sass.html
- http://www.sassmeister.com/



















