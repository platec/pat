# pat

pat是一个轻量级的指令型模板解决方案。具备局部刷新，双向绑定，指令扩展，filter过滤等功能。

如果你用过angular或者vue应该对指令（directive）这个概念不会陌生,angular大而全，vue精简了一部分留下了组件，而pat在angular，react，vue的基础上，结合各自的特点专注于解决模板层面的问题。

与目前主要的框架相比，pat具有以下特点：

* 单一职责，pat只负责解决模板问题。使用者可以单独使用，也可以跟任何其他框架结合使用。
* 支持mustache风格的模板语法，很好的避免了指令写多了模板可读性差的问题。
* 具有指令型框架的特点，扩展性强，功能强大，可以扩展自己的指令。同时支持filter与自定义watcher。
* 具有virtual dom中间层，一方面加快了分析指令的速度，另一方面也为服务端渲染提供了可能。
* 同时支持脏检测与defineproperty的数据检测机制。并且都是支持ie8的。在defineproperty模式下使用vbscript来做兼容处理。


## usage


直接使用线上地址：

压缩版本：

```
http://g.alicdn.com/mm/pat/1.0/pat-min.js
```
未压缩版本：

```
http://g.alicdn.com/mm/pat/1.0/pat.js
```



## Getting Stared


### hello world

我们来看下最简单的例子：


```js

html:
<div id="test">{{text}}</div>

js:
var p = new Pat({
  el:'test',
  data:{
    text:'hello world'
  }
})

```
展现出来就是hello world，这是最简单的渲染。这边的`{{}}`称之为插值。默认会转义，如果不想要转义，就像mustache那样使用`{{{}}}`来实现。

> 如react那样，pat会针对每个dom元素加上一个标识id，用于后面局部刷新时去找dom节点。如果是插值渲染的文本节点那么会跟react那样包裹一个span标记。而对于普通文本不会包裹。

### 列表
下面是一个列表渲染的例子，支持mustache风格的语法，我们更推荐使用mustache风格的语法，可读性更好。


```js

html:

<!--指令型语法-->
<!--
<div t-if="item in lists" id="test">{{item.name}}-{{__INDEX__}}</div>
-->

<!--mustache风格语法-->
{{#for(item in lists)}}
<div id="test">{{item.name}}-{{__INDEX__}}</div>
{{/for}}

js:
var p = new Pat({
  el:'test',
  data:{
    lists:[{
        name:'1111'
    },{
        name:'2222'
    }],
    text:'hello world'
  }
})


```

for指令可以使用`__INDEX__`拿到当前的index。

> pat的指令都有一个`t`的前缀，对于 for，if，unless都是一种指令。实际上就连上面的插值也是一种特殊的指令，指令是pat的核心技术。用户也可以扩展自己的指令。

### 局部刷新

pat使局部刷新更加简单，通过操作数据来达到dom的局部更新目的。


```js

html:
<div id="test">{{text}}</div>

js:
var p = new Pat({
  el:'test',
  data:{
    text:'hello world'
  }
})

p.$data.text = "hi world"

```

每一个pat实例都会有一个$data属性，它是pat托管的数据，可以通过修改$data里面的值来达到局部刷新的目的。避免了复杂的dom操作。


> pat同时支持两种数据变化监听机制，默认使用defineproperty机制，使用注入get set的方式监听数据变更之后批量修改。另外也支持脏检测机制，兼容性以及开发体验更好。详情[点此了解]()。




### 双向绑定

与angular，vue一样，用户可以使用`t-model`指令来选择性的使用双向绑定的功能。

```js

html:
<div id="test">{{text}}</div>
<input type="text" value="" t-model="text">

js:
var p = new Pat({
  el:'test',
  data:{
    text:'hello world'
  }
})

```


想要知道更多用法，[请点这里]()。



## test

采用karma + jasmine做功能性测试，后期考虑使用nightwatch或者casper来做ui的e2e测试

```
npm test
```

## build

项目使用webpack打包，在根目录执行：

```
npm run-script build
```

