# 1.Vue  

Vue 是一个构建数据驱动的渐进性框架，它的目标是通过 API 实现响应数据绑定和视图
更新。

是m-v-vm模式，即（`model-view-modelView`），通过modelView作为中间层（即vm的实例），进行双向数据的绑定与变化。

1. 通过建立虚拟dom树`document.createDocumentFragment()`,方法创建虚拟dom树。

2. 一旦被监测的数据改变，会通过`Object.defineProperty`定义的数据拦截，截取到数据的变化。

3. 截取到的数据变化，从而通过订阅——发布者模式，触发`Watcher`（观察者）,从而改变虚拟dom的中的具体数据。

4. 最后，通过更新虚拟dom的元素值，从而改变最后渲染dom树的值，完成双向绑定

    优 缺 点：

   优点：

   1、数据驱动视图，对真实 dom 进行抽象出 virtual dom（本质就是一个 js 对象），
   并配合 diff 算法、响应式和观察者、异步队列等手段以最小代价更新 dom，渲染
   页面
   2、组件化，组件用单文件的形式进行代码的组织编写，使得我们可以在一个文
   件里编写 html\css（scoped 属性配置 css 隔离）\js 并且配合 Vue-loader 之后，支
   持更强大的预处理器等功能
   3、强大且丰富的 API 提供一系列的 api 能满足业务开发中各类需求
   4、由于采用虚拟 dom，让 Vue ssr 先天就足
   5、生命周期钩子函数，选项式的代码组织方式，写熟了还是蛮顺畅的，但仍然
   有优化空间（Vue3 composition-api）
   6、生态好，社区活跃
   缺点：
   1、由于底层基于 Object.defineProperty 实现响应式，而这个 api 本身不支持 IE8
   及以下浏览器
   2、csr 的先天不足，首屏性能问题（白屏）
   3、由于百度等搜索引擎爬虫无法爬取 js 中的内容，故 spa 先天就对 seo 优化心
   有余力不足（谷歌的 puppeteer 就挺牛逼的，实现预渲染底层也是用到了这个工
   具）

# data 为什么是函数  

组件是可复用的`vue`实例，一个组件被创建好之后，就可能被用在各个地方，而组件不管被复用了多少次，组件中的`data`数据都应该是相互隔离，互不影响的，基于这一理念，组件每复用一次，`data`数据就应该被复制一次，之后，当某一处复用的地方组件内`data`数据被改变时，其他复用地方组件的`data`数据不受影响 

# 2.父子组件如何通信 

### 1.通过props实现通信：父向子

- 父组件发送的形式是以属性的形式绑定值到子组件身上。

- 然后子组件用属性props接收 父组件数据           

  而传递的方式也分为两种：

（1）静态传递

子组件通过props选项来声明一个自定义的属性，然后父组件就可以在嵌套标签的时候，通过这个属性往子组件传递数据了。

 （2）动态传递

但是我们更多的情况需要动态的数据。这时候就可以用 v-bind 来实现。通过v-bind绑定props的自定义的属性，传递去过的就不是静态的字符串了，它可以是一个表达式、布尔值、对象等等任何类型的值。

```html
  <div id="app">
    <div>{{pmsg}}</div>
     <!--1、menu-item  在 APP中嵌套着 故 menu-item   为  子组件      -->
     <!-- 给子组件传入一个静态的值 -->
    <menu-item title='来自父组件的值'></menu-item>
    <!-- 2、 需要动态的数据的时候 需要属性绑定的形式设置 此时 ptitle  来自父组件data 中的数据 . 
		  传的值可以是数字、对象、数组等等
	-->
    <menu-item :title='ptitle' content='hello'></menu-item>
  </div>

  <script type="text/javascript">
    Vue.component('menu-item', {
      // 3、 子组件用属性props接收父组件传递过来的数据  
      props: ['title', 'content'],
      data: function() {
        return {
          msg: '子组件本身的数据'
        }
      },
      template: '<div>{{msg + "----" + title + "-----" + content}}</div>'
    });
    var vm = new Vue({
      el: '#app',
      data: {
        pmsg: '父组件中内容',
        ptitle: '动态绑定属性'
      } });
  </script>
```

