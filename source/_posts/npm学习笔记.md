---
title: npm学习速记
date: 2016-04-20
tags: web开发
categories: web开发
---

{% blockquote %}
本着夯实基础是关键的理念，所以之前一直潜心专研html5，css3和js。对于日新月异的前端工程化构建工具一直没去关注，现阶段，在我计划的学习路径中开始了解并且学习前端工程化思路和实现。npm，gulp，git，webpack和vue将是我后面学习的方向。
{% endblockquote %}

{% asset_img npm.png %}
<!--more-->

---------------------------------------------------------------------------------------------------



## <center>npm简介</center> 
npm 是 2009 年开始的一个 javascript模块管理工具，也是最流行的代码共享平台之一。2013 年 npm<!--more--> 的模块总数是 4 万，2014 年就升到 8 万以上，超过所有其他同类平台。今年 4 月npm官方（http://blog.npmjs.org/post/143451680695/how-many-npm-users-are-there ）发了一份统计，截止 4 月全球估计有 4 百万用户使用 npm，并且这个数字每年会翻一倍。NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上
很多问题，常见的使用场景有以下几种：
  
* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

npm不需要单独安装。在安装Node的时候，会连带一起安装npm。安装doe移步至[node官网](https://nodejs.org/en/download/)。另外，Node附带的npm如果不是最新版本，可以用下面的命令，更新到最新版本
```javascript
$ npm install npm@latest -g
```

---------------------------------------------------------------


## <center>npm命令大全</center> 
### 2.1简单查询

>查看 npm 命令列表
```javascript
$ npm help
```
>查看各个命令的简单用法
```javascript
$ npm -l 
```
>查看当前npm 的版本
```javascript
$ npm -v
```
>查看 npm 的配置
```javascript
$ npm config list -l
```
>以树型结构列出当前项目安装的所有模块，以及它们依赖的模块
```javascript
$ npm ls
```
>列出所有全局安装的模块
```javascript
$ npm ls -g
```
>全局和本地的依赖包的安装路径
```javascript
$ npm root - g 
$ npm root
```
>查看全局或者本地的依赖包
```javascript
$ npm list -g 
$ npm list
```
>查看依赖包的package.json 的信息，也可以单独查找某一个配置项
```javascript
$ npm view <package name> 
$ npm view <package name> dependencies  //查看gulp包的依赖关系
$ npm view <package name> repository.url  //查看gulp包的源文件地址
```
>搜索远程资源库中的依赖包，当在需要发布一个依赖包的时候，可以用这个命令。查找是否已有依赖包
```javascript
$ npm search <package name> 
```
>查看某个包的信息
```javascript
$ npm info <package name>
```





### 2.2  npm init命令
#### 简单例子
npm init用来初始化生成一个新的package.json文件,package.json是基于nodejs项目必不可少的配置文件，它是存放在项目根目录的普通json文件.它会引导用户设置一系列配置，如果你觉得不用修改默认配置，一路回车就可以了。
<span style="color:#931F1D">(注意：json文件内是不能写注释的，下面代码的注释是方便学习的笔记，一个最简单的例子)</span>

```json
{
  "name": "test",   //项目名称（必须）
  "version": "1.0.0",   //项目版本（必须）
  "description": "This is for study npm project !",   //项目描述（必须）
  "homepage": "",   //项目主页
  "repository": {    //项目资源库
    "type": "git",
    "url": "https://git.oschina.net/xxxx"
  },
  "author": {    //项目作者信息
    "name": "oysun",
    "email": "web-oysun@qq.com"
  },
  "license": "ISC",    //项目许可协议
  "devDependencies": {    //项目依赖的插件
    "gulp": "^3.8.11",
    "gulp-less": "^3.0.0"
  }
}
```
#### Package.json 属性说明
`name`
* 在package.json中最重要的就是name和version字段。他们都是必须的，如果没有就无法install。name和version一起组成的标识在假设中是唯一的。改变包应该同时改变version
* 这个名字可能会作为参数被传入require()，所以它应该比较短，但也要意义清晰
* name必须小于等于214个字节，包括前缀名称在内（如xxx/xxxmodule）
* name不能以"_"或"."开头
* 不能含有大写字母
* name会成为url的一部分，不能含有url非法字符
* 创建一个模块前可以先到后边的网址查查name是否已经被占用.https://www.npmjs.com/


`version`
* version必须可以被npm依赖的一个node-semver模块解析。具体规则见下面的dependencies模块

`description` 
* 一个描述，方便别人了解你的模块作用，搜索的时候也有用

`homepage`
* 这个项目主页url和url属性不同，如果你填写了url属性，npm注册工具会认为你把项目发布到其他地方了，获取模块的时候不会从npm官方仓库获取，而是会重定向到url属性配置的地址

`keywords`
* 一个字符串数组，方便别人搜索到本模块

`author`
* 包的作者姓名

`bugs`
* 你项目的提交问题的url和（或）邮件地址。这对遇到问题的屌丝很有帮助。例子：
```json
{ "url" : "http://github.com/owner/project/issues",
  "email" : "project@hostname.com"
}
```
* 你可以指定一个或者指定两个。如果你只想提供一个url，那就不用对象了，字符串就行。
如果提供了url，它会被npm bugs命令使用。

`license`
* 指定一个许可证，让人知道使用的权利和限制的。最简单的方法是，假如你用一个像BSD或者MIT这样通用的许可证，就只需要指定一个许可证的名字，像这样：{ "license" : "BSD" }如果你有更复杂的许可条件，或者想要提供给更多地细节，可以这样:
```json
"licenses" : [
  { "type" : "MyLicense"
  , "url" : "http://github.com/owner/project/path/to/license"
  }
]
```
* 你可以在https://spdx.org/licenses/ 这个地址查阅协议列表

`files`
* files是一个包含项目中的文件的数组。如果命名了一个文件夹，那也会包含文件夹中的文件。（除非被其他条件忽略了）你也可以提供一个.npmignore文件，让即使被包含在files字段中得文件被留下。其实就像.`gitignore`一样。
* 
`main`
* main字段配置一个文件名指向模块的入口程序。如果你包的名字叫foo，然后用户require("foo")，main配置的模块的exports对象会被返回。
这应该是一个相对于根目录的文件路径。对大对数模块而言，这个属性更多的是让模块有一个主入口文件，然而很多模块并不写这个属性

bin`
* 很多包都有一个或多个可执行的文件希望被放到PATH中。npm让妈妈再也不用担心了（实际上npm本身也是通过bin属性安装为一个可执行命令的）。要用这个功能，给package.json中的bin字段一个命令名到文件位置的map。初始化的时候npm会将他链接到prefix/bin（全局初始化）或者./node_modules/.bin/（本地初始化）。
比如，npm有：
```json
{ "bin" : { "npm" : "./cli.js" } }
```
* 所以，当你初始化npm，它会创建一个符号链接到cli.js脚本到/usr/local/bin/npm。如果你只有一个可执行文件，并且名字和包名一样。那么你可以只用一个字符串，比如：
```json
{ "name": "my-program"
, "version": "1.2.5"
, "bin": "./path/to/program" }
```
* 结果和这个一样：
```json
{ "name": "my-program"
, "version": "1.2.5"
, "bin" : { "my-program" : "./path/to/program" } }
```
`man`
* 指定一个单一的文件或者一个文件数组供man程序使用。如果只提供一个单一的文件，那么它初始化后就是man<pkgname>的结果，而不管实际的文件名是神马，比如：
```json
{ "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : "./man/doc.1"
}
```

* 这样man foo就可以用到./man/doc.1文件了。如果文件名不是以包开头，那么它会被冠以前缀，下面的:
```json
{ "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : [ "./man/foo.1", "./man/bar.1" ]
}
```
* 会为man foo和man foo-bar创建文件。man文件需要以数字结束，然后可选地压缩后以.gz为后缀。The number dictates which man section the file is installed into.
```json
{ "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : [ "./man/foo.1", "./man/foo.2" ]
}
```
* 会为man foo和man 2 foo创建。

`directories`
* CommonJS Packages规范说明了几种方式让你可以用directorieshash标示出包得结构。如果看一下npm's package.json，你会看到有directories标示出doc, lib, and man。
在未来，这个信息可能会被用到。

`directories.lib`
* 告其他人你的库文件夹在哪里。目前没有什么特别的东西需要用到lib文件夹，但确实是重要的元信息。

`directories.bin`
* 如果你指定一个“bin”目录，然后在那个文件夹中得所有文件都会被当做"bin"字段使用。如果你已经指定了“bin”字段，那这个就无效。

`directories.man`
* 一个放满man页面的文件夹。贴心地创建一个“man”字段。

`directories.doc`
* 在这个目录里边放一些markdown文件，可能最终有一天它们会被友好的展现出来（应该是在npm的网站上） 我要说话

`directories.example`

* 放一些示例脚本，或许某一天会有用

`repository`
* 指定你的代码存放的地方。这个对希望贡献的人有帮助。如果git仓库在github上，那么npm docs命令能找到你。这样做：
```json
"repository" :
  { "type" : "git"
  , "url" : "http://github.com/isaacs/npm.git"
  }

"repository" :
  { "type" : "svn"
  , "url" : "http://v8.googlecode.com/svn/trunk/"
  }
```
`scripts`
* scripts属性是一个对象，里边指定了项目的生命周期个各个环节需要执行的命令。key是生命周期中的事件，value是要执行的命令。具体的内容有 install start stop 等，详见: https://docs.npmjs.com/misc/scripts

`config`
* 用来设置一些项目不怎么变化的项目配置，例如port等。用户用的时候可以使用如下用法：
```json
http.createServer(...).listen(process.env.npm_package_config_port)
```
* 可以通过npm config set foo:port 80来修改config。详见 https://docs.npmjs.com/misc/config

`dependencies`
* dependencies属性是一个对象，配置模块依赖的模块列表，key是模块名称，value是版本范围，版本范围是一个字符，可以被一个或多个空格分割。dependencies也可以被指定为一个git地址或者一个压缩包地址。不要把测试工具或transpilers写到dependencies中。 下面是一些写法，详见https://docs.npmjs.com/misc/semver

`devDependencies`
* 如果有人想要下载并使用你的模块，也许他们并不希望或需要下载一些你在开发过程中使用的额外的测试或者文档框架。在这种情况下，最好的方法是把这些依赖添加到devDependencies属性的对象中。这些模块会在npm link或者npm install的时候被安装，也可以像其他npm配置一样被管理，详见npm的config文档。对于一些跨平台的构建任务，例如把CoffeeScript编译成JavaScript，就可以通过在package.json的script属性里边配置prepublish脚本来完成这个任务，然后需要依赖的coffee-script模块就写在devDependencies属性种。例如:
```json
{ "name": "ethopia-waza",
  "description": "a delightfully fruity coffee varietal",
  "version": "1.2.3",
  "devDependencies": {
    "coffee-script": "~1.6.3"
  },
  "scripts": {
    "prepublish": "coffee -o lib/ -c src/waza.coffee"
  },
  "main": "lib/waza.js"
}
```

### 2.3 包安装命令

npm的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已.
* 但是代码中，直接通过require()的方式是没有办法调用全局安装的包的。全局的安装是供命令行使用的。
* 每个模块可以“全局安装”，也可以“本地安装”。“全局安装”指的是将一个模块安装到系统目录中，各个项目都可以调用。一般来说，全局安装只适用于工具模块，比如eslint和gulp。“本地安装”指的是将一个模块下载到当前项目的node_modules子目录，然后只有在项目目录之中，才能调用这个模块
 
>本地安装
```javascript
$ npm install <package name>
```

>全局安装
```javascript
$ npm install <package name> -global
$ npm install <package name> -g
```
>npm install也支持直接输入Github代码库地址
```javascript
$ npm install git://github.com/package/path.git
$ npm install git://github.com/package/path.git#0.1.0
```
>install命令总是安装模块的最新版本，如果要安装模块的特定版本，可以在模块名后面加上@和版本号。
```javascript
$ npm install <package name>@latest
$ npm install <package name>@0.1.1
$ npm install <package name>@">=1.9.0 <3.1.0"
```
>如果使用--save-exact参数，会在package.json文件指定安装模块的确切版本
```javascript
$ npm install <package name> --save-exact 
$ npm install <package name> -E
```

>安装包信息将加入到dependencies（生产阶段的依赖）
```javascript
$ npm install <package name> --save 
$ npm install <package name> -S
```
>安装包信息将加入到devDependencies（开发阶段的依赖），所以开发阶段一般使用它
```javascript
$ npm install <package name> --save-dev 
$ npm install <package name> -D
```
*install命令可以使用不同参数，指定所安装的模块属于哪一种性质的依赖关系，即出现在packages.json文件的哪一项中*

>>–save：模块名将被添加到dependencies，可以简化为参数-S。
>>–save-dev: 模块名将被添加到devDependencies，可以简化为参数-D

<span style="color:#931F1D;background-color:#fcf8e3;">一旦安装了某个模块，就可以在代码中用require命令加载这个模块。另外，模块的依赖都被写入了package.json文件后，他人打开项目的根目录（项目开源、内部团队合作），使用npm install命令可以根据dependencies配置安装所有的依赖包</span>

### 2.4 包移除，更新插件命令

>卸载已安装的模块
```javascript
$ npm uninstall <package name>
```
>检查本地有哪些本地包,列出需要更新的包的信息，需要更新的包的名称、当前版本号、最新的版本号等
```javascript
$ npm outdated
```

>更新本地安装的某个模块和全局模块
```javascript
$ npm update <package name>
$ npm update  <package name> -g
```
>全部更新
```javascript
$ npm update
$ npm update -g
```
### 2.5 高级命令 

后续深入学习补充

### 2.6 其它

后续深入学习补充

------------------------------------------------------------

## <center>npm使用技巧</center>

【1】国内访问外网都很慢，甚至不能访问！大家都懂，都很无奈！
使用淘宝的npm国内镜像可以极大的提高包下载的速度
```javascript
$ npm config set strict-ssl false //取消ssl验证
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
````
【2】windows下无法直接删除node_modules，因为node_modules内部嵌套的子目录太多使用rimraf插件可以很好的解决问题
```javascript   
$ npm install rimraf -g
$ rimraf node_modules
```
【3】npm默认将全局包和缓存文件放在C盘。如果想更改路劲可以这样设置：
```javascript
$ npm config set prefix "d:x\node"//修改nodejs全局包的安装路径
$ npm config set cache "D:\nodejs\npm-cache" //修改缓存路径
```
<span style="color:#931F1D;background-color:#fcf8e3;">注意：由于改变了module的默认地址，所以上面的用户变量都要跟着改变一下“用户变量"PATH"修改为你重新设置的路径，要不，使用module的时候会导致输入命令出现“xxx不是内部或外部命令，也不是可运行的程序或批处理文件”这个错误。</span>



