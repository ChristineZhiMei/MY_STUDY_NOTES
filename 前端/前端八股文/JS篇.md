# 基础篇
## Js数组方法（追问sort背后用的什么算法

### `sort`背后的算法是Timsort一种混合算法
短数组用插入排序
长数组用归并排序

## 将日期 2018-07-09 14:13:11 转换为毫秒数

1. 首先使用replace方法将日期字符串中的"-"替换为"/"，**这是因为在某些浏览器中，用"-"分隔的日期字符串可能无法正确解析**  
2. 然后通过new Date()创建日期对象，再使用getTime()方法获取毫秒数  
这种方法简单可靠，兼容性好
```js
str = str.replace(new RegExp("-","gm"),"/");
var startDateM = (new Date(str)).getTime(); //得到毫秒数
```

1. 使用split方法配合正则表达式将日期字符串分割成数组  
2. 通过Date构造函数传入年、月、日、时、分、秒来创建日期对象（注意月份要减1因为JS中月份从0开始计数）  
3. 使用Date.parse()将日期对象转换为毫秒数  
这种方法更灵活，可以精确控制日期的每个部分
```js
var arr = str.split(/[- : \/]/);
var startDate = Date.parse(new Date(arr[0], arr[1]-1, arr[2], arr[3], arr[4], arr[5]));
```
## const 对象属性只读 Object.freeze()

## 运算符的隐式类型转换规则(仅限于比较时)
```js
const a = ([]) ? true : false; // true
const b = [] ==false ? true : false; // true
const c = ({}==false) ? true : false; // false
```
 1. 数组`[]`会先被转换为原始值，即空字符串`""`
 2. 空字符串`""`会被转换为数字`0`
 3. `false`也会被转换为数字`0`
 4. `0 == 0 `为 `true`
 5. 对象`{}`转换为原始值会得到`"[object Object]"`
 6. 这个字符串无法与`false`相等

*注意：单独用空数组作条件时，是 真值 *

## 原型链
![[Pasted image 20250707192147.png]]


---
# 零碎知识篇
## 0作为除数的分子在js中是合法的
```js
// 执行以下程序，当用户在prompt输入框中输入0时，输出结果为a b d
var num = prompt('请输入分母:')
try{
  console.log('a');
  value = 0 / num;
  console.log('b');
}
catch(e){
  console.log('c');
}
finally{
  console.log('d');
}
```

**补充说明**：在JavaScript中，0作为分母的运算是合法的：  
- 非0数除以0会得到**Infinity**  
- 0除以0会得到**NaN**、
	 - NAN：*Not a Number* 表示未定义或不可表示的数值，NaN与任何其他值（包括NaN本身）进行比较的结果都是false，包括NaN == NaN。在编程中，可以使用 Number.isNaN() 或 isNaN() 来判断一个值是否为NaN
这些都是合法的运算结果，不会触发异常，所以catch块不会执行。

## 说一说null 和 undefined 的区别，**如何让一个属性变为null**
得分点 操作的变量没有被赋值、全局对象的一个属性、函数没有return返回值、值 `null` 特指对象的值未设置 undefined == null、undefined !== null 
**标准回答** undefind 是全局对象的一个属性，当一个变量没有被赋值或者一个函数没有返回值或者某个对象不存在某个属性却去访问或者函数定义了形参但没有传递实参，这时候都是undefined。
undefined通过typeof判断类型是'undefined'。undefined == undefined undefined === undefined 。 
null代表对象的值未设置，相当于一个对象没有设置指针地址就是null。null通过typeof判断类型是'object'。null === null null == null null == undefined null !== undefined undefined 表示一个变量初始状态值，而 null 则表示一个变量被人为的设置为空对象，而不是原始状态。在实际使用过程中，不需要对一个变量显式的赋值 undefined，当需要释放一个对象时，直接赋值为 null 即可。 让一个变量为null，直接给该变量赋值为null即可。 
**加分回答** null 其实属于自己的类型 Null，而不属于Object类型，typeof 之所以会判定为 Object 类型，是因为JavaScript 数据类型在底层都是以二进制的形式表示的，二进制的前三位为 0 会被 typeof 判断为对象类型，而 null 的二进制位恰好都是 0 ，因此，null 被误判断为 Object 类型。 对象被赋值了null 以后，对象对应的堆内存中的值就是游离状态了，GC 会择机回收该值并释放内存。因此，需要释放某个对象，就将变量设置为 null，即表示该对象已经被清空，目前无效状态。


