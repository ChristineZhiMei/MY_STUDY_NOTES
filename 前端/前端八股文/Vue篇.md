# 谈谈你对Vue的理解
## Vue是什么？
它是一个用于创建用户界面的开源JS框架，也是创建单页应用的Web应用框架。Vue所关注的核心是MVC模式中的视图层View，同时他能方便的获取数据更新，并且通过组件内特定的方法实现视图与数据的交互。

## Vue的核心特性
### 数据驱动MVVM
。。。

## 组件化
 - 是把图形、非图形的各种逻辑均抽象为一个统一的组件来实现开发的模式
 - Vue中每个.vue文件都可以视为一个组件
 - 降低整个系统的耦合性，保持接口不变的情况下，替换不同的组件来完成需求
 - 调试方便，系统通过组件联系起来，因为组件间耦合性低，通过排除法可以快速定位错误位置
 - 提高可维护性，组件是复用的，修改组件就可以完成功能的升级

## 指令系统
值带有`v-`前缀的特殊属性作用，表达式的值改变时会产生连带的影响，响应式地作用于DOM
  - 条件渲染指令`v-if`
  - 列表渲染指令`v-for`
  - 属性绑定-- `v-bind`
  - 事件绑定-- `v-on`
  - 双向数据绑定`v-model`

## 和React对比
### 相同点
 - 都有组件化思维
 - 都支持服务端渲染
 - 都有Virtual DOM
 - 数据驱动视图
 - 都支持native方案 vue的vexx，React的RN
 - 都有自己的构建工具 vue-cli React的Create React App
### 区别
 - react是单向数据流、vue是双向数据流
 - react使用的是不可变数据，vue使用的是可变数据
 - 组件化通信的方式不同，React是回调函数来通信，Vue父子间通信有两种方式，事件和回调
 - diff算法不同，react主要是使用diff队列保存需要更新哪些DOM，得到patch树，再统一操作批量更新DOM，Vue使用双指针，边对比边更新DOM

# VueRouter的实现原理

核心是改变url，但是不刷新页面，不向服务器发送请求
# Hash路由
 - url的Hash是以#开头的，当hash改变时，页面不会因此刷新，浏览器不会想服务器发送请求
	 - 兼容性好，丑陋
	 - 不需要后端支持
 - 更改Hash以及Hashchange事件
```HTML
<script>
    location.hash = '#/news'
    const myFunc = () => {
        location.replace('#/detail')
    }
    window.addEventListener('hashchange',()=>{
        document.querySelector('.root').innerHTML = '跳转了'
        console.log('1243')
    })
</script>
<body>
    <div class="app" >
        <div class="root">
            6666
        </div>
        <button onclick='myFunc()'>点击切换路由</button>
    </div>
</body>
```

