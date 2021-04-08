JS

## 1.懒加载

1.什么是懒加载？

懒加载也就是延迟加载。
当访问一个页面的时候，先把img元素或是其他元素的背景图片路径替换成一张大小为1*1px图片的路径（这样就只需请求一次，俗称占位图），只有当图片出现在浏览器的可视区域内时，才设置图片正真的路径，让图片显示出来。这就是图片懒加载。

2.为什么要使用懒加载？

很多页面，内容很丰富，页面很长，图片较多。比如说各种商城页面。这些页面图片数量多，而且比较大，少说百来K，多则上兆。要是页面载入就一次性加载完毕。估计大家都会等到黄花变成黄花菜了。

3.懒加载的原理是什么？

页面中的img元素，如果没有src属性，浏览器就不会发出请求去下载图片，只有通过javascript设置了图片路径，浏览器才会发送请求。
懒加载的原理就是先在页面中把所有的图片统一使用一张占位图进行占位，把正真的路径存在元素的“data-url”（这个名字起个自己认识好记的就行）属性里，要用的时候就取出来，再设置；

4.懒加载的实现步骤？

1)首先，不要将图片地址放到src属性中，而是放到其它属性(data-original)中。
2)页面加载完成后，根据scrollTop判断图片是否在用户的视野内，如果在，则将data-original属性中的值取出存放到src属性中。
3)在滚动事件中重复判断图片是否进入视野，如果进入，则将data-original属性中的值取出存放到src属性中。

5.懒加载的优点是什么？

页面加载速度快、可以减轻服务器的压力、节约了流量,用户体验好

```HTML
<p>function lazyload () {
  // 获取所有要进行懒加载的图片
  var eles = document.querySelectorAll(&#39;img[data-original][lazyload]&#39;)
  Array.prototype.forEach.call(eles, function (item, index) {
    var rect
    if (item.dataset.original === &#39;&#39;)
      return
    rect = item.getBoundingClientRect()
    // 图片一进入可视区，动态加载
    if (rect.bottom &gt;= 0 &amp;&amp; rect.top &lt; viewHeight) {
      !function () {
        var img = new Image()
        img.src = item.dataset.original
        img.onload = function () {
          item.src = img.src
        }
        item.removeAttribute(&#39;data-original&#39;)
        item.removeAttribute(&#39;lazyload&#39;)
      }()
    }
  })
}
// 首屏要人为的调用，否则刚进入页面不显示图片
lazyload()</p>
<p>document.addEventListener(&#39;scroll&#39;, lazyload)</p>

```

## 2. Promise

### （1）什么是 Promise

  Promise 是一个对象，从它可以获取异步操作的消息。 是异步编程的一种解决方案

### （2）API

Promise 的常用 API 如下：

- Promise.resolve(value)

> 类方法，该方法返回一个以 value 值解析后的 Promise 对象 1、如果这个值是个 thenable（即带有 then 方法），返回的 Promise 对象会“跟随”这个 thenable 的对象，采用它的最终状态（指 resolved/rejected/pending/settled）
>  2、如果传入的 value 本身就是 Promise 对象，则该对象作为 Promise.resolve 方法的返回值返回。
>  3、其他情况以该值为成功状态返回一个 Promise 对象。

- Promise.reject

> 类方法，且与 resolve 唯一的不同是，返回的 promise 对象的状态为 rejected。

- Promise.prototype.then

> 实例方法，为 Promise 注册回调函数，函数形式：fn(vlaue){}，value 是上一个任务的返回结果，then 中的函数一定要 return 一个结果或者一个新的 Promise 对象，才可以让之后的then 回调接收。

- Promise.prototype.catch

> 实例方法，捕获异常，函数形式：fn(err){}, err 是 catch 注册 之前的回调抛出的异常信息。

- Promise.race

> 类方法，多个 Promise 任务同时执行，返回最先执行结束的 Promise 任务的结果，不管这个 Promise 结果是成功还是失败。 。

- Promise.all

> 类方法，多个 Promise 任务同时执行。
>  如果全部成功执行，则以数组的方式返回所有 Promise 任务的执行结果。 如果有一个 Promise 任务 rejected，则只返回 rejected 任务的结果。

 （3）总结：

一个 Promise 对象有三个状态，并且状态一旦改变，便不能再被更改为其他状态。

