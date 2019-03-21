结构
概述
一般来讲，你组织声明文件的方式取决于库是如何被使用的。 在JavaScript中一个库有很多使用方式，这就需要你书写声明文件去匹配它们。 这篇指南涵盖了如何识别常见库的模式，和怎样书写符合相应模式的声明文件。

针对每种主要的库的组织模式，在模版一节都有对应的文件。 你可以利用它们帮助你快速上手。

识别库的类型
首先，我们先看一下TypeScript声明文件能够表示的库的类型。 这里会简单展示每种类型的库的使用方式，如何去书写，还有一些真实案例。

识别库的类型是书写声明文件的第一步。 我们将会给出一些提示，关于怎样通过库的 使用方法及其源码来识别库的类型。 根据库的文档及组织结构不同，这两种方式可能一个会比另外的那个简单一些。 我们推荐你使用任意你喜欢的方式。

全局库
全局库是指能在全局命名空间下访问的（例如：不需要使用任何形式的import）。 许多库都是简单的暴露出一个或多个全局变量。 比如，如果你使用过 jQuery，$变量可以被够简单的引用：

$(() => { console.log('hello!'); } );
你经常会在全局库的指南文档上看到如何在HTML里用脚本标签引用库：

<script src="http://a.great.cdn.for/someLib.js"></script>
目前，大多数流行的全局访问型库实际上都以UMD库的形式进行书写（见后文）。 UMD库的文档很难与全局库文档两者之间难以区分。 在书写全局声明文件前，一定要确认一下库是否真的不是UMD。

从代码上识别全局库
全局库的代码通常都十分简单。 一个全局的“Hello, world”库可能是这样的：

function createGreeting(s) {
    return "Hello, " + s;
}
或这样：

window.createGreeting = function(s) {
    return "Hello, " + s;
}
当你查看全局库的源代码时，你通常会看到：

顶级的var语句或function声明
一个或多个赋值语句到window.someName
假设DOM原始值像document或window是存在的
你不会看到：

检查是否使用或如何使用模块加载器，比如require或define
CommonJS/Node.js风格的导入如var fs = require("fs");
define(...)调用
文档里说明了如何去require或导入这个库
全局库的例子
由于把一个全局库转变成UMD库是非常容易的，所以很少流行的库还再使用全局的风格。 然而，小型的且需要DOM（或 没有依赖）的库可能还是全局类型的。

全局库模版
模版文件global.d.ts定义了myLib库作为例子。 一定要阅读 "防止命名冲突"补充说明。

模块化库
一些库只能工作在模块加载器的环境下。 比如，像 express只能在Node.js里工作所以必须使用CommonJS的require函数加载。

ECMAScript 2015（也就是ES2015，ECMAScript 6或ES6），CommonJS和RequireJS具有相似的导入一个模块的表示方法。 例如，对于JavaScript CommonJS （Node.js），有下面的代码

var fs = require("fs");
对于TypeScript或ES6，import关键字也具有相同的作用：

import fs = require("fs");
你通常会在模块化库的文档里看到如下说明：

var someLib = require('someLib');
或

define(..., ['someLib'], function(someLib) {

});
与全局模块一样，你也可能会在UMD模块的文档里看到这些例子，因此要仔细查看源码和文档。