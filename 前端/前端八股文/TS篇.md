# type和interface的区别
## type和interface的区别
 - **type** 可以定义一个集合，包含各种类型的属性和值，来描述对象，函数，联合类型，交叉类型等
 - **interface** 定义了一个对象的形状，描述对象具有的属性以及其类型
语法不同，type是通过等号来定义类型别名，而interface是使用花括号直接定义接口成员

## type和interface的可扩展性
 - **type** 是通过联合类型（|）和交叉类型（&）进行组合，不能直接扩展其他的type
 - **interface** 它可以被扩展，可以使用extends关键词来实现接口的继承
`type`更适合定义复杂的类型别名和泛型类型，以及进行条件类型的定义，interface也是具有相似的能力，它适合描述对象的形状，定义类的契约和实现，以及接口之间的继承和扩展

## type和interface的兼容性
 - **type** 不能重复定义
 - **interface** 如果定义多个会被合并


在日常开发工作中，我们要遵循团队规范。可以用`interface`定义一个复杂的对象类型，在组合不同类型时，可以使用 `type`关键字来定义

# 用过修饰器工厂吗？
# 说说你对 TypeScript 的理解？与 JavaScript 的区别？
## 解释
`TypeScript` 是 `JavaScript` 的类型的超集，支持`ES6`语法，支持面向对象编程的概念，如类、接口、继承、泛型等
是一种静态类型检查的语言，提供了类型注解，在代码编译阶段就可以检查出数据类型的错误
同时扩展了`JavaScript` 的语法，所以任何现有的`JavaScript` 程序可以不加改变的在 `TypeScript` 下工作
为了保证兼容性，`TypeScript` 在编译阶段需要编译器编译成纯 `JavaScript` 来运行，是为大型应用之开发而设计的语言，如下：
## 特性
- **类型批注和编译时类型检查** ：在编译时批注变量类型
- **类型推断**：ts 中没有批注变量类型会自动推断变量的类型
- **类型擦除**：在编译过程中批注的内容和接口会在运行时利用工具擦除
- **接口**：ts 中用接口来定义对象类型
- **枚举**：用于取值被限定在一定范围内的场景
- **Mixin**：可以接受任意类型的值
- **泛型编程**：写代码时使用一些以后才指定的类型
- **名字空间**：名字只在该区域内有效，其他区域可重复使用该名字而不冲突
- **元组**：元组合并了不同类型的对象，相当于一个可以装不同类型数据的数组

## 与JS的区别
- TypeScript 是 JavaScript 的超集，扩展了 JavaScript 的语法
- TypeScript 可处理已有的 JavaScript 代码，并只对其中的 TypeScript 代码进行编译
- TypeScript 文件的后缀名 .ts （.ts，.tsx，.dts），JavaScript 文件是 .js
- 在编写 TypeScript 的文件的时候就会自动编译成 js 文件


# 说说 typescript 的数据类型有哪些？
- boolean（布尔类型）
- number（数字类型）
- string（字符串类型）
- array（数组类型）
- tuple（元组类型）
- enum（枚举类型）
- any（任意类型）
- null 和 undefined 类型
- void 类型
- never 类型
- object 对象类型

# 说说你对 TypeScript 中枚举类型的理解？应用场景？
枚举是一个被命名的整型常数的集合，用于声明一组命名的常数,当一个变量有几种可能的取值时,可以将它定义为枚举类型
在日常生活中也很常见，例如表示星期的SUNDAY、MONDAY、TUESDAY、WEDNESDAY、THURSDAY、FRIDAY、SATURDAY就可以看成是一个枚举
*类型分为：*
- 数字枚举
- 字符串枚举
- 异构枚举

# 说说你对 TypeScript 中接口的理解？应用场景？
**接口**是一系列抽象方法的声明，是一些方法特征的集合，这些方法都应该是抽象的，需要由具体的**类**去实现，然后第三方就可以通过这组抽象方法调用，让具体的类执行具体的方法

可选 `age?`
只读 `readonly age`
接口继承 `extends`


如果多人开发的都需要用到这个函数的时候，如果没有注释，则可能出现各种运行时的错误，这时候就可以使用接口定义参数变量