- pending，异步任务正在进行。
- resolved (也可以叫fulfilled)，异步任务执行成功。
- rejected，异步任务执行失败。

使用过程：

- 首先初始化一个 Promise 对象，可以通过两种方式创建， 这两种方式都会返回一个 Promise 对象。

  - 1、new Promise(fn)
  - 2、Promise.resolve(fn)

- 然后调用上一步返回的 promise 对象的 then 方法，注册回调函数。

- then 中的回调函数可以有一个参数，也可以不带参数。如果 then 中的回调函数依赖上一步的返回结果，那么要带上参数。比如:

  ```
      new Promise(fn)
      .then(fn1(value）{
          //处理value
      })
  ```

- 最后注册 catch 异常处理函数，处理前面回调中可能抛出的异常。

ES6的 async/await也是基于 Promise 实现的 

##  3.async  和 await

 怎么用的? 实际业务中如何使用它，解决了什么问题?  

async 函数执行的返回结果是一个 promise 对象，函数返回值是这个 promise 状态值 resolve的值，当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句。await 操作符用于等待一个 Promise 对象, 返回 Promise 对象的处理结果，如果等待的不是 Promise 对象，则返回该值本身。 它只能在异步函数 async function 内部使用。 

例如在向后端接口发送请求时，，，

 理解async函数需要先理解Generator函数，因为 `async函数是Generator函数的语法糖` 

Generator是ES6标准引入的新的数据类型。Generator可以理解为一个状态机，内部封装了很多状态，同时返回一个迭代器Iterator对象。可以通过这个迭代器遍历相关的值及状态。 Generator的显著特点是可以多次返回，每次的返回值作为迭代器的一部分保存下来，可以被我们显式调用。

#### Generator函数的声明

一般的函数使用function声明，return作为回调(没有遇到return，在结尾调用return undefined)，只可以回调一次。而Generator函数使用function*定义，除了return，还使用yield返回多次。

Generator函数提供了3个方法，next/return/throw

next方式是按步执行，每次返回一个值,同时也可以每次传入新的值作为上一次yield的返回值

```
function* foo(x) {
    let a = yield x + 1;
    let b= yield a + 2;
    return x + 3;
}
const result = foo(0) // foo {<suspended>}？？？？？？？？？？？？？？？？
result.next(1);  // {value: 1, done: false}
result.next(2);  // {value: 2, done: false}
result.next(3);  // {value: 3, done: true}
result.next(4);  //{value: undefined, done: true}
```

return则直接跳过所有步骤，直接返回 {value: undefined, done: true}

throw则根据函数中书写try catch返回catch中的内容，如果没有写try，则直接抛出异常

##  4.防抖和节流及应用场景？？

手写防抖 !

（1）防抖（debounce）
在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时，重新出发定时器。

当连续触发scroll事件，handle函数只会在1秒时间内执行一次，在如果继续滚动执行，就会清除定时器，重新计时。相当于就是多次执行，只执行一次。

```
<div class="box" id="container">
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
    <p>防抖演示</p>
  </div>

function debounce(fn, wait) {
  var timeout = null;// 使用闭包，缓存变量
  return function() {
        if(timeout !== null) {
          console.log('清除定时器啦')
          clearTimeout(timeout);  //清除这个定时器
        }
        timeout = setTimeout(fn, wait);
    }
  }
  // 处理函数
  function handle() {
      console.log(Math.random());
  }
  var container = document.getElementById('container')
  container.addEventListener('scroll', debounce(handle, 1000));
```

（2）节流（throttle）
规定在一个单位时间内，只能触发一次函数。如果这个单位时间内触发多次函数，只有一次生效。

```
var throttle = function(func, delay) {
    var timer = null; // 使用闭包，缓存变量
    var prev = Date.now(); // 最开始进入滚动的时间
    return function() {
      var context = this;   // this指向window
      var args = arguments;
      var now = Date.now();
      var remain = delay - (now - prev); // 剩余时间
      clearTimeout(timer);
      // 如果剩余时间小于0，就立刻执行
      if (remain <= 0) {
        func.apply(context, args);
        prev = Date.now();
      } else {
        timer = setTimeout(func, remain);
      }
    }
  }
  function handle() {
      console.log(Math.random());
  }
  var container = document.getElementById('container')
  container.addEventListener('scroll', throttle(handle, 1000));

```


