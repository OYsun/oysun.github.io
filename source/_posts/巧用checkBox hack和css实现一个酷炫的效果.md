---
title: 巧用checkBox hack和css实现一个酷炫的效果
date: 2016-05-12 
tags: web前端开发
categories:  web前端开发
---

{% blockquote %}
纯用css完成的动画，巧妙的运用checkBox hack技术和css3的@keyframes动画实现的效果。
{% endblockquote %}

<!--more-->

------------------------------------------------

{% asset_img demo1.gif %}

首先，我是用`<input>`元素和`<label>`元素的组合，在`<label>`元素中放置的是每一个input的具体内容。图标使用的是[Font Awesome](http://fontawesome.io/).

{% asset_img demo1-j-1.PNG %}

为了使得展示界面更美观，网页设计常用css3渐变背景，附上代码
```css
/*渐变背景样式*/
body {
  -webkit-perspective: 800px;
  perspective: 800px;
  height: 100vh;
  margin: 0;
  overflow: hidden;
  font-family: 'Gudea', sans-serif;
  background: #EA5C54;
  /*下面是背景渐变样式，根据不同浏览器hack*/
  /* FF3.6+  */
  background: -webkit-gradient(linear, left top, right bottom, color-stop(0%, #EA5C54), color-stop(100%, #bb6dec));
  /* Chrome,Safari4+ */
  background: -webkit-linear-gradient(-45deg, #EA5C54 0%, #bb6dec 100%);
  /* Chrome10+,Safari5.1+ */
  /* Opera 11.10+ */
  /* IE10+ */
  background: -webkit-linear-gradient(315deg, #EA5C54 0%, #bb6dec 100%);
  background: linear-gradient(135deg, #EA5C54 0%, #bb6dec 100%);
  /* W3C */
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#EA5C54 ', endColorstr='#bb6dec',GradientType=1 );
  /* IE6-9 回退机制 */
}
```
为什么每个块不用`<div>`而是用`input`和`label`标签呢？大家发现没有，这个动画中，当这四个input其中一个被选中时，其左侧有个放大的灰色图标出现动画。如果用div包裹我想得用js实现了，但是用input的checkBox hack的方法可以很巧妙的实现：
```css
.app_inner {
  position: relative;
}
.app_inner input[type="radio"] {
  display: none;
}
.app_inner input[type="radio"]:checked + label .app_inner__tab {
  height: 175px;
}
.app_inner input[type="radio"]:checked + label .app_inner__tab .tab_right {
  top: 39px;
  -webkit-transition: all 0.3s 0.2s cubic-bezier(0.455, 0.03, 0.515, 0.955);
  transition: all 0.3s 0.2s cubic-bezier(0.455, 0.03, 0.515, 0.955);
}
.app_inner input[type="radio"]:not(checked) + label .app_inner__tab {
  height: 80px;
  border-left: 12px solid rgba(0, 0, 0, 0.12);
}
.app_inner input[type="radio"]:not(checked) + label .app_inner__tab .tab_right {
  top: 200px;
  -webkit-transition: all 0.3s 0.3s cubic-bezier(0.455, 0.03, 0.515, 0.955);
  transition: all 0.3s 0.3s cubic-bezier(0.455, 0.03, 0.515, 0.955);
}
.app_inner input[type="radio"]:checked + label .app_inner__tab .tab_left .tab_left__image {
  -webkit-animation: move_in 0.55s 0.05s cubic-bezier(0.455, 0.03, 0.515, 0.955) forwards;
          animation: move_in 0.55s 0.05s cubic-bezier(0.455, 0.03, 0.515, 0.955) forwards;
  -webkit-transition: all 0.3s 0.36s cubic-bezier(0.455, 0.03, 0.515, 0.955);
  transition: all 0.3s 0.36s cubic-bezier(0.455, 0.03, 0.515, 0.955);
}
.app_inner input[type="radio"]:not(checked) + label .app_inner__tab .tab_left .tab_left__image {
  -webkit-animation: move_out 0.75s 0s cubic-bezier(0.455, 0.03, 0.515, 0.955) forwards;
          animation: move_out 0.75s 0s cubic-bezier(0.455, 0.03, 0.515, 0.955) forwards;
  -webkit-transition: all 0.3s 0.3s cubic-bezier(0.455, 0.03, 0.515, 0.955);
  transition: all 0.3s 0.3s cubic-bezier(0.455, 0.03, 0.515, 0.955);
}
.app_inner input[type="radio"]:checked + label .app_inner__tab .tab_left .big {
  left: 260px;
}
.app_inner input[type="radio"]:not(checked) + label .app_inner__tab .tab_left .big {
  left: 400px;
}
.app_inner input[type="radio"]:checked + label .app_inner__tab h2 i {
  opacity: 0;
}
.app_inner input[type="radio"]:not(checked) + label .app_inner__tab h2 i {
  opacity: .3;
}    
```
每一个块都是input，当input被选中时，就会出现这个灰色的大图标，相反，则隐藏大图标，这样就不需要使用js了。至于后面图标平滑的动画则是使用css3的@keyframes。
```css
    @-webkit-keyframes move_out {
  0% {
    top: 47px;
  }
  100% {
    top: 200px;
  }
}

@keyframes move_out {
  0% {
    top: 47px;
  }
  100% {
    top: 200px;
  }
}
@-webkit-keyframes move_in {
  0% {
    top: -200px;
  }
  100% {
    top: 47px;
  }
}
@keyframes move_in {
  0% {
    top: -200px;
  }
  100% {
    top: 47px;
  }
}
@-webkit-keyframes bump {
  0% {
    top: 16px;
  }
  25% {
    top: 13px;
  }
  50% {
    top: 16px;
  }
  75% {
    top: 13px;
  }
  100% {
    top: 16px;
  }
}
@keyframes bump {
  0% {
    top: 16px;
  }
  25% {
    top: 13px;
  }
  50% {
    top: 16px;
  }
  75% {
    top: 13px;
  }
  100% {
    top: 16px;
  }
}
@-webkit-keyframes intro {
  0% {
    -webkit-transform: translateY(-50%) scale(0) rotateX(10deg) rotateY(10deg) rotateZ(40deg);
            transform: translateY(-50%) scale(0) rotateX(10deg) rotateY(10deg) rotateZ(40deg);
  }
  100% {
    -webkit-transform: translateY(-50%) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg);
            transform: translateY(-50%) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  }
}
@keyframes intro {
  0% {
    -webkit-transform: translateY(-50%) scale(0) rotateX(10deg) rotateY(10deg) rotateZ(40deg);
            transform: translateY(-50%) scale(0) rotateX(10deg) rotateY(10deg) rotateZ(40deg);
  }
  100% {
    -webkit-transform: translateY(-50%) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg);
            transform: translateY(-50%) scale(1) rotateX(0deg) rotateY(0deg) rotateZ(0deg);
  }
}
```