# 说说你对 TypeScript 中类的理解？应用场景？
类（Class）是面向对象程序设计（OOP，Object-Oriented Programming）实现信息封装的基础

>类是一种用户定义的引用数据类型，也称类类型

### 修饰符

可以看到，上述的形式跟`ES6`十分的相似，`typescript`在此基础上添加了三种修饰符：

- 公共 public：可以自由的访问类程序里定义的成员
- 私有 private：只能够在该类的内部进行访问
- 受保护 protect：除了在该类的内部可以访问，还可以在子类中仍然可以访问

### 抽象类

抽象类做为其它派生类的基类使用，它们一般不会直接被实例化，不同于接口，抽象类可以包含成员的实现细节
`abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法，如下所示：

```ts
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```

这种类并不能被实例化，通常需要我们创建子类去继承，如下：
```ts
class Cat extends Animal {

    makeSound() {
        console.log('miao miao')
    }
}

const cat = new Cat()

cat.makeSound() // miao miao
cat.move() // roaming the earch...
```

## 应用场景
除了日常借助类的特性完成日常业务代码，还可以将类（class）也可以作为接口，尤其在 `React` 工程中是很常用的。这就是 `class`作为接口的实际应用，我们用一个 `class` 起到了接口和设置初始值两个作用，方便统一管理，减少了代码量

# 说说你对 TypeScript 中函数的理解？与 JavaScript 函数的区别？

## 是什么？
跟`javascript` 定义函数十分相似，可以通过`funciton` 关键字、箭头函数等形式去定义，例如下面一个简单的加法函数：
```ts
const add = (a: number, b: number) => a + b
```

## 使用方式

当我们没有提供函数实现的情况下，有两种声明函数类型的方式，如下所示：
```ts
// 方式一
type LongHand = {
  (a: number): number;
};

// 方式二
type ShortHand = (a: number) => number;
```
### 可选参数

当函数的参数可能是不存在的，只需要在参数后面加上 `?` 代表参数可能不存在，如下：
```ts
const add = (a: number, b?: number) => a + (b ? b : 0)
```

### 剩余类型
剩余参数与`JavaScript`的语法类似，需要用 `...` 来表示剩余参数
如果剩余参数 `rest` 是一个由`number`类型组成的数组，则如下表示：
```ts
const add = (a: number, ...rest: number[]) => rest.reduce(((a, b) => a + b), a)
```

### 函数重载

允许创建数项名称相同但输入输出类型或个数不同的子程序，它可以简单地称为一个单独功能可以执行多项任务的能力

关于`typescript`函数重载，必须要把精确的定义放在前面，最后函数实现时，需要使用 `|`操作符或者`?`操作符，把所有可能的输入类型全部包含进去，用于具体实现

这里的函数重载也只是多个函数的声明，具体的逻辑还需要自己去写，`typescript`并不会真的将你的多个重名 `function`的函数体进行合并

例如我们有一个add函数，它可以接收 `string`类型的参数进行拼接，也可以接收 `number` 类型的参数进行相加，如下：

```ts
// 上边是声明
function add (arg1: string, arg2: string): string
function add (arg1: number, arg2: number): number
// 因为我们在下边有具体函数的实现，所以这里并不需要添加 declare 关键字

// 下边是实现
function add (arg1: string | number, arg2: string | number) {
  // 在实现上我们要注意严格判断两个参数的类型是否相等，而不能简单的写一个 arg1 + arg2
  if (typeof arg1 === 'string' && typeof arg2 === 'string') {
    return arg1 + arg2
  } else if (typeof arg1 === 'number' && typeof arg2 === 'number') {
    return arg1 + arg2
  }
}
```
## 区别

从上面可以看到：

- 从定义的方式而言，typescript 声明函数需要定义参数类型或者声明返回值类型
- typescript 在参数中，添加可选参数供使用者选择
- typescript 增添函数重载功能，使用者只需要通过查看函数声明的方式，即可知道函数传递的参数个数以及类型

# 说说你对 TypeScript 中泛型的理解？应用场景？