## 说一说伪数组和数组的区别？
1. 伪数组的特点：类型是object、不能使用数组方法、可以获取长度、可以使用for in遍历 
2. 伪数组可以装换为数组的方法：a. Array.prototype.slice.call() b.Array.from() c. `[...伪数组]` 
3. 有哪些是伪数组：函数的参数arguments，Map和Set的keys()、values()和entires() 

## 说一说map 和 forEach 的区别？
forEach是针对数组中每一个元素，提供一个可执行的函数操作，因此它（可能）会改变原数组中的值。不会返回有意义的值，或者说会返回undefined；
而map是会分配内存空间创建并存储一个新的数组，新数组中的每一个元素由调用的原数组中的每一个元素执行所写的函数得来，返回的就是新数组，因此不会改变原数组的值；   


## 说一说es6中箭头函数？
1. 写法简洁 
2. 无自己的this，继承上一个作用域的this（全局或上一个函数） 
3. 内部this无法被改变 
4. arguments的特殊性（window下保存，this指向上一个函数则arguments表示上一个函数的参数） 5.不能作为构造函数（无自己的this、constructor） 
5. 无自己的prototype  
  
## 事件扩展符用过吗(...)，什么场景下？

1. 数组去重`[...new Set(arr)]`
2. 数组拷贝`[...arr]` 
3. 伪数组转真数组 `[...伪数组]`
4. 构造字面量对象时, 将对象表达式按key-value的方式展开
5. 只能用于可迭代对象
	JavaScript 中内置的可迭代对象包括：
	- **数组（Array）**
	- **字符串（String）**
	- **Set**
	- **Map**
	- **DOM 集合**（如 `NodeList`）
	- **arguments 对象**（函数内部的参数集合）
6. 等价于apply的方式
```js
/ 示例：将数组作为参数传递给函数
function sum(a, b, c) {
  return a + b + c;
}
const numbers = [1, 2, 3];
sum(...numbers); // 等价于 sum(1, 2, 3)，返回 6


// 示例：使用 apply() 传递数组参数
function sum(a, b, c) {
  return a + b + c;
}
const numbers = [1, 2, 3];
sum.apply(null, numbers); // 同样返回 6，null 表示不指定 this
```

### 关于可迭代对象
在 JavaScript 中，**可迭代对象（Iterable）** 是一种特殊的数据结构，它定义了一种标准的方式来按顺序访问其元素。这种对象可以被 `for...of` 循环遍历，也能使用展开运算符（`...`）、解构赋值等语法。

**核心概念：迭代协议**
可迭代对象必须实现 **迭代协议（Iteration Protocol）**，该协议包含两个核心组件：
 1. `Symbol.iterator` 方法
	- 这是一个特殊的内置符号，用于定义对象的迭代器。
	- 调用该方法会返回一个 **迭代器对象（Iterator）**。
 2. 迭代器对象
	- 必须包含一个 `next()` 方法，每次调用返回一个对象：
	    - `value`：当前迭代的值。
	    - `done`：布尔值，表示迭代是否结束。

