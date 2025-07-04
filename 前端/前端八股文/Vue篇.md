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