在节流函数内部使用开始时间prev、当前时间now和剩余时间remain，当剩余时间小于等于0意味着执行处理函数，这样保证第一次就能立即执行函数并且每隔delay时间执行一次；

如果还没到时间，就会在remaining之后触发，保证最后一次触发事件也能执行函数，如果在remaining时间内又触发了滚动事件，那么会取消当前的计数器并计算出新的remaing时间。

通过时间戳和定时器的方法，我们实现了第一次立即执行，最后一次也执行，规定时间间隔执行的效果。

（3）总结
函数防抖和函数节流都是防止某一时间频繁触发，但是原理却不一样。
防抖是将多次执行变为只执行一次，节流是将多次执行变为每隔一段时间执行。

- 防抖(debounce)


search搜索联想，用户在不断输入值时，用防抖来节约请求资源。

window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次

- 节流(throttle)


鼠标不断点击触发，mousedown(单位时间内只触发一次)

监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断

## 5.堆栈 

栈 (stack) ： 用来保存简单的数据字段

堆 (heap) :    用来保存栈中简单数据字段对指针的引用

基本类型、引用类型数据以及 堆栈的关系如下图：

![img](C:\Users\superssssss\Desktop\Interview\mj-images\watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvbGVpZ2VxdXNoYXdhbnlpZXI=,size_16,color_FFFFFF,t_70) 

如上图所示，栈内存中 关于 引用类型的数据的是通过指针(地址)来引用的，指针和地址指向堆内存中的数据。
为啥为导致上述区别，是因为：

基本类型的数据简单，所占用空间比较小，内存由系统自动分配
引用类型数据比较复杂，复杂程度是动态的，计算机为了较少反复的创建和回收引用类型数据所带来的损耗，就先为其开辟另外一部分空间——及堆内存，以便于这些占用空间较大的数据重复利用。堆内存中的数据不会随着方法的结束立即销毁，有可能该对象会被其它方法所引用，直到系统的垃圾回收机制检索到该对象没有被任何方法所引用的时候才会对其进行回收
原文链接：https://blog.csdn.net/woleigequshawanyier/article/details/85038675

##  6.为什么JavaScript是单线程

JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。那么，为什么JavaScript不能有多个线程呢？这样能提高效率啊。

JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？

所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征，将来也不会改变。

为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。

##  7.Event Loop

(1)JS中的的Event Loop

主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）。 

 ![Event Loop](C:\Users\superssssss\Desktop\Interview\mj-images\bg2014100802.png) 

 上图中，主线程运行的时候，产生堆（heap）和栈（stack），栈中的代码调用各种外部API，它们在"任务队列"中加入各种事件（click，load，done）。只要栈中的代码执行完毕，主线程就会去读取"任务队列"，依次执行那些事件所对应的回调函数。执行栈中的代码（同步任务），总是在读取"任务队列"（异步任务）之前执行

 (2)Node.js的Event Loop

 Node.js也是单线程的Event Loop，但是它的运行机制不同于浏览器环境。 

![Node.js](C:\Users\superssssss\Desktop\Interview\mj-images\bg2014100803.png) 

 根据上图，Node.js的运行机制如下。