## 说一说new会发生什么？
**得分点** 创建空对象、为对象添加属性、把新对象当作this的上下文、箭头函数不能作为构造函数 
**标准回答** `new` 关键字会进行如下的操作： 
1. 创建一个空的简单JavaScript对象（即`{}`）；
2. 为步骤1新创建的对象添加属性`__proto__`，将该属性链接至构造函数的原型对象 ； 
3. 将步骤1新创建的对象作为`this`的上下文 ； 
4. 如果该函数没有返回对象，则返回`this`。 
**加分回答** `new`关键字后面的构造函数不能是箭头函数。

## 说一说defer和async区别？
得分点 加载JS文档和渲染文档可以同时进行、JS代码立即执行、JS代码不立即执行、渲染引擎和JS引擎互斥 
标准回答  浏览器会立即加载JS文件并执行指定的脚本，“立即”指的是在渲染该 script 标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行 加上async属性，加载JS文档和渲染文档可以同时进行（异步），当JS加载完成，JS代码立即执行，会阻塞HTML渲染。  加上defer，加载后续文档元素的过程将和 script.js 的加载并行进行（异步），当HTML渲染完成，才会执行JS代码。 
加分回答 渲染阻塞的原因： 由于 JavaScript 是可操纵 DOM 的,如果在修改这些元素属性同时渲染界面（即 JavaScript 线程和 UI 线程同时运行）,那么渲染线程前后获得的元素数据就可能不一致了。因此为了防止渲染出现不可预期的结果,浏览器设置 GUI 渲染线程与 JavaScript 引擎为互斥的关系。当浏览器在执行 JavaScript 程序的时候,GUI 渲染线程会被保存在一个队列中,直到 JS 程序执行完成,才会接着执行。如果 JS 执行的时间过长,这样就会造成页面的渲染不连贯,导致页面渲染加载阻塞的感觉

## 说一说如何实现可过期的localstorage数据？

**得分点** 惰性删除、定时删除 
**标准回答** localStorage只能用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去删除。所以要实现可过期的localStorage缓存的中重点就是：如何清理过期的缓存。 
 - 目前有两种方法，一种是惰性删除，另一种是定时删除。 
	 - 惰性删除是指某个键值过期后，该键值不会被马上删除，而是等到下次被使用的时候，才会被检查到过期，此时才能得到删除。实现方法是，存储的数据类型是个对象，该对象有两个key，一个是要存储的value值，另一个是当前时间。获取数据的时候，拿到存储的时间和当前时间做对比，如果超过过期时间就清除Cookie。 
	 - 定时删除是指，每隔一段时间执行一次删除操作，并通过限制删除操作执行的次数和频率，来减少删除操作对CPU的长期占用。另一方面定时删除也有效的减少了因惰性删除带来的对localStorage空间的浪费。实现过程，获取所有设置过期时间的key判断是否过期，过期就存储到数组中，遍历数组，每隔1S（固定时间）删除5个（固定个数），直到把数组中的key从localstorage中全部删除。 
**加分回答** LocalStorage清空应用场景：token存储在LocalStorage中，要清空

## 说一下token 能放在cookie中吗？
**得分点** 能、不设置cookie有效期、重新登录重写cookie覆盖原来的cookie 
**标准回答** 能。 token一般是用来判断用户是否登录的，它内部包含的信息有：uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign（签名，token 的前几位以哈希算法压缩成的一定长度的十六进制字符串） `token`可以存放在`Cookie`中，`token` 是否过期，应该由后端来判断，不该前端来判断，所以`token`存储在`cookie`中只要不设置`cookie`的过期时间就ok了，如果 `token` 失效，就让后端在接口中返回固定的状态表示`token` 失效，需要重新登录，再重新登录的时候，重新设置 `cookie` 中的 `token` 就行。 
**加分回答** token认证流程 
1. 客户端使用用户名跟密码请求登录 
2. 服务端收到请求，去验证用户名与密码 
3. 验证成功后，服务端签发一个 token ，并把它发送给客户端 
4. 客户端接收 token 以后会把它存储起来，比如放在 cookie 里或者 localStorage 里 
5. 客户端每次发送请求时都需要带着服务端签发的 token（把 token 放到 HTTP 的 Header 里） 
6. 服务端收到请求后，需要验证请求里带有的 token ，如验证成功则返回对应的数据


