# 1.data 为什么是函数  

组件是可复用的`vue`实例，一个组件被创建好之后，就可能被用在各个地方，而组件不管被复用了多少次，组件中的`data`数据都应该是相互隔离，互不影响的，基于这一理念，组件每复用一次，`data`数据就应该被复制一次，之后，当某一处复用的地方组件内`data`数据被改变时，其他复用地方组件的`data`数据不受影响 

# 2.父子组件如何通信 

### 1.通过prop实现通信

子组件的props选项能够接收来自父组件数据。没错，仅仅只能接收，props是单向绑定的，即只能父组件向子组件传递，不能反向。而传递的方式也分为两种：

（1）静态传递

子组件通过props选项来声明一个自定义的属性，然后父组件就可以在嵌套标签的时候，通过这个属性往子组件传递数据了。

 （2）动态传递

我们已经知道了可以像上面那样给 props 传入一个静态的值，但是我们更多的情况需要动态的数据。这时候就可以用 v-bind 来实现。通过v-bind绑定props的自定义的属性，传递去过的就不是静态的字符串了，它可以是一个表达式、布尔值、对象等等任何类型的值。

### 2.通过$ref 实现通信

对于ref官方的解释是：ref 是被用来给元素或子组件注册引用信息的。引用信息将会注册在父组件的 $refs 对象上。
看不懂对吧？很正常，我也看不懂。那应该怎么理解？看看我的解释：

- 如果ref用在子组件上，指向的是组件实例，可以理解为对子组件的索引，通过$ref可能获取到在子组件里定义的属性和方法。
- 如果ref在普通的 DOM 元素上使用，引用指向的就是 DOM 元素，通过$ref可能获取到该DOM 的属性集合，轻松访问到DOM元素，作用与JQ选择器类似。

这里再补充一点就是，prop和$ref之间的区别：

- prop 着重于数据的传递，它并不能调用子组件里的属性和方法。像创建文章组件时，自定义标题和内容这样的使用场景，最适合使用prop。
- $ref 着重于索引，主要用来调用子组件里的属性和方法，其实并不擅长数据传递。而且ref用在dom元素的时候，能使到选择器的作用，这个功能比作为索引更常有用到。

### 3.通过$emit 实现通信

上面两种示例主要都是父组件向子组件通信，而通过$emit 实现子组件向父组件通信。
对于$emit官网上也是解释得很朦胧，我按我自己的理解是这样的:
vm.$emit( event, arg )
$emit 绑定一个自定义事件event，当这个这个语句被执行到的时候，就会将参数arg传递给父组件，父组件通过@event监听并接收参数。



#  3.双向绑定和响应式数据原理 ?

双向绑定：
 **数据变化更新视图  view => model** 利用Object.defineProperty的get、set函数对数据更改、读取进行监听。如果数据改变就通知watcher进行重新渲染页面
 **视图变化更新数据  model => view 利用事件监听，通过target.value拿到新值赋值给data**

 响应式原理：
 每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。

  ![img](C:\Users\superssssss\Desktop\Interview\mj-images\webp) 

 

vue2.x的响应式原理：

**level1:** vue2.0中，响应式实现的核心就是 ES5的Object.defineProperty(obj, prop, descriptor). 通过Object.defineProperty()劫持data和props各个属性的getter和setter，getter做依赖收集，setter派发更新。整体来说是一个 数据劫持 + 发布-订阅者模式。

**level2:** 具体来说， ① vue初始化阶段(beforeCreate之后create之前)，遍历data/props，调用Object.defineProperty给每个属性加上getter、setter。② 每个组件、每个computed都会实例化一个watcher（当然也包括每个自定义watcher,刷新界面、计算属性和侦听属性都属于观察者 ），订阅渲染/计算所用到的所用data/props/computed，一旦数据发生变化，setter被调用，会通知渲染watcher重新计算、更新组件。

    function observer(data) {
      // 仅当 data 为数组和纯对象时才执行
      if (typeof data !== 'object' || data === null) {
        return
      }
      // 当 data 为一个数组时，替换其原型（vue 实例中的 data 不可能为数组，仅当vue实例中的 data 属性为数组才执行这一步）
      rewritePrototype(data)
      defineReactive(data)//定义响应的
    }
    
    // 用于重写 vue 实例中的 data
    function defineReactive(data) {
      for (const key in data) {
        if (data.hasOwnProperty(key)) {
          let value = data[key]
      observer(data[key]) // 当 data 的某个属性为为数组或纯对象时做进一步处理
      if (Array.isArray(value)) return
    
      Object.defineProperty(data, key, {
        get() {
          return value
        },
        set(newVal) {
          value = newVal
          updateView() // 视图更新
        }  })
    }  }}
    
    // 为 data 中的数组重写一个 原型
    function rewritePrototype(data) {
      if (!Array.isArray(data)) {
        return
      }
      const OldProto = Array.prototype
      // 新的原型proto的原型指向Array的显示原型
      const proto = Object.create(OldProto)
      const rewritten = ['pop', 'push', 'shift', 'unshift', 'splice']
      rewritten.forEach(methodName => {
        proto[methodName] = function(...args) {
          OldProto[methodName].apply(this, args)
          updateView()
        }
      })
      data['proto'] = proto
    }
    
    function updateView() {
      console.log('视图更新')
    }