### 2.通过$ref 实现通信

对于ref官方的解释是：ref 是被用来给元素或子组件注册引用信息的。引用信息将会注册在父组件的 $refs 对象上。看不懂对吧？看看我的解释：

- 如果ref用在子组件上，指向的是组件实例，可以理解为对子组件的索引，通过$ref可能获取到在子组件里定义的属性和方法。
- 如果ref在普通的 DOM 元素上使用，引用指向的就是 DOM 元素，通过$ref可能获取到该DOM 的属性集合，轻松访问到DOM元素，作用与JQ选择器类似。

这里再补充一点就是，prop和$ref之间的区别：

- prop 着重于数据的传递，它并不能调用子组件里的属性和方法。像创建文章组件时，自定义标题和内容这样的使用场景，最适合使用prop。
- $ref 着重于索引，主要用来调用子组件里的属性和方法，其实并不擅长数据传递。而且ref用在dom元素的时候，能使到选择器的作用，这个功能比作为索引更常有用到。

### 3.通过$emit 实现通信:子向父

- 子组件用`$emit()`触发事件。

  `$emit()`  第一个参数为 自定义的事件名称     第二个参数为需要传递的数据

- 父组件用v-on 监听子组件的事件（@子组件事件名），然后执行父组件的事件处理函数（将子组件参数携带过去）

```
<button @click='$emit("enlarge-text", 5)'>扩大父组件中字体大小</button>
```

```
<menu-item :parr='parr' @enlarge-text='handle($event)'></menu-item>

```





#  3.双向绑定和响应式数据原理 ?

双向绑定：
 **数据变化更新视图  view => model** 利用Object.defineProperty的get、set函数对数据更改、读取进行监听。如果数据改变就通知watcher重新计算、更新组件，重新渲染页面
 **视图变化更新数据  model => view 利用事件监听，通过target.value拿到新值赋值给data**

 响应式原理：
 每个组件实例都对应一个 watcher 实例，它会在组件渲染的过程中把“接触”过的数据 property 记录为依赖。之后当依赖项的 setter 触发时，会通知 watcher，从而使它关联的组件重新渲染。

  ![img](C:\Users\superssssss\Desktop\Interview\mj-images\webp) 

 

vue2.x的响应式原理：

**level1:** vue2.0中，响应式实现的核心就是 ES5的Object.defineProperty(obj, prop, descriptor). 通过Object.defineProperty()劫持data和props各个属性的getter和setter，getter做依赖收集，setter派发更新。整体来说是一个 数据劫持 + 发布-订阅者模式。

**level2:** 具体来说， ① vue初始化阶段(beforeCreate之后create之前)，遍历data/props，调用Object.defineProperty给每个属性加上getter、setter。② 每个组件、每个computed都会实例化一个watcher（当然也包括每个自定义watcher,刷新界面、计算属性和侦听属性都属于观察者 ），订阅渲染/计算所用到的所用data/props/computed，一旦数据发生变化，setter被调用，会通知watcher重新计算、更新组件。

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

虚拟 dom 是相对于浏览器所渲染出来的真实 dom 的，在 react，vue 等技术出现之前，
我们要改变页面展示的内容只能通过遍历查询 dom 树的方式找到需要修改的 dom 然
后修改样式行为或者结构，来达到更新 ui 的目的。这种方式相当消耗计算资源，因为每次查询 dom 几乎都需要遍历整颗 dom 树，如果建立一个与 dom 树对应的虚拟 dom 对象（ js 对象），以对象嵌套的方式来表示 dom树，那么每次 dom 的更改就变成了 js 对象的属性的更改，这样一来就能查找 js 对象的属性变化要比查询 dom 树的性能开销小

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