> （1）V8引擎解析JavaScript脚本。
>
> （2）解析后的代码，调用Node API。
>
> （3）[libuv库](https://github.com/joyent/libuv)负责Node API的执行。它将不同的任务分配给不同的线程，形成一个Event Loop（事件循环），以异步的方式将任务的执行结果返回给V8引擎。
>
> （4）V8引擎再将结果返回给用户。

除了setTimeout和setInterval这两个方法，Node.js还提供了另外两个与"任务队列"有关的方法：[process.nextTick](http://nodejs.org/docs/latest/api/process.html#process_process_nexttick_callback)和[setImmediate](http://nodejs.org/docs/latest/api/timers.html#timers_setimmediate_callback_arg)。它们可以帮助我们加深对"任务队列"的理解。

process.nextTick方法可以在当前"执行栈"的尾部----下一次Event Loop（主线程读取"任务队列"）之前----触发回调函数。也就是说，它指定的任务总是发生在所有异步任务之前。setImmediate方法则是在当前"任务队列"的尾部添加事件，也就是说，它指定的任务总是在下一次Event Loop时执行，这与setTimeout(fn, 0)很像

 另外，由于process.nextTick指定的回调函数是在本次"事件循环"触发，而setImmediate指定的是在下次"事件循环"触发，所以很显然，前者总是比后者发生得早，而且执行效率也高（因为不用检查"任务队列"） 

## 8.同步和异步 

（1）同步：同步是指一个进程在执行某个请求的时候，如果该请求需要一段时间才能返回信息，那么这个进程会一直等待下去，直到收到返回信息才继续执行下去。

异步：异步是指进程不需要一直等待下去，而是继续执行下面的操作，不管其他进程的状态，当有信息返回的时候会通知进程进行处理，这样就可以提高执行的效率了，即异步是我们发出的一个请求，该请求会在后台自动发出并获取数据，然后对数据进行处理，在此过程中，我们可以继续做其他操作，不管它怎么发出请求，不关心它怎么处理数据。

（2）同步与异步适用的场景  

就算是ajax去局部请求数据，也不一定都是适合使用异步的，比如应用程序往下执行时以来从服务器请求的数据，那么必须等这个数据返回才行，这时必须使用同步。而发送邮件的时候，采用异步发送就可以了，因为不论花了多长时间，对方能收到就好。**总结来说，就是看需要的请求的数据是否是程序继续执行必须依赖的数据**



可以简单地理解为：可以改变程序正常执行顺序的操作就可以看成是异步操作 ，最基础的异步是setTimeout和setInterval函数 

javascript是单线程。所有任务可以分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。

具体来说，异步运行机制如下：

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
（4）主线程不断重复上面的第三步。

"任务队列"是一个事件的队列（也可以理解成消息的队列），IO设备完成一项任务，就在"任务队列"中添加一个事件，表示相关的异步任务可以进入"执行栈"了。主线程读取"任务队列"，就是读取里面有哪些事件。
"任务队列"中的事件，除了IO设备的事件以外，还包括一些用户产生的事件（比如鼠标点击、页面滚动等等），比如$(selectot).click(function)，这些都是相对耗时的操作。只要指定过这些事件的回调函数，这些事件发生时就会进入"任务队列"，等待主线程读取。
所谓"回调函数"（callback），就是那些会被主线程挂起来的代码，前面说的点击事件$(selectot).click(function)中的function就是一个回调函数。异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。例如ajax的success，complete，error也都指定了各自的回调函数，这些函数就会加入“任务队列”中，等待执行。

## 9.闭包 

1.概念：指有权访问另一个函数作用域中变量的函数。 简单理解就是 ，一个作用域可以访问另外一个函数内部的局部变量。 

2.作用：延伸变量的作用范围。

而我们知道，函数的执行上下文，在执行完毕之后，生命周期结束，那么该函数的执行上下文就会失去引用。其占用的内存空间很快就会被垃圾回收器释放。可是闭包的存在，会阻止这一过程。 

3.案例：利用闭包的方式得到当前li 的索引号

```js
for (var i = 0; i < lis.length; i++) {
// 利用for循环创建了4个立即执行函数
// 立即执行函数也成为小闭包因为立即执行函数里面的任何一个函数都可以使用它的i这变量
(function(i) {
    lis[i].onclick = function() {
      console.log(i);
    }
 })(i);
}
```

## 10.正则表达式 ？？（笔记）

## 11.JS实现继承的几种方式及优缺点？

### 1.原型链继承

实现方式：将子类的原型链指向父类的对象实例（**prototype的方式：** ）

```
function Parent(){
  this.name = "parent";
  this.list = ['a'];
}
Parent.prototype.sayHi = function(){
  console.log('hi');
}
function Child(){

}
Child.prototype = new Parent();
var child = new Child();
console.log(child.name);
child.sayHi();
```

原理：子类实例child的`__proto__`指向Child的原型链prototype，而Child.prototype指向Parent类的对象实例，该父类对象实例的`__proto__`指向Parent.prototype,所以Child可继承Parent的构造函数属性、方法和原型链属性、方法
 优点：可继承构造函数的属性，父类构造函数的属性，父类原型的属性
 缺点：无法向父类构造函数传参；且所有实例共享父类实例的属性，若父类共有属性为引用类型，一个子类实例更改父类构造函数共有属性时会导致继承的共有属性发生变化；实例如下：

```
var a = new Child();
var b = new Child();
a.list.push('b');
console.log(b.list); // ['a','b']
```

### 2.构造函数继承

实现方式：在子类构造函数中使用call或者apply劫持父类构造函数方法，并传入参数

```
function Parent(name, id){
  this.id = id;
  this.name = name;
  this.printName = function(){
    console.log(this.name);
  }
}
Parent.prototype.sayName = function(){
  console.log(this.name);
};
function Child(name, id){
  Parent.call(this, name, id);
  // Parent.apply(this, arguments);
}
var child = new Child("jin", "1");
child.printName(); // jin
child.sayName() // Error
```

原理：使用call或者apply更改子类函数的作用域，使this执行父类构造函数，子类因此可以继承父类共有属性
 优点：可解决原型链继承的缺点
 缺点：不可继承父类的原型链方法，构造函数不可复用

### 3.组合继承

原理：综合使用构造函数继承和原型链继承

```
function Parent(name, id){
  this.id = id;
  this.name = name;
  this.list = ['a'];
  this.printName = function(){
    console.log(this.name);
  }
}
Parent.prototype.sayName = function(){
  console.log(this.name);
};
function Child(name, id){
  Parent.call(this, name, id);
  // Parent.apply(this, arguments);
}
Child.prototype = new Parent();
var child = new Child("jin", "1");
child.printName(); // jin
child.sayName() // jin

var a = new Child();
var b = new Child();
a.list.push('b');
console.log(b.list); // ['a']
```

优点：可继承父类原型上的属性，且可传参；每个新实例引入的构造函数是私有的
 缺点：会执行两次父类的构造函数，消耗较大内存，子类的构造函数会代替原型上的那个父类构造函数

##  12.垃圾回收机制  

（1）JavaScript 引擎中有一个后台进程称为[垃圾回收器](https://en.wikipedia.org/wiki/Garbage_collection_%28computer_science%29)，它监视所有对象，并删除那些不可访问的对象。 如果几个对象引用形成一个环，互相引用，但根访问不到它们，这几个对象也是垃圾，也要被清除。 

  **John**对象 变成不可达的状态，没有办法访问它，没有对它的引用。垃圾回收器将丢弃 **John** 数据并释放内存。 

 （2）基本的垃圾回收算法称为**“标记-清除”**，定期执行以下“垃圾回收”步骤:

- 垃圾回收器获取根并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- 除标记的对象外，所有对象都被删除。这就是垃圾收集的工作原理 

##  13.new怎么实现的 

 MDN对new运算符的定义：

new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

new操作符会干下面这些事：

1. 在内存中创建一个新的空对象（即{}）；
2. 让this指向这个新对象；     链接该对象（即设置该对象的构造函数）到另一个对象 ；
3. 执行构造函数里的代码，添加属性和方法 ；
4. 返回新对象。



## 14.基本类型和引用类型区别

**1.基本类型值**指的是那些保存在**栈内存**中的简单数据段，即这种值完全保存在内存中的一个位置。是**按值访问**的，因为我们操作的是它们实际保存的值 

而**引用类型值**则是指那些保存在**堆内存**中的对象，意思是变量中保存的实际上只是一个指针，这个指针指向内存中的另一个位置，该位置保存对象。**按引用访问**，因为我们操作的不是实际的值，而是被那个值所引用的对象。 

另外类的成员变量存储在堆中，包括基础类型和引用类型

方法中的局部变量存储在栈中，但是引用的对象存储在堆中

**2 .复制变量值**

除了保存方式不同之外，在一个变量向另一个变量复制基本类型值和引用类型值时，也存在不同。

如果复制基本类型的值，会在栈中创建一个新值，然后把该值复制到为新变量分配的位置上。两个变量是完全独立的，它们可以参与任何操作而不会相互影响。 

当复制引用类型的值时，同样也会将存储在栈中的值复制一份到为新变量分配的空间中。不同的是，这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象。复制后，两个变量实际上将引用同一个对象。因此改变其中一个变量，就会影响到另一个变量。                所以有了深浅拷贝的问题？

![img](C:\Users\superssssss\Desktop\Interview\mj-images\Center) 



## 15  函数柯里化？ 

就是高阶函数的一个特殊用法  

拿被做了无数次示例的add函数，来做一个简单的实现。

```
// 普通的add函数
function add(x, y) {
    return x + y
}
// Currying后
function curryingAdd(x) {
    return function (y) {
        return x + y
    }
}
add(1, 2)           // 3
curryingAdd(1)(2)   // 3
```

实际上就是把add函数的x，y两个参数变成了先用一个函数接收x然后返回一个函数去处理y参数。现在思路应该就比较清晰了，就是只传递给函数一部分参数来调用它，让它返回一个函数去处理剩下的参数。

首先通过_add可以看出，柯里化函数的运行过程其实是一个参数的收集过程，我们将每一次传入的参数收集起来，并在最里层里面处理。在实现createCurry时，可以借助这个思路来进行封装。 

柯里化确实是把简答的问题复杂化了，但是复杂化的同时，我们使用函数拥有了更加多的自由度。而这里对于函数参数的自由处理，正是柯里化的核心所在。

举一个非常常见的例子。验证一串数字是否是正确的手机号，按照普通的思路来做，大家可能是这样封装，如下：

```
function checkPhone(phoneNumber) {
    return /^1[34578]\d{9}$/.test(phoneNumber);
}
```

而如果想要验证是否是邮箱呢？这么封装：

```
function checkEmail(email) {
    return /^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/.test(email);
}
```

我们还可能会遇到验证身份证号，验证密码等各种验证信息，因此在实践中，为了统一逻辑，我们就会封装一个更为通用的函数，将用于验证的正则与将要被验证的字符串作为参数传入。

```
function check(targetString, reg) {
    return reg.test(targetString);
}
```

但是这样封装之后，在使用时又会稍微麻烦一点，因为会总是输入一串正则，这样就导致了使用时的效率低下。

```
check(/^1[34578]\d{9}$/, '14900000088');
check(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/, 'test@163.com');
```

这个时候，我们就可以借助柯里化，在check的基础上再做一层封装，以简化使用。

```
var _check = createCurry(check);

var checkPhone = _check(/^1[34578]\d{9}$/);
var checkEmail = _check(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/);
```

最后在使用的时候就会变得更加直观与简洁了。

```
checkPhone('183888888');
checkEmail('xxxxx@test.com');
```

## 16 原型 原型链 作用域链 

1.作用域链：只要是代码都一个作用域中，写在函数内部的局部作用域，未写在任何函数内部即在全局作用域中；如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域；！！！！！根据在**[内部函数可以访问外部函数变量]**的这种机制，用链式查找决定哪些数据能被内部函数访问，就称作作用域链

2.构造函数原型prototype

JavaScript 规定，每一个构造函数都有一个prototype 属性，指向另一个对象。注意这个prototype就是一个对象，这个对象的所有属性和方法，都会被构造函数所拥有。

我们可以把那些不变的方法，直接定义在 prototype 对象上，这样所有对象的实例就可以共享这些方法。

对象原型——proto——：对象都会有一个属性 --proto--指向构造函数的 prototype 原型对象，之所以我们对象可以使用构造函数 prototype 原型对象的属性和方法，就是因为对象有 __proto__ 原型的存在。--proto--对象原型和原型对象 prototype 是等价的

3.原型链

​	每一个实例对象又有一个__proto__属性，指向的构造函数的原型对象，构造函数的原型对象也是一个对象，也有__proto__属性，这样一层一层往上找就形成了原型链。

![](C:\Users\superssssss\Desktop\Interview\mj-images\img5.png)

## 17.this指向 

一、this 的指向，是当我们调用函数的时候确定的。调用方式的不同决定了this 的指向不同

1.普通函数：指向window

2.对象的方法：o.sayHai()指向o

3.构造函数里：指向实例对象，原型对象this也是指向实例对象

4。绑定事件函数：函数调用者

5.定时器和立即执行函数： window

二、改变函数内部 this 指向

1. call方法：call()方法调用一个对象。简单理解为调用函数的方式，但是它可以改变函数的 this 指向

应用场景:  经常做继承. 

```js
var o = {
	name: 'andy'
}
 function fn(a, b) {
      console.log(this);
      console.log(a+b)
};
fn(1,2)// 此时的this指向的是window 运行结果为3
fn.call(o,1,2)//此时的this指向的是对象o,参数使用逗号隔开,运行结果为3
```

以上代码运行结果为:

2.apply方法

apply() 方法调用一个函数。简单理解为调用函数的方式，但是它可以改变函数的 this 指向。应用场景:  经常跟数组有关系

```js
var o = {
	name: 'andy'
}
 function fn(a, b) {
      console.log(this);
      console.log(a+b)
};
fn()// 此时的this指向的是window 运行结果为3
fn.apply(o,[1,2])//此时的this指向的是对象o,参数使用数组传递 运行结果为3
```

3 bind方法

bind() 方法不会调用函数,但是能改变函数内部this 指向,返回的是原函数改变this之后产生的新函数。如果只是想改变 this 指向，并且不想调用这个函数的时候，可以使用bind

```js
 var o = {
 name: 'andy'
 };

function fn(a, b) {
	console.log(this);
	console.log(a + b);
};
var f = fn.bind(o, 1, 2); //此处的f是bind返回的新函数
f();//调用新函数  this指向的是对象o 参数使用逗号隔开
```

####  call、apply、bind异同

- 共同点 : 都可以改变this指向
- 不同点:
  - call 和 apply  会调用函数, 并且改变函数内部this指向.
  - call 和 apply传递的参数不一样,call传递参数使用逗号隔开,apply使用数组传递
  - bind  不会调用函数, 可以改变函数内部this指向.

- 应用场景
  1. call 经常做继承. 
  2. apply经常跟数组有关系.  比如借助于数学对象实现数组最大值最小值
  3. bind  不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向.

## .ES6？？？

- [2.1 ES6 let 与 const](http://www.runoob.com/w3cnote/es6-let-const.html)
- [2.2 ES6 解构赋值](http://www.runoob.com/w3cnote/deconstruction-assignment.html)
- [2.3 ES6 Symbol](http://www.runoob.com/w3cnote/es6-symbol.html)
- [3.1.1 ES6 Map 与 Set](http://www.runoob.com/w3cnote/es6-map-set.html)
- [3.1.2 ES6 Reflect 与 Proxy](http://www.runoob.com/w3cnote/es6-reflect-proxy.html)
- [3.2.1 ES6 字符串](http://www.runoob.com/w3cnote/es6-string.html)
- [3.2.2 ES6 数值](http://www.runoob.com/w3cnote/es6-number.html)
- [3.2.3 ES6 对象](http://www.runoob.com/w3cnote/es6-object.html)
- [3.2.4 ES6 数组](http://www.runoob.com/w3cnote/es6-array.html)
- [4.1 ES6 函数](http://www.runoob.com/w3cnote/es6-function.html)
- [4.2 ES6 迭代器](http://www.runoob.com/w3cnote/es6-iterator.html)
- [4.3 ES6 Class 类](http://www.runoob.com/w3cnote/es6-class.html)
- [4.4 ES6 模块](http://www.runoob.com/w3cnote/es6-module.html)
- [5.1 ES6 Promise 对象](http://www.runoob.com/w3cnote/es6-promise.html)
- [5.2 ES6 Generator 函数](http://www.runoob.com/w3cnote/es6-generator.html)
- [5.3 ES6 async 函数](http://www.runoob.com/w3cnote/es6-async.html)

 

 











字符串常用api

数组浅拷贝有哪些方法？

\14. 手动实现一个深拷贝？

\15. 递归深拷贝实现种，循环引用怎么解决？

手撕数组去重 撕了3种方法  

之 前 说了 ES6 se t 可 以 数 组 去 重 ， 是 否 还 有 数 组 去 重 的 方 法
参 考 回 答 ：
法一：indexOf 循环去重
法二：Object 键值对去重；把数组的值存成 Object 的 key 值，比如 Object[value1] = true，
在判断另一个值的时候，如果 Object[value2]存在的话，就说明该值是重复的。



22. [算法题]() 实现 add(1, 2, 3) === add(1)(2)(3) 其实就要用到函数柯里化  

   23. 写一个解析类似 let url = '  <http://www.baidu.com?a=1&b=2>' 这样的函数？

泛型   接口接口能够定义什么类型的数据 