## 说一说axios的拦截器原理及应用？
axios拦截器分为响应和请求拦截器
请求拦截器 在请求发送前进行必要操作处理，例如添加统一cookie、请求体加验证、设置请求头等，相当于是对每个接口里相同操作的一个封装；  
响应拦截器 同理，响应拦截器也是如此功能，只是在请求得到响应之后，对响应体的一些处理，通常是数据统一处理等，也常来判断登录失效等。

## 说一说创建ajax过程？

**得分点** new XMLHttpRequest()、设置请求参数open()、发送请求request.send()、响应request.onreadystatechange
**标准回答** 创建ajax过程： 
1. 创建XHR对象：new XMLHttpRequest() 
2. 设置请求参数：request.open(Method, 服务器接口地址); 
3. 发送请求: request.send()，如果是get请求不需要参数，post请求需要参数request.send(data) 
4. 监听请求成功后的状态变化：根据状态码进行相应的处理。 XHR.onreadystatechange = function () { if (XHR.readyState == 4 && XHR.status == 200) { console.log(XHR.responseText); // 主动释放,JS本身也会回收的 XHR = null; } }; 
**加分回答** POST请求需要设置请求头 readyState值说明 
	0：初始化,XHR对象已经创建,还未执行open 
	1：载入,已经调用open方法,但是还没发送请求 
	2：载入完成,请求已经发送完成 
	3：交互,可以接收到部分数据 
	4：数据全部返回 status值说明 200：成功 404：没有发现文件、查询或URl 500：服务器产生内部错误



## 说一下fetch 请求方式？
1、fetch 是 XMLHttpRequest 的一种替代方案，fetch是js原生语法也能像ajax一样获取后台数据 
优点： 
- 基于标准Promise实现，支持async/ await 
- 语法简洁，更加语意化fetch 可以理解成简化版的 XMLHttpRequest 
缺点：
- 兼容性不好，不支持IE 
- 不能中断请求（xhr有个xhr.abort 方法能直接中断请求） 
- 没法检测请求进度  

## 说一下有什么方法可以保持前后端实时通信？
得分点 轮询、长轮询、 iframe流、WebSocket、SSE 
**标准回答** 保持前后端实时通信的方法有以下几种： 
	**轮询** 是客户端和服务器之间会一直进行连接，每隔一段时间就询问一次。其缺点也很明显：连接数会很多，一个接受，一个发送。而且每次发送请求都会有Http的Header，会很耗流量，也会消耗CPU的利用率。
		优点就是
			实现简单，无需做过多的更改。
		缺点是
			轮询的间隔过长，会导致用户不能及时接收到更新的数据；
			轮询的间隔过短，会导致查询请求过多，增加服务器端的负担 
	**长轮询** 是对轮询的改进版，客户端发送HTTP给服务器之后，如果没有新消息，就一直等待。有新消息，才会返回给客户端。在某种程度上减小了网络带宽和CPU利用率等问题。由于http数据包的头部数据量往往很大（通常有400多个字节），但是真正被服务器需要的数据却很少（有时只有10个字节左右），这样的数据包在网络上周期性的传输，难免对网络带宽是一种浪费。
		优点是
			做了优化，有较好的时效性。
		缺点是
			保持连接会消耗资源; 服务器没有返回有效数据，程序超时。 
	**iframe流** 方式是在页面中插入一个隐藏的iframe，利用其src属性在服务器和客户端之间创建一条长连接，服务器向iframe传输数据（通常是HTML，内有负责插入信息的javascript），来实时更新页面。
		优点是
			消息能够实时到达；浏览器兼容好。
		缺点是
			服务器维护一个长连接会增加开销；IE、chrome、Firefox会显示加载没有完成，图标会不停旋转。 
	**WebSocket** 是类似Socket的TCP长连接的通讯模式，一旦WebSocket连接建立后，后续数据都以帧序列的形式传输。在客户端断开WebSocket连接或Server端断掉连接前，不需要客户端和服务端重新发起连接请求。
		优点是
			在海量并发和客户端与服务器交互负载流量大的情况下，极大的节省了网络带宽资源的消耗，有明显的性能优势，且客户端发送和接受消息是在同一个持久连接上发起，实时性优势明显。
		缺点是
			浏览器支持程度不一致，不支持断开重连。 
	**SSE(Server-Sent Event)** 是建立在浏览器与服务器之间的通信渠道，然后服务器向浏览器推送信息。SSE 是单向通道，只能服务器向浏览器发送，因为 streaming 本质上就是下载。 
		优点是
			SSE 使用 HTTP 协议，现有的服务器软件都支持。SSE 属于轻量级，使用简单；SSE 默认支持断线重连； 
加分回答 
	轮询适用于：小型应用，实时性不高 
	长轮询适用于：一些早期的对及时性有一些要求的应用：web IM 聊天 
	iframe适用于：客服通信等 
	WebSocket适用于：微信、网络互动游戏等 
	SSE适用于：金融股票数据、看板等
## 说一说事件循环Event loop，宏任务与微任务？
##

---

# 手撕篇
## sleep 毫秒
```js
// sleep
function sleep(ms){
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve()
        },ms)
    })
}
async function test(){
    console.log('Start')
    await sleep(2000)
    console.log('End')
}
test()
```
## 手写 instanceof
```js
// instanceof
function myInstanceof(obj, class_name){
    if(obj === class_name.prototype) return true
    if(obj == null) return false
    return myInstanceof(Object.getPrototypeOf(obj),class_name)
}
```
## 手写 new 操作符
```js
// new
function myNew(Func,...args){
    if(typeof Func !== 'function'){
        throw new TypeError('Func is not a Function')
    }
    const newObj = Object.create(Func.prototype)
    const result = Func.apply(newObj,args)
    return typeof result === 'object' && result !== null ? result : newObj
}

function Person(name, age){
    this.name = name
    this.age = age
}
Person.prototype.speak = function(){
    console.log(this.name, this.age)
}

const p1 = myNew(Person, '小明', 19)
p1.speak()
```

## 手写 call、apply、bind 函数
### call
```js
// call
Function.prototype._call = function(thisObj,...args){
    // 将thisObj转换成对象类型
    thisObj = (thisObj !== null && thisObj !== undefined) ? Object(thisObj) : window
    // 使用thisObj调用函数，绑定this
    let fn = Symbol()
    // 将函数挂在到对象上
    thisObj[fn] = this

    let result = thisObj[fn](...args)
    delete thisObj[fn]

    return result
}

function printThis(name){
    console.log(this.age)
    console.log(name)
}

const p1 = new Object()
p1.age = 19

printThis._call(p1,'小明')
```

### apply
```js
// apply
Function.prototype._apply = function(thisObj,args){
    // 将thisObj转换成对象类型
    thisObj = (thisObj !== null && thisObj !== undefined) ? Object(thisObj) : window
    // 使用thisObj调用函数，绑定this
    let fn = Symbol()
    // 将函数挂在到对象上
    thisObj[fn] = this
    args = args || [] 
    let result = thisObj[fn](...args)
    delete thisObj[fn]

    return result
}

function printThis(name){
    console.log(this.age)
    console.log(name)
}

const p1 = new Object()
p1.age = 19

printThis._apply(p1,['小明'])
```

### bind
```js
// bind
Function.prototype._call = function(thisObj,args){
    // 将thisObj转换成对象类型
    thisObj = (thisObj !== null && thisObj !== undefined) ? Object(thisObj) : window
    // 使用thisObj调用函数，绑定this
    let fn = Symbol()
    // 将函数挂在到对象上
    thisObj[fn] = this
    args = args || [] 
    let result = thisObj[fn](...args)
    delete thisObj[fn]

    return result
}

// 利用call模拟bind
Function.prototype._bind = function(thisObj, args){
    return () => {
        this._call(thisObj, args)
    }
}

function printThis(name){
    console.log(this.age)
    console.log(name)
}

const p1 = new Object()
p1.age = 19

printThis._bind(p1,['小明'])()
```

## 手写深拷贝
```js
// cloneDeep(解决循环引用版本)
function cloneDeep(obj, hash = new WeakMap()){
    if(!obj || typeof obj !== "object") return obj

    if(hash.get(obj)) return hash.get(obj)
    
    let cloneObj = Array.isArray(obj) ? [] : {}

    hash.set(obj, cloneObj)

    for(let key in obj){
        // hasOwnProperty 确保只处理自身属性而非原型链上的属性
        if(obj.hasOwnProperty(key)){
            cloneObj[key] = deepClone(obj[key], hash)
        }
    }
    return cloneObj
}
```
## 手写防抖节流
### 防抖
```js
// Debounce
function debounce(func, delay){
    let timer = null
    return function(...args){
        if(timer){
            clearTimeout(timer)
        }
        timer = setTimeout(()=>{
            func.call(this,...args)
            timer = null
        },delay)
    }
}
```
### 节流
```js
// Debounce
function debounce(func, delay){
    let timer = null
    return function(...args){
        if(timer){
            clearTimeout(timer)
        }
        timer = setTimeout(()=>{
            func.call(this,...args)
            timer = null
        },delay)
    }
}
```

## 手写Ajax请求
```js
// AJAX请求
function ajax(url){
    return new Promise((resolve, reject) => {
        // 创建一个 XHR 对象
        const xhr = new XMLHttpRequest()
        // 指定请求类型，请求URL，和是否异步
        xhr.open('GET', url , true)
        xhr.onreadystatechange = () => {
            if(xhr.readyState === 4) {
                if(xhr.status === 200){
                    resolve(JSON.parse(xhr.responseText))
                }else{
                    reject(xhr.status + 'error')
                }
            }
        }
        // 发送定义好的请求
        xhr.send(null)
    })
}
```
## 手写数组去重
### set方法
```js
//数组去重set
function unique(arr){
    return [...new Set(arr)]
}
```

### filter + indexOf
```js
// filter过滤
function unique(arr){
    return arr.filter((item, index) => {
        return arr.indexOf(item) === index
    })
}
```
## 手写数组扁平
```js
// 转成字符串扁平化
function myFlat(arr){
    return arr.toString().split(',').map(item => parseInt(item))
}
```

```js
// 递归
function myFlat(arr){
    let temp = []
    for(let i of arr) temp.push(Array.isArray(i) ? temp.push(...myFlat(i)) : i)
    return temp
}
```
## 手写数组乱序
```js
// sort + Math.random()
function shuffle(arr){
    return arr.sort(() => Math.random() - 0.5)
}
```
## 手写 Promise.all()、Promise.race()
### Promise.race()
```js
function myPromiseRace(args){
    return new Promise((resolve, reject) => {
        for(let i of args){
            i.then((data) => {
                resolve(data)
            }).catch((err) => {
                reject(err)
            })
        }
    })
}
```
## 手写 JSONP
```js
function addScript(src){
    const script = document.createElement('script')
    script.src = src
    script.type = "text/javascript"
    document.body.appendChild(script)
}
addScript("http://xxx.x.x.x:xxxx/xxx?callback=handleRes");
function handleRes(res){
    console.log(res)
}
```
需要服务器返回handleRes方法包裹的信息
## 手写函数柯里化
```js
function sum(x,y,z){
    return x + y + z
}
function hyCurrying(fn){
    function curried(...args){
        if(args.length >= fn.length){
            return fn.apply(this,args)
        }else{
            return function(...args2){
                return curried.apply(this, args.concat(args2))
            }
        }
    }
    return curried
}
let currySum = hyCurrying(sum)
console.log(currySum(1)(2,3))
```
## 手写发布-订阅模式 || Event Bus
```js
class EventBus{
    constructor(){
        this.events = this.events || {}
    }

    $on(eventName, callback){
        if(this.events[eventName]){
            this.events[eventName].push(callback)
        }else{
            this.events[eventName] = []
            this.events[eventName].push(callback)
        }
    }

    $emit(eventName, ...args){
        if(this.events[eventName]){
            this.events[eventName].map(fn => fn.apply(this, args))
        }
    }

    $once(eventName, callback){
        const self = this
        const onceCallback = function(){
            callback.apply(self, arguments)
            self.$off(eventName)
        }
        this.$on(eventName, onceCallback)
    }
    
    $off(eventName){
        if(eventName){
            if(this.events[eventName]){
                delete this.events[eventName]
            }
        }else{
            this.events = {}
        }
    }
}

// 创建事件总线实例
const eventBus = new EventBus()
// 注册一个普通事件监听器
eventBus.$on('message', (msg) => {
    console.log('Received message:', msg)
})
// 注册一个只执行一次的事件监听器
eventBus.$once('greeting', (name) => {
    console.log(`Hello, ${name}`)
})
// 触发事件
eventBus.$emit('message', 'Hello World'); // 输出: Received message: Hello World
eventBus.$emit('greeting', 'Alice');      // 输出: Hello, Alice!
eventBus.$emit('greeting', 'Bob');        // 不会触发，因为$once的回调只执行一次

// 注销事件
eventBus.$off('message');
eventBus.$emit('message', 'Test');        // 不会触发，因为'message'事件已被注销
```
## setTimeout 实现 setInterval
```js
function setIntervalFunc(fn, time){
    let timerId = null
    let isStopped = false
    var interval = function(){
        if(isStopped) return
        timerId = setTimeout(() => {
            interval()
            fn()
        }, time)
    }
    timerId = setTimeout(interval, time)
    return function stop(){
        isStopped = true
        clearTimeout(timerId)
    }
}

let fn = () => console.log(123)
let stopInterval = setIntervalFunc(fn, 1000)
setTimeout(() => {
    stopInterval()
    console.log('stop!')
},10000)
```
## 异步循环打印 1，2，3
```js
let sleep = function(time, i){
    return new Promise((resolve) => {
        setTimeout(()=>{
            resolve(i)
        }, time)
    })
}

let start = async function(){
    for(let i = 1; i <= 3; i++){
        let result = await sleep(1000, i)
        console.log(result)
    }
}
start()
```
## 循环打印红、黄、绿
```js
function red() {
    console.log('red');
}
function green() {
    console.log('green');
}
function yellow() {
    console.log('yellow');
}
 
const task = (timer, light) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (light === 'red') {
                red()
            }
            else if (light === 'green') {
                green()
            }
            else if (light === 'yellow') {
                yellow()
            }
            resolve()
        }, timer)
    })
}
const taskRunner =  async () => {
    await task(3000, 'red')
    await task(2000, 'green')
    await task(2100, 'yellow')
    taskRunner()
}
taskRunner()
```
## 手写寄生组合继承
```js
function Person(name){
    this.name = name
}
Person.prototype.speak = function(){
    console.log(this.name)
}

function Student(name, age){
    Person.call(this, name)
    this.age = age
}
Student.prototype = Object.create(Person.prototype)
Student.prototype.constructor = Student
Student.prototype.newSpeak = function(){
    console.log(this.name, this.age)
}

let s1 = new Student('Jenny', 19)
s1.speak()
s1.newSpeak()
```