### Object.defineProperty()除了get、set外还能干嘛？

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

   **换成 Proxy 主要是出于性能考虑：**

   Object.defineProperty是一个相对比较昂贵的操作，因为它直接操作对象的属性，颗粒度比较小。将它替换为es6的Proxy，在目标对象之上架了一层拦截，代理的是对象而不是对象的属性。这样可以将原本对对象属性的操作变为对整个对象的操作，颗粒度变大。

   javascript引擎在解析的时候希望对象的结构越稳定越好，如果对象一直在变，可优化性降低，proxy不需要对原始对象做太多操作

1. 针对对象：针对整个对象，而不是对象的某个属性，所以也就不需要对 keys 进行遍历。这解决了上述 Object.defineProperty() 第二个问题 
2.  支持数组：Proxy 不需要对数组的方法进行重载，？？省去了众多 hack，减少代码量等于减少了维护成本，而且标准的就是最好的。 
3. ？？Proxy 的第二个参数可以有 13 种拦截方法，这比起 Object.defineProperty() 要更加丰富 
4.  Proxy 作为新标准受到浏览器厂商的重点关注和性能优化，相比之下 Object.defineProperty() 是一个已有的老方法，可以享受新版本红利。



# 4.Vue中的nextTick？

 **nextTick** 是 Vue 的一个核心实现，在介绍 Vue 的 nextTick 之前，为了方便大家理解，我先简单介绍一下 JS 的运行机制。

JS 运行机制  ：JS 执行是单线程的，它是基于事件循环的。事件循环大致分为以下几个步骤：

（1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。

（2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件(根据放置先后时间顺序决定事件执行顺序)。

（3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。

（4）主线程不断重复上面的第三步。

 **主线程的执行过程就是一个 tick**，而所有的异步结果都是通过 “任务队列” 来调度被调度。 **消息队列中存放的是一个个的任务（task）** 我们了解到数据的变化到 DOM 的重新渲染是一个异步过程，发生在下一个 tick。**这就是我们平时在开发的过程中，比如从服务端接口去获取数据的时候，数据做了修改，如果我们的某些方法去依赖了数据修改后的 DOM 变化，可以在数据变化之后立即使用`Vue.nextTick(callback)` **。这样回调函数在 DOM 更新完成后就会调用 

<https://www.jianshu.com/p/a7550c0e164f> 

 

# 5.vue-router的两种模式 ??

 SPA(single page application):单一页面应用程序，只有一个完整的页面；它在加载页面时，不会加载整个页面，而是只更新某个指定的容器中内容。**单页面应用(SPA)的核心之一是: 更新视图而不重新请求页面**;vue-router在实现单页面前端路由时，提供了两种方式：Hash模式和History模式；根据mode参数来决定采用哪一种方式。

#### 1、Hash模式：

**	vue-router 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。**hash（#）是URL 的锚点，代表的是网页中的一个位置，单单改变#后的部分，浏览器只会滚动到相应位置，不会重新加载网页，也就是说 #是用来指导浏览器动作的，对服务器端完全无用，HTTP请求中也不会不包括#；同时每一次改变#后的部分，都会在浏览器的访问历史中增加一个记录，使用”后退”按钮，就可以回到上一个位置；所以说**Hash模式通过锚点值的改变，根据不同的值，渲染指定DOM位置的不同数据**

​	hash模式背后的原理是onhashchange事件,可以在window对象上监听这个事件:

window.onhashchange=function(event){

console.log(event.oldURL,event.newURL);

lethash=location.hash.slice(1);

 hash发生变化的url都会被浏览器记录下来，从而你会发现浏览器的前进后退都可以用了 .这样一来，尽管浏览器没有请求服务器，但是页面状态和url一一关联起来，后来人们给它起了一个霸气的名字叫前端路由，成为了单页应用标配。

#### 2、History模式：

由于hash模式会在url中自带#，如果不想要很丑的 hash，我们可以用路由的 history 模式，只需要在配置路由规则时，加入"mode: 'history'",这种模式充分利用 history.pushState API 来完成 URL 跳转而无须重新加载页面。

前面的hashchange，你只能改变#后面的url片段，而history api则给了前端完全的自由

history api可以分为两大部分，切换和修改

切换历史状态:包括back,forward,go三个方法，对应浏览器的前进，后退，跳转操作

修改历史状态:包括了pushState,replaceState两个方法,这两个方法接收三个参数:stateObj,title,url

 通过pushstate把页面的状态保存在state对象中，当页面的url再变回这个url时，可以通过event.state取到这个state对象，从而可以对页面状态进行还原，这里的页面状态就是页面字体颜色，其实滚动条的位置，阅读进度，组件的开关

```
//main.js文件中
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 <http://yoursite.com/user/id>
不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 <http://oursite.com/user/id> 就会返回 404，这就不好看了。
所以呢，**你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。**

```
 export const routes = [ 
  {path: "/", name: "homeLink", component:Home}
 {path: "/register", name: "registerLink", component: Register},
 {path: "/login", name: "loginLink", component: Login},
{path: "*", redirect: "/"}]
```

此处就设置如果URL输入错误或者是URL 匹配不到任何静态资源，就自动跳到到Home页面

#### 3、使用路由模块来实现页面跳转的方式

方式1：直接修改地址栏

方式2：this.$router.push(‘路由地址’)

方式3：`<router-link to="路由地址"></router-link>`

# 6.vue3.0 新特性 

**1、双向绑定**
2.0现有限制：

无法检测到新的属性添加/删除
无法监听数组的变化
需要深度遍历，浪费内存
3.0优化：

使用 ES6的Proxy 作为其观察者机制，取代之前使用的Object.defineProperty。Proxy默认可以支持数组
允许框架拦截对象上的操作
多层对象嵌套，使用懒代理
**2、虚拟DOM**
2.0 VDOM性能瓶颈：
* 虽然vue能够保证触发更新的组件最小化，但单个组件部分变化需要遍历该组件的整个vdom树
* 传统vdom性能跟模版大小正相关，跟动态节点的数量无关
  3.0优化工作
  在 vue 3.0 中重新推翻后重写了虚拟 DOM 的实现，官方宣称渲染速度最快可以翻倍。更多编译时的优化以减少运行时的开销

**3、Tree-Shaking**
2.0现有限制： 并不是每个人都使用框架的所有功能，但仍需下载/解析相应代码
3.0优化：将大多数全局API和内部组件移至ES模块导出，？？tree-shaking更友好（Tree-shaking的本质是消除无用的js代码。 ）


**4、Composition API**

 (1)使用传统的option配置方法写组件的时候问题，随着业务复杂度越来越高，代码量会不断的加大；由于相关业务的代码需要遵循option的配置写到特定的区域，导致后续维护非常的复杂，同时代码可复用性不高，而composition-api就是为了解决这个问题而生的

- state更名为reactive。reactive等价于 Vue 2.x 的Vue.observable()，用于获取一个对象的响应性代理对象    const obj = reactive({ count: 0 });
- value更名为ref，并提供isRef和toRefs。const unwrapped = isRef(foo) ? foo.value : foo;
- watch可作用于单一函数

(2) composition-api详细：

1、setup 函数
setup() 函数是 vue3 中，专门为组件提供的新属性。它为我们使用 vue3 的 Composition API 新特性提供了统一的入口。会在 beforeCreate 之后、created 之前执行

1.2 形参
第一个形参 props 用来接收 props 数据，**第二个形参 context 用来定义上下文** 

**reactive 函数**

`reactive()` 函数接收一个普通对象，返回一个响应式的数据对象

**ref 函数**

`ref()` 函数用来根据给定的值创建一个响应式的数据对象，`ref()` 函数调用的返回值是一个对象，这个对象上只包含一个 `value` 属性

`isRef()` 用来判断某个值是否为 ref() 创建出来的对象 

`toRefs()` 函数可以将 `reactive()` 创建出来的响应式对象，转换为普通的对象，只不过，这个对象上的每个属性节点，都是 `ref()` 类型的响应式数据 

**LifeCycle Hooks**

新版的生命周期函数，可以按需导入到组件中，且只能在 setup() 函数中使用

列表，是 vue 2.x 的生命周期函数与新版 Composition API 之间的映射关系：

beforeCreate -> use setup()
created -> use setup()
beforeMount -> onBeforeMount
mounted -> onMounted
beforeUpdate -> onBeforeUpdate
updated -> onUpdated
beforeDestroy -> onBeforeUnmount
destroyed -> onUnmounted
errorCaptured -> onErrorCaptured

# 7.生命周期

- 创建前后 beforeCreate/created

在beforeCreate 阶段，vue实例的挂载元素el和数据对象data都为undefined，还未初始化。在created阶段，vue实例的数据对象有了，el还没有。

- 载入前后 beforeMount/mounted

在beforeMount阶段，vue实例的$el和data都初始化了，但还是挂载之前未虚拟的DOM节点，data尚未替换。 
在mounted阶段，vue实例挂载完成，data成功渲染。

- 更新前后 beforeUpdate/updated

当data变化时，会触发beforeUpdate和updated方法。这两个不常用，不推荐使用。

- 销毁前后beforeDestory/destoryed

beforeDestory是在vue实例销毁前触发，一般在这里要通过removeEventListener解除手动绑定的事件。实例销毁后，触发的destroyed。

# 8.计算属性和 watch 的区别??

计算属性是自动监听依赖值的变化，从而动态返回内容，监听是一个过程，在监听的值变化时，可以触发一个回调，并做一些事情。
所以区别来源于用法，只是需要动态值，那就用计算属性；需要知道值的改变后执行业务逻辑，才用 watch，用反或混用虽然可行，但都是不正确的用法。
**说出一下区别会加分**
computed 是一个对象时，它有哪些选项？
computed 和 methods 有什么区别？
computed 是否能依赖其它组件的数据？
watch 是一个对象时，它有哪些选项？

1. 有get和set两个选项
2. methods是一个方法，它可以接受参数，而computed不能，computed是可以缓存的，methods不会。
3. computed可以依赖其他computed，甚至是其他组件的data
4. watch 配置 
   handler
   deep 是否深度
   immeditate 是否立即执行

**总结**

当有一些数据需要随着另外一些数据变化时，建议使用computed。
当有一个通用的响应数据变化的时候，要执行一些业务逻辑或异步操作的时候建议使用watcher

#  9.v-show 与 v-if 

1. v-hsow和v-if的区别：
   v-show是css切换，v-if是完整的销毁和重新创建。
2. 使用
   频繁切换时用v-show，运行时较少改变时用v-if
3. v-if=‘false’ v-if是条件渲染，当false的时候不会渲染

   4.v-if里面的key值用处 

用于 管理可复用的元素。因为Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。比如...的实例，在，点击切换按钮，虽然DOM变了，但是之前在输入框键入的内容并没有改变，只是替换了placeholder的内容，说明input元素被复用了，如果不希望这样做，可以使用vue.js提供的key属性，它可以让你自己决定是否要复用元素，key的值必须是唯一的！！！ 这么做使 Vue 变得非常快，但是这样也不总是符合实际需求。 **2.2.0+ 的版本里，当在组件中使用 v-for 时，key 是必须的。** 

 给input元素添加key，就不会复用了，切换类型时键入的内容也会被删除，不过label元素仍然是被复用的，因为没有添加key属性！ 

















 vue [keep](https://www.nowcoder.com/jump/super-jump/word?word=keep)-alive，对性能的影响 

 vue 八股文 

 实现一个数据结构，实现路由记录的插入、前进、后退 

 

 

 

 

 

 

 

 