- observer 函数做的事很简单，就是做了个判断，只有 data 为数组或纯对象时才会往下执行 defineReactive 和 rewritePrototype 这两个函数。
- defineReactive 函数利用 Object.defineProperty() 方法对 data 中的所有属性进行了重写，此时我们就可以在每个属性的 set 方法中渲染视图了，当在 vue 实例中通过 this.xx 修改数据时，就会调用set方法，从而渲染视图。
- rewritePrototype 函数则是负责当 data 中的属性是数组时，会对这个数组原型上某些方法（即 对数组本身具有修改作用的方法）进行重写，以达到调用这些方法的同时调用 updateView 来渲染视图的目的。（注意：重写这些方法并不意味着改写 Array.prototype 上的方法，而是给数组创建一个新的原型，这个新原型继承 Array.prototype ，再在这个新的原型上重写这些方法覆盖 Array.prototype 上的方法，如此就防止了污染全局的 Array.prototype）
- 我们发现，当添加属性或删除属性时，不会更新视图，这是因为 Object.defineProperty() 方法本就无法监听其属性的新增和删除，因而 vue 内置了 Vue.set / Vue.delete 

### **Virtual DOM**（虚拟DOM）

```
<div class="wrapper" id="box" index="1" key="1">
  <div class="child1">第一个字节点</div>
  <div class="child2">第二个子节点</div>
</div>
```

以上代码用虚拟DOM表示：

```
{
  sel: 'div#box.wrapper', // 选择器
  props: { index: '1' }, // 属性
  text: null, // 该元素的文本节点
  key: '1',
  children: [
    {
      sel: 'div.child1',
      props: null,
      text: '第一个字节点',
      children: null,
      elm: <div class="child1">第一个字节点</div>
    },
    {
      sel: 'div.child2'
      text: '第二个字节点',
      children: null,
      elm: <div class="child2">第二个字节点</div>
    }
  ],
  elm: <div class="wrapper" id="box" index="1">...</div>, // 这是一个DOM对象，指向的就是该节点
}
```

以上演示就是一个简易的虚拟DOM。可以看出，我们 `在 vue 中写的 html 代码并非是 html 代码，本质上还是 js 代码，vue 将我们写的 “html代码” 转换成了虚拟 DOM。

那么转换成虚拟DOM又有什么好处呢？
     当点击一个按钮触发DOM的变化时，仅仅一个很小的变化DOM就会全部重新渲染，这显然很不合理。为了使DOM只重新渲染有变化的部分，就需要在DOM中找不同，这就有了虚拟DOM的出现。

虽然找不同的过程也会有一些性能消耗，但这都是原生js中实现的逻辑，相对于DOM渲染的代价来说，这点消耗就是九牛一毛

### diff 算法

只针对虚拟 DOM 的同层级做对比 

![](C:\Users\superssssss\Desktop\Interview\mj-images\1617030162292.png)

### Object.defineProperty()除了get、set外还能干嘛

1.**Object.defineProperty()**方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象 

对象里目前存在的属性描述符有两种主要形式：*数据描述符*和*存取描述符*。*数据描述符*是一个具有值的属性，该值可以是可写的，也可以是不可写的。*存取描述符*是由 getter 函数和 setter 函数所描述的属性。一个描述符只能是这两者其中之一；不能同时是两者 

2.描述符可拥有的键值

`                          configurable``enumerable``value``writable ``get``    set   

-  数据描述符         可以           可以          可以      可以    不可以不可以
-  存取描述符        可以           可以           不可以  不可以 可以    可以     

3.还可以：

创建属性 [修改属性]自定义Setters和Getters

### Vue3 Proxy？？？

1. Object.defineProperty 只能对已存在的属性进行劫持，不存在的属性没有感知，删除属性没有感知

2. 在 get 和 set 里面不能直接对原对象本身进行操作，需要拷贝一份原对象（可能会有性能问题），否则会栈溢出

3. Vue 没有提供对数组的监听（并不是 Object.defineProperty 不支持对数组的劫持），而是性能考虑！

4. 对象里面还有复杂数据类型的话，需要递归劫持里面的每一个属性（Proxy 也需要递归，但递归的是一个对象）

5. 核心一句话，换成 Proxy 主要是出于性能考虑
  

# 4.Vue中的nextTick？

 **nextTick** 是 Vue 的一个核心实现，在介绍 Vue 的 nextTick 之前，为了方便大家理解，我先简单介绍一下 JS 的运行机制。

JS 运行机制  ：JS 执行是单线程的，它是基于事件循环的。事件循环大致分为以下几个步骤：

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件(根据放置先后时间顺序决定事件执行顺序)。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。

 **主线程的执行过程就是一个 tick**，而所有的异步结果都是通过 “任务队列” 来调度被调度。 **消息队列中存放的是一个个的任务（task）** 我们了解到数据的变化到 DOM 的重新渲染是一个异步过程，发生在下一个 tick。**这就是我们平时在开发的过程中，比如从服务端接口去获取数据的时候，数据做了修改，如果我们的某些方法去依赖了数据修改后的 DOM 变化，可以在数据变化之后立即使用`Vue.nextTick(callback)` **。这样回调函数在 DOM 更新完成后就会调用 

<https://www.jianshu.com/p/a7550c0e164f> 

 

 







vue3改成了proxy，为什么使用proxy

 

 

 

 

 

 

 

 

 

 

 