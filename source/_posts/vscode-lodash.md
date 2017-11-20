---
title: 开发Vscode插件Lodash sinpptes的感想
date: 2016-11-22
tags:  web开发
categories: web开发
---

{% blockquote %}
第一次开发vscode插件，记录一下一些坑，方便以后工作需要继续开发vscode插件做个可以查阅的笔记。
{% endblockquote %}

{% asset_img title.png %}

<!--more-->

------------------------------------------------------------------------------------------




## 实现


{% asset_img demo.gif %}

输入"_"就可以出一大堆Lodash函数提示，找到想要的函数按住tab键就可以了，函数的每个参数都可以用tab键来光标切换。

## 安装

- 在vscode插件市场上安装

 [vscode插件地址](https://marketplace.visualstudio.com/items?itemName=oysun.Lodash)

```javascript
ext install Lodash
```

- 如果网络不好可以用下载里面的.vsix文件本地安装

文件在[github](https://github.com/OYsun/Vscode-Lodash-Snippents)上

{% asset_img install.png %}

## 过程

  用过很多编辑器，像webstorm，sublime 和Atom。webstorm很强大，但是过于笨重，里面内置的很多功能对我来说很鸡肋，还有一点就是巨卡，这点受不了。Atom编辑器无论界面
还是功能都很不错，曾经还一度的折腾过，但是她在打开大项目文件的时候还是比较卡，可气的是在卡的时候输个字符都要延迟好久。至于sublime，就是配置有点麻烦，花点时间还是
可以折腾出自己满意的样子，不过呢，我还是比较喜欢用vscode。尤其是她的“温馨提示”功能很喜欢，能够提示函数参数的数据类型，可以避免一些错误。
  写vscode插件呢首先得要装node，npm这些基础的就不说额。这里说两个写vscode插件必须装的依赖包
  第一个是yo generator-code，得安装到全局（类似生产插件项目文件的脚手架）

  ```javascript
    npm install -g yo generator-code
  ```
  然后输入

  ```javascript
  yo code
  ```
  生成项目文件，至于项目文件的一些解释和教程，参考下面几篇文章：

  {% blockquote %}
  http://www.cnblogs.com/caipeiyu/p/5507252.html
  https://code.visualstudio.com/docs/extensionAPI/activation-events
  https://code.visualstudio.com/docs/extensionAPI/extension-manifest
  {% endblockquote %}

  写完插件呢，怎么使用？有个很low的办法，就是拷贝项目到插件目录，但是这不靠谱吧。所以我们需要一个打包工具叫 vsce 同样的可以用npm来安装，打开cmd执行命令

  ```javascript
      npm install -g vsce
  ```
  安装完这个工具（⊂((・⊥・))⊃）就是开始入坑了！
  要想将自己写的插件发布到vscode市场上，你还得注册微软帐号拿到token才行，拿到token后在自己的插件文件目录下,创建自己的publisher，注意这个publisher要和你插件的publisher一致

  ```javascript
    vsce create-publisher (publisher name)
  ```

  然后就可以publish到vscode插件市场上，当然这最重要的就是输入你得token

  ```javascript
    vsce publish -p <token>
  ```

  如果你不想发布到vscode插件市场上，只想自己用或者给自己团队用，可以package打包成.visx.文件在本地安装

  ```javascript
    vsce package
  ```