## History路由
 - H5规范中提供了`history.pushState`和`history.replaceState`进行路由控制。通过这两种方法可以实现改变url且不向服务器发送请求
 - history路由没有hash类似的hashchange事件
 - 需要服务端配合避免刷新404
	 - 比如a.com/web/order 和a.com/web/goods
	 - -对于后端来说可能是两个页面，要做一个通配符识别，将/web/*后面的统一返回某个html中
 - 改变当前url有两种方式
	 - 点击前进和后退 -> popstate事件
	 - history.pushState history.replaceState -> 触发相应的函数，在后面手动添加回调
	
```HTML
<body>
    <div id="app" >
        <a href="/web/list">列表</a>
        <a href="/web/detail">详情页</a>、
        <a href="/web/other">404</a>
    </div>
</body>
<script>
    const routerHistoryObj = {
        '/web/list': '<div>列表</div>',
        '/web/detail': '<div>详情页</div>'
    }
    let pathname = ''
    let length = document.querySelectorAll('#app a[href]').length
    for(let i = 0; i < length; i++){
        document.querySelectorAll('#app a[href]')[i].addEventListener('click',(event)=>{
            event.preventDefault()
            pathname = event.currentTarget.getAttribute('href')
            window.history.pushState({}, null, 'index.html'.concat(pathname))
            handleHref()
        })
    }
    window.addEventListener('popstate', handleHref);
    function handleHref(){
        document.getElementById('app').innerHTML = routerHistoryObj[pathname] || '404'
        path = null
    }
</script>
</html>
```

# 用户用组件库怎么实现按需加载

# Keep-alive的原理

# 说一说vue钩子函数？
# 说一说组件通信的方式？
# 说一说computed和watch的区别？
 computed值有缓存、触发条件是依赖值发生更改、 watch无缓存支持异步、监听数据变化 
 标准回答 
	 **computed**： 是计算属性，依赖其它属性值，并且 computed 的值有缓存，只有它依赖的属性值发生改变，下一次获取 computed 的值时才会重新计算 computed 的值；
	 **watch**： 更多的是观察的作用，支持异步，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；
 加分回答 
	 **computed应用场景**：需要进行数值计算，并且依赖于其它数据时，应该使用computed，因为可以利用 computed 的缓存特性，避免每次获取值时，都要重新计算； 
	 **watch应用场景**：需要在数据变化时执行异步或开销较大的操作时，应该使用 watch，使用 watch 选项允许我们执行异步操作 ( 访问一个 API )，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态。这些都是计算属性无法做到的。
# 说一说 Vue 中 $nextTick 作用与原理？
得分点 异步渲染、获取DOM、Promise 
标准回答 
	Vue 在更新 DOM 时是异步执行的，在修改数据后，视图不会立刻更新，而是等同一事件循环中的所有数据变化完成之后，再统一进行视图更新。所以修改完数据，立即在方法中获取DOM，获取的仍然是未修改的DOM。
	 $nextTick的作用是：该方法中的代码会在当前渲染完成后执行，就解决了异步渲染获取不到更新后DOM的问题了。
	 $ nextTick的原理：$ nextTick本质是返回一个Promise 
加分回答 应用场景：在钩子函数created()里面想要获取操作Dom，把操作DOM的方法放在$nextTick中

# 说一说 Vue 列表为什么加 key？
得分点 性能优化、diff算法节点比对、key不能是index 
标准回答 
	为了性能优化 因为vue是虚拟DOM，更新DOM时用diff算法对节点进行一一比对，比如有很多li元素，要在某个位置插入一个li元素，但没有给li上加key，那么在进行运算的时候，就会将所有li元素重新渲染一遍，但是如果有key，那么它就会按照key一一比对li元素，只需要创建新的li元素，插入即可，不需要对其他元素进行修改和重新渲染。 
加分回答 
	key也不能是li元素的index，因为假设我们给数组前插入一个新元素，它的下标是0，那么和原来的第一个元素重复了，整个数组的key都发生了改变，这样就跟没有key的情况一样了
	
Vue列表加key的目的是为diff算法添加标识，因为diff算法判断新旧VDOM是否相同的依据是节点的tag和key。如果tag和key相同则会进一步进行比较，使得尽可能多的节点进行复用。此外，key绑定的值一般是一个唯一的值，比如id。如果绑定数组的索引index，则起不到优化diff算法的作用，因为一旦数组内元素进行增删，后续节点的绑定的key也会发生变化，导致diff进行多余的更新操作。 ...  

# 说一说vue-router 实现懒加载的方法？
得分点 import、require 
标准回答 
	vue-router 实现懒加载的方法有两种： 
		ES6的impot方式: component: () => import('../views/About.vue'), 
		VUE中的异步组件进行懒加载方式: component: resolve=>(require(['../views/About'],resolve)) 
加分回答 
	vue-router 实现懒加载的作用：性能优化，不用到该路由，不加载该组件。
# 说一说Vue2.0 双向绑定的原理与缺陷？
得分点 Object.defineProperty、getter、setter 
标准回答 
	Vue响应式指的是：组件的data发生变化，立刻触发试图的更新 
	原理： Vue 采用数据劫持结合发布者-订阅者模式的方式来实现数据的响应式，通过Object.defineProperty来劫持数据的setter，getter，在数据变动时发布消息给订阅者，订阅者收到消息后进行相应的处理。 
	通过原生js提供的监听数据的API，当数据发生变化的时候，在回调函数中修改dom 核心API：Object.defineProperty 
	Object.defineProperty API的使用 作用: 用来定义对象属性 
	特点： 默认情况下定义的数据的属性不能修改 描述属性和存取属性不能同时使用，使用会报错 
	响应式原理： 获取属性值会触发getter方法 设置属性值会触发setter方法 在setter方法中调用修改dom的方法 
加分回答 Object.defineProperty的缺点 
1. 一次性递归到底开销很大，如果数据很大，大量的递归导致调用栈溢出 
2. 不能监听对象的新增属性和删除属性 
3. 无法正确的监听数组的方法，当监听的下标对应的数据发生改变时
# 说一说Vue3.0 实现数据双向绑定的方法 ？
得分点 Proxy、数据拦截、劫持整个对象、返回一个新对象、有13种劫持 
标准回答 
	Vue3.0 是通过Proxy实现的数据双向绑定，Proxy是ES6中新增的一个特性，实现的过程是在目标对象之前设置了一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。 
	用法： ES6 原生提供 Proxy 构造函数，用来生成 Proxy 实例。 var proxy = new Proxy(target, handler) target: 是用Proxy包装的被代理对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）。 handler: 是一个对象，其声明了代理target 的一些操作，其属性是当执行一个操作时定义代理的行为的函数。 
加分回答 `Object.defineProperty` 的问题：在Vue中，`Object.defineProperty`无法监控到数组下标的变化，导致直接通过数组的下标给数组设置值，不能实时响应。目前只针对以上方法做了hack处理，所以数组属性是检测不到的，有局限性Object.defineProperty只能劫持对象的属性,因此我们需要对每个对象的每个属性进行遍历。Vue里，是通过递归以及遍历data对象来实现对数据的监控的，如果属性值也是对象那么需要深度遍历,显然如果能劫持一个完整的对象，不管是对操作性还是性能都会有一个很大的提升。 
Proxy的两个优点：可以劫持整个对象，并返回一个新对象，有13种劫持
# 说一下Diff算法？
得分点 patch、patchVnode、updateChildren、vue优化时间复杂度为O(n) 
标准回答 Diff算法比较过程 
	第一步：patch函数中对新老节点进行比较 如果新节点不存在就销毁老节点 如果老节点不存在，直接创建新的节点 当两个节点是相同节点的时候进入 patctVnode 的过程，比较两个节点的内部 
	第二步：patchVnode函数比较两个虚拟节点内部 如果两个虚拟节点完全相同，返回 当前vnode 的children 不是textNode，再分成三种情况 - 有新children，没有旧children，创建新的 - 没有新children，有旧children，删除旧的 - 新children、旧children都有，执行`updateChildren`比较children的差异，这里就是diff算法的核心 当前vnode 的children 是textNode，直接更新text 
	第三步：updateChildren函数子节点进行比较 
		- 第一步 头头比较。若相似，旧头新头指针后移（即 `oldStartIdx++` && `newStartIdx++`），真实dom不变，进入下一次循环；不相似，进入第二步。 
		- 第二步 尾尾比较。若相似，旧尾新尾指针前移（即 `oldEndIdx--` && `newEndIdx--`），真实dom不变，进入下一次循环；不相似，进入第三步。
		- 第三步 头尾比较。若相似，旧头指针后移，新尾指针前移（即 `oldStartIdx++` && `newEndIdx--`），未确认dom序列中的头移到尾，进入下一次循环；不相似，进入第四步。
		- 第四步 尾头比较。若相似，旧尾指针前移，新头指针后移（即 `oldEndIdx--` && `newStartIdx++`），未确认dom序列中的尾移到头，进入下一次循环；不相似，进入第五步。 
		- 第五步 若节点有key且在旧子节点数组中找到sameVnode（tag和key都一致），则将其dom移动到当前真实dom序列的头部，新头指针后移（即 `newStartIdx++`）；否则，vnode对应的dom（`vnode[newStartIdx].elm`）插入当前真实dom序列的头部，新头指针后移（即 `newStartIdx++`）。 - 但结束循环后，有两种情况需要考虑： 
			- 新的字节点数组（newCh）被遍历完（`newStartIdx > newEndIdx`）。那就需要把多余的旧dom（`oldStartIdx -> oldEndIdx`）都删除，上述例子中就是`c,d`； 
			- 新的字节点数组（oldCh）被遍历完（`oldStartIdx > oldEndIdx`）。那就需要把多余的新dom（`newStartIdx -> newEndIdx`）都添加。




# Vue3的生命周期

组件从创建到销毁的过程  
创建

- beforCreate：实例创建之前，数据是未加载状态，属性和方法都不能使用
- created：在实例、数据加载后，能初始化数据，DOM渲染之前执行，修改数据不会更新视图  
    挂载
- beforeMounted：完成模板的编译，虚拟DOM已经创建完毕，在数据渲染前最后一次更改数据，修改数据不会更新视图
- mounted：页面、数据渲染完成，编译好的模板挂载完毕，可访问DOM节点  
    更新
- beforeUpdate：组件数据更新之前使用，数据是新的，页面上的数据是旧的，组件即将更新，准备渲染，可修改数据
- updated：render函数重新做了渲染，这时数据和页面都是新的，避免更新数据造成循环  
    销毁
- beforeUnmount：实例销毁之前，可以消除定时器等副作用函数
- unounted：实例销毁后执行  
    使用keep-alive会多出两个属性
- activated：组件被激活时
- deactivited：组件被销毁时

# Vue-Router 有哪几种路由守卫?
Vue-Router 提供了 **三类路由守卫**，用于在路由跳转前后执行逻辑（如权限校验、数据预加载）
 1. 全局守卫
	- **`router.beforeEach`（全局前置守卫）** 路由跳转 **前** 调用
	- **`router.beforeResolve`（全局解析守卫）**导航被确认 **前**，所有组件内守卫和异步组件已解析
	-  **`router.afterEach`（全局后置守卫）** 路由跳转 **完成后** ,日志记录、页面标题更新
2. 路由独享守卫（作用于特定的路由）
    - `beforeEnter` 对单个路由进行校验
3. 组件内守卫（作用于组件）
    - `beforeRouteEnter` 组件实例创建前调用，通过回调获取组件实例
    - `beforeRouteUpdate`当路由改变，但组件被复用时调用
    - `beforeRouteLeave`离开当前路由前调用
守卫执行顺序
 - 导航触发 - beforeRouteLeave（上一个组件的组件内守卫）
 - 全局 - beforeEach
 - 路由独享 - beforeEnter
 - 组件内 - beforeRouterEnter
 - 全局 - beforeResolve
 - 导航确认 - 更新视图
 - 全局 - afterEach