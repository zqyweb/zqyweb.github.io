---
title: JS基础知识
date: 2019-09-15 14:11:11
tags: JS
---

# get 和 post的区别
![ngCI6x.png](https://s2.ax1x.com/2019/09/15/ngCI6x.png)
![ngi92R.png](https://s2.ax1x.com/2019/09/15/ngi92R.png)(https://imgchr.com/i/ngi92R)

# 闭包
闭包是指有权访问另一个函数作用域中的变量的函数。创建方式即在一个函数内部创建另一个函数。
```
function outer(){
        var i=0;
        return function(){
            console.log(i++);
        }
    }
    var getNum = outer();
    getNum();
    i=1;
    getNum();
```
图解：
![nYpOqe.png](https://s2.ax1x.com/2019/09/09/nYpOqe.png)
![nY9FsS.png](https://s2.ax1x.com/2019/09/09/nY9FsS.png)
![nY9nGq.png](https://s2.ax1x.com/2019/09/09/nY9nGq.png)
![nY9cFA.png](https://s2.ax1x.com/2019/09/09/nY9cFA.png)
![nY9WSP.png](https://s2.ax1x.com/2019/09/09/nY9WSP.png)
![nY9fQf.png](https://s2.ax1x.com/2019/09/09/nY9fQf.png)
![nY9Ieg.png](https://s2.ax1x.com/2019/09/09/nY9Ieg.png)
![nY9vOU.png](https://s2.ax1x.com/2019/09/09/nY9vOU.png)
![nYCSw4.png](https://s2.ax1x.com/2019/09/09/nYCSw4.png)
![nYCpTJ.png](https://s2.ax1x.com/2019/09/09/nYCpTJ.png)

## 使用闭包-封装私有变量
```
function Ninja(){
    var feints = 0;
    this.getFeints = function(){
        return feints;
    };
    this.feint = function(){
        feints++;
    };
}

var ninja1 = new Ninja();
ninja1.feint();
console.log(ninja1.feints === undefined,"无法直接获取该变量");
console.log(ninja1.getFeints() === 1,"可以通过getFeints反复操作变量feints");

var ninja2 = new Ninja();
console.log(ninja2.getFeints() === 0,"ninja2具有自己的私有feints变量");
```
## 使用闭包-回调函数
回调函数指的是需要在将来不确定的某一时刻异步调用的函数。
```
<div id="box1">First Box</div>
<script>
  function animateIt(elementId){
      var elem = document.getElementById(elementId);
      var tick = 0;
      var timer = setInterval(function(){
          if(tick <100){
              elem.style.left = elem.style.top = tick + "px";
              tick++;
          }else{
              clearInterval(timer);
              console.log(tick === 100);
              console.log(elem);
              console.log(timer);
          }
      },10);
  }
  animateIt("box1");
</script>
```
## 闭包的缺点

# 对象的创建方式
## 字面量
## Object构造函数
缺点：虽然构造函数或字面量都可以用来创建单个对象，明显的缺点是使用同一个接口创建很多对象，会产生大量的重复代码。为解决这个问题，使用工厂模式。
## 工厂模式
```
function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        console.log(this.name);
    };
    return o;
}
var person1 = createPerson("Nicholas",29,"Software Engineer");
var person2 = createPerson("Greg",27,"Docror"); 
```
## 构造函数模式
```
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        console.log(this.name);
    }
}
var person1 = new Person("Nicholas",29,"Software Engineer");
var person2 = new Person("Greg",27,"Doctor");
```
new一个对象会有哪几步：
（1）创建一个新对象
（2）将构造函数的作用域赋给新对象（只要使用构造函数创建一个子对象时，都会让子对象自动继承构造函数的原型对象）
（3）执行构造函数中的代码（为这个新对象添加属性）
（4）返回新对象地址，保存到变量中

构造函数的问题：每个方法都要在每个实例上重新创建一遍。
解决：原型模式

## 原型模式
prototype通过调用构造函数而创建的那个对象实例的原型对象。使用原型对象的好处是可以让所有对象shilling共享它所包含的属性和方法。

构造函数与原型对象：只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype属性，这个属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个 constructor（构造函数）属性，这个属性包含一个指向 prototype 属性所在函数的指针。

实例与原型对象：当调用构造函数创建一个实例后，该实例的内部指针__proto__指向构造函数的原型对象，这个属性对脚本是完全不可见的。


## 组合使用构造函数模式和原型模式

## 动态原型模式

## 寄生构造函数模式

## 稳妥构造函数模式


# 继承
## 原型链
## 借用构造函数
## 组合继承
## 原型式继承
## 寄生式继承
## 寄生组合式继承

# 如何解决异步回调地狱

## Promise
## Generator
## async/await

# 前端事件流
事件流是指从页面中接收事件的顺序。
IE的事件流是事件冒泡流。
Netscape的事件流是指事件捕获流。
## IE事件流
IE事件流叫事件冒泡。即事件开始由最具体的元素接收，然后逐级向上传播到较为不具体的节点。
![ngG1OK.png](https://s2.ax1x.com/2019/09/15/ngG1OK.png)

所有现代浏览器都支持事件冒泡，但是具体实现上还有一些差别。

## Netscape事件捕获
Netscape Communicator团队提出的另一种事件流叫做事件捕获（event capturing）。事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。如果仍以前面的 HTML 页面作为演示事件捕获的例子，那么单击 <div>元素就会以下列顺序触发 click 事件。
![ngGX11.png](https://s2.ax1x.com/2019/09/15/ngGX11.png)

由于老版本的浏览器不支持，因此很少人使用时间捕获。

## DOM级事件
“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段，处于目标阶段和事件冒泡阶段。
![ngJ7KP.png](https://s2.ax1x.com/2019/09/15/ngJ7KP.png)

多数支持DOM事件流的浏览器都实现了一种特定的行为：即使“DOM2级事件”规范明确要求捕获阶段不会涉及事件目标，但IE9,Safari,Chrome,Firefox和Opera 9.5及更高版本都会在捕获阶段触发事件对象上的事件。

# 事件处理程序
事件是用户或浏览器自身执行的某种动作，而响应某个事件的函数叫做事件处理程序。（事件侦听器）
## HTML事件处理程序
## DOM0级事件处理程序
## DOM2级事件处理程序
## IE事件处理程序
## 跨浏览器的事件处理程序



# 如何让事件先冒泡后捕获
在DOM0标准事件模型中，是先捕获后冒泡。但是如果要实现先冒泡后捕获的效果，对于同一个事件，监听捕获和冒泡，分别对应相应的处理函数，监听到捕获事件，先暂缓执行，直到冒泡事件被捕获后再执行捕获之间。

# 事件委托
事件委托是指不在事件的发生地上设置监听函数，而是在其父元素上设置监听函数，通过事件冒泡，父元素可以监听到子元素上事件的触发，通过判断事件发生元素DOM的类型，来做出不同的响应。

对“事件处理程序过多”问题的解决方案就是事件委托。
事件委托利用了事件冒泡，只指定一个事件处理程序，就可以管理某一类型的所有事件。

优点：
（1）document对象很快可以访问，而且可以在页面生命周期的任何时点上为它添加事件处理程序。即只要可单击的元素呈现在页面上，就可以立即具备适当的功能。
（2）在页面中设置事件处理程序所需要的时间更少。只添加一个事件处理程序所需的DOM引用更少，所花的时间也更少。
（3）整个页面占用的内存空间更少，能够提升整体性能。

# 图片懒加载和预加载
预加载：提前加载图片，当用户需要查看时可直接从本地缓存中渲染。
懒加载：主要目的是作为服务器前端优化，减少请求数或延迟请求数。
本质：两者的行为是相反的，一个是提前加载，一个是迟缓甚至不加载。
懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。
## 图片预加载
### 方法一：用CSS实现预加载
```
#preload-01 {background: url(http://domain.tld/images-01.png) no-repeat -9999px -9999px;}
#preload-02 {background: url(http://domain.tld/images-02.png) no-repeat -9999px -9999px;}
#preload-03 {background: url(http://domain.tld/images-03.png) no-repeat -9999px -9999px;}
```
通过background属性将图片预加载到屏幕外的背景上。缺点是：该方法加载的图片会同页面的其他内容一起加载，增加了页面的整体加载时间。为了解决这个问题，增加JS代码，来推迟预加载的事件，直到页面加载完毕。
### 方法二：
```
function preloader(){
    if(document.getElementById){
        document.getElementById("preload-01").style.background = "url(http://domain.tld/images-01.png) no-repeat -9999px -9999px;";
        document.getElementById("preload-02").style.background = "url(http://domain.tld/images-02.png) no-repeat -9999px -9999px;";
        document.getElementById("preload-03").style.background = "url(http://domain.tld/images-03.png) no-repeat -9999px -9999px;";
    }
}
function addLoadEvent(func){
    var oldonload = window.onload;
    if(typeof window.onload != 'function'){
        window.onload = func;
    }else{
        window.onload = function(){
            if(oldonload){
                oldonload();
            }
            func();
        }
    }
}
addLoadEvent(preloader);
```

### 方法三：使用javascript实现
```
function preloader(){
    if(document.images){
        var img1 = new Image();
        var img2 = new Image();
        var img3 = new Image();
        img1.src = "http://domain.tld/path/to/image-001.gif";
        img2.src = "http://domain.tld/path/to/image-002.gif";
        img3.src = "http://domain.tld/path/to/image-003.gif";
    }
}
function addLoadEvent(func){
    var oldonload = window.onload;
    if(typeof window.onload != 'function'){
        window.onload = func;
    }else{
        window.onload  = function(){
            if(oldonload){
                oldonload();
            }
            func();
        }
    }
}
addLoadEvent(preloader);
```
### 方法四：使用Ajax实现预加载
```
window.onload = function(){
    setTimeout(function(){
        var xhr = new XMLHttpRequest();
        xhr.open('GET','http://domain.tld/preload.js');
        xhr.send('');
        xhr = new XMLHttpRequest();
        xhr.open('GET','http://domain.tld/preload.css');
        xhr.senf('');
        new Image().src = "http://domain.tld/preload.png";
    },1000);
}



window.onload = function() {
 
    setTimeout(function() {
 
        // reference to <head>
        var head = document.getElementsByTagName('head')[0];
 
        // a new CSS
        var css = document.createElement('link');
        css.type = "text/css";
        css.rel  = "stylesheet";
        css.href = "http://domain.tld/preload.css";
 
        // a new JS
        var js  = document.createElement("script");
        js.type = "text/javascript";
        js.src  = "http://domain.tld/preload.js";
 
        // preload JS and CSS
        head.appendChild(css);
        head.appendChild(js);
 
        // preload image
        new Image().src = "http://domain.tld/preload.png";
 
    }, 1000);
 
};
```
## 图片懒加载实现
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .gallery{
            text-align: center;
        }
        img{
            width:327px;
            height:275px;
        }
    </style>
</head>
<body>
    <div class="gallery">
        <img src="./img/0.jpg" data-src="./img/1.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/2.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/4.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/5.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/6.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/7.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/8.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/1.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/2.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/4.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/5.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/6.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/7.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/8.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/1.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/2.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/4.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/5.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/6.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/7.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/8.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/1.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/2.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/4.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/5.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/6.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/7.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/8.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/1.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/2.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/4.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/5.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/6.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/7.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/8.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/1.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/2.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/4.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/5.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/6.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/7.jpg" alt="">
        <img src="./img/0.jpg" data-src="./img/8.jpg" alt="">
    </div>
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>

    <script>
    /*判断当前图片是否是在可视区域*/
    $(window).scroll(handleScroll);
    function handleScroll(){
        let imgs = $('img[data-src]');
        let bodyScrollHeight = document.scrollTop || document.documentElement.scrollTop;//获取网页卷去的高度，即网页滚动的位置
        let windowHeight = window.innerHeight;  //当前视口的高度
        for(let i=0;i<imgs.length;i++){
            let imgHeight = $(imgs[i]).offset().top;//每一张图片距离顶部的坐标
            console.log(imgHeight);
            //当前图片距离页面顶部的距离 < 视口高度+卷去的高度
            if(imgHeight<windowHeight+bodyScrollHeight && imgHeight>=bodyScrollHeight){
                setTimeout(function() {
                    $(imgs).eq(i).attr('src',$(imgs).eq(i).attr('data-src'));
                },1000)
            }
        }
    }
    </script>
</body>
</html>
```
# mouseover和mouseenter的区别
mouseover：当鼠标移入元素或其子元素都会触发事件，所以有一个重复触发，冒泡过程。对应的移除事件是mouseout.(不论鼠标指针穿过被选元素或其子元素，都会触发mouseover事件)
mouseenter:当事件移除元素本身（不包含元素的子元素）会触发事件，也就是不会冒泡，对应的移除事件是mouseleave.（只有在鼠标指针穿过被选元素时，才会触发mouseenter事件）

# this
## 全局作用域或者普通函数中this指向全局对象window
```
//直接打印
console.log(this) //window

//function声明函数
function bar () {console.log(this)}
bar() //window

//function声明函数赋给变量
var bar = function () {console.log(this)}
bar() //window

//自执行函数
(function () {console.log(this)})(); //window
```
## 方法调用中谁调用this指向谁
```
//对象方法调用
var person = {
  run: function () {console.log(this)}
}
person.run() // person

//事件绑定
var btn = document.querySelector("button")
btn.onclick = function () {
  console.log(this) // btn
}
//事件监听
var btn = document.querySelector("button")
btn.addEventListener('click', function () {
  console.log(this) //btn
})

//jquery的ajax
$.ajax({
  self: this,
  type: "get",
  url: url,
  async: true,
  success: function (res) {
    console.log(this) // this指向传入$.ajxa()中的对象
    console.log(self) // window
  }
});
//这里说明以下，将代码简写为$.ajax（obj） ，this指向obj,在obj中this指向window，因为在在success方法中，独享obj调用自己，所以this指向obj
```
## 在构造函数或构造函数原型对象中this指向构造函数的实例
```
//不使用new指向window
function Person(name) {
  console.log(this) // window
  this.name = name;
}
Person('inwe')
//使用new
function Person(name) {
  this.name = name
  console.log(this) //people
  self = this
}
var people = new Person('iwen')
console.log(self === people) //true
//这里new改变了this指向，将this由window指向Person的实例对象people
```
### 箭头函数中指向外层作用域的this
```
var obj = {
  foo() {
    console.log(this);
  },
  bar: () => {
    console.log(this);
  }
}

obj.foo() // {foo: ƒ, bar: ƒ}
obj.bar() // window
```
# 改变函数内部this指针的指向函数
通过apply和call改变函数的this指向，他们两个函数的第一个参数都是一样的表示要改变指向的那个对象;第二个是传入的参数，apply是数组，而call则是arg1,arg2…这种形式。
通过bind改变this作用域会返回一个新的函数（我理解的是bind不改变原函数，再var一个新的函数拿来用。），这个函数不会马上执行。call 是把第二个及以后的参数作为 func 方法的实参传进去，而 func1 方法的实参实则是在 bind 中参数的基础上再往后排。

总结：
当我们使用一个函数需要改变this指向的时候才会用到callapplybind
如果你要传递的参数不多，则可以使用fn.call(thisObj, arg1, arg2 …)
如果你要传递的参数很多，则可以用数组将参数整理好调用fn.apply(thisObj, [arg1, arg2 …])
如果你想生成一个新的函数长期绑定某个函数给某个对象使用，则可以使用const newFn = fn.bind(thisObj); newFn(arg1, arg2…)
call和apply第一个参数为null/undefined，函数this指向全局对象，在浏览器中是window，在node中是global

## 手写实现apply,call,bind
```
Function.prototype.bind = function(context, ...bindArgs) {
  // func 为调用 bind 的原函数
  const func = this;
  context = context || window;
  
  if (typeof func !== 'function') {
    throw new TypeError('Bind must be called on a function');
  }
  // bind 返回一个绑定 this 的函数
  return function(...callArgs) {
    let args = bindArgs.concat(callArgs);
    if (this instanceof func) {
      // 意味着是通过 new 调用的 而 new 的优先级高于 bind
      return new func(...args);
    }
    return func.call(context, ...args);
  }
}

// 通过隐式绑定实现
Function.prototype.call = function(context, ...args) {
  context = context || window;
  context.func = this;

  if (typeof context.func !== 'function') {
    throw new TypeError('call must be called on a function');
  }

  let res = context.func(...args);
  delete context.func;
  return res;
}

Function.prototype.apply = function(context, args) {
  context = context || window;
  context.func = this;

  if (typeof context.func !== 'function') {
    throw new TypeError('apply must be called on a function');
  }

  let res = context.func(...args);
  delete context.func;
  return res;
}

作者：我不吃饼干呀
链接：https://juejin.im/post/5c9edb066fb9a05e267026dc
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
# JS的各种位置
clientHeight:表示可视区的高度，不包含border和滚动条
offsetHeight:表示可视区的高度，包含了border和滚动条
scrollHeight:表示所有区域的高度，包含了因为滚动被隐藏的部分。
clientTop:表示边框border的厚度，在未指定的情况下一般为0.
scrollTop:滚动后被隐藏的高度，获取对象相当于由offsetParent属性指定的父坐标

![nWHjds.gif](https://s2.ax1x.com/2019/09/16/nWHjds.gif)



# 合并对象
# 深拷贝和浅拷贝
# 