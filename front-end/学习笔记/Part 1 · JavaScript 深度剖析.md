### Part 1 -模块二：ES 新特性与 TypeScript、JS 性能优化

*****

> 章节：
>
> ​	Part 1 · JavaScript 深度剖析：
>
> ​			模块二：ES 新特性与 TypeScript、JS 性能优化
>
> 概要：
>
> - ECMAScript与JavaScript
> - ECMAScript的发展过程
> - ECMAScript2015的新特性



### 2、ECMAScript概述

**********

- ECMAScript通常看做JavaScript的标准化规范

- 实际上JavaScript是ECMAScript的扩展语言

- ECMAScript只提供了最基本的语法（例如，怎么定义变量常量）

![image-20201110132619521](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20201110132619521.png)

![image-20201110132658528](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20201110132658528.png)

- JavaScript语言本身指的就是ECMAScript

- ES2015=ES6

  

  

 ![image-20201110132840084](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20201110132840084.png)



### 3、ES2015概述

“ES6”——有泛指ES2015在内往后的新版本的意思，注意分辨文中是指ES2015还是新版本

[ES2015文档]: http://www.ecma-international.org/ecma-262/6.0/

ES2015新特性（在ES5.1基础之上的变化，主要分4大类）：

- 解决原有语法上的一些问题或者不足
- 对原有语法进行增强
- 全新的对象、全新的方法、全新的功能
- 全新的数据类型和数据结构



### 4、ES2015准备工作

*****

**运行环境：**NodeJS和最新Chrome

NodeJS小工具：**Nodemon**——修改完代码后自动执行代码

**命令使用：**

###### 查看node版本

```
node --version
```

###### 打开相应项目

```
code .\01-02-01-01-es2015-spec\
```

###### 初始化一个新项目

```
yarn init
```

###### 添加nodemon依赖包到devDependencies

```
yarn add nodemon --dev
```

###### 自动执行代码（nodemon写法：nodemon替换node即可）

```
yarn nodemon 01-test.js
```

[yarn使用方法]: https://yarn.bootcss.com/docs/usage/



### 5、ES2015 let 与块级作用域

******

**作用域：**某个成员能够起作用的范围

ES2015之前，有两种作用域：

- 全局作用域
- 函数作用域

ES2015新增：

- **块级作用域：**

  - 块：用一对花括号包裹的范围，例如if、for语句中的{}

  ```
  if (true) {
  	console.log('haha,me')
  }
  
  for (var i = 0; i< 10; i++){
  	console.log('haha,me')
  }
  ```

##### let和var区别：

- 块级作用域
- 变量提升

###### 块级作用域

  ```
 // var--ES6以前块没有独立的作用域，导致块内定义的成员，外部也能访问到
 if (true) {
      var foo = 'hhh';
  }
  console.log(foo); // hhh
  
  // let-- 声明的成员只会在所声明的块中生效
  if (true) {
      let foo = 'hhh';
  }
  console.log(foo); // foo is not defined
  ```

 在 for 循环中的表现（应用场景：计数器）

```
// var--变量同名，外层for循环受影响
for (var i = 0; i < 3; i++) {
  for (var i = 0; i < 3; i++) {
    console.log(i)
  }
  console.log('内层结束   i = ' + i)
}

/*
0
1
2
内层结束   i = 3
*/

// let--变量同名，外层for循环无影响
for (var i = 0; i < 3; i++) {
  for (let i = 0; i < 3; i++) {
    console.log(i)
  }
  console.log('内层结束 i = ' + i)
}

/*
0
1
2
内层结束 i = 0
0
1
2
内层结束 i = 1
0
1
2
内层结束 i = 2
*/
```

let 应用场景：循环绑定事件，事件处理函数中获取正确索引

```
// var
var elements = [{}, {}, {}]
for (var i = 0; i < elements.length; i++) {
  elements[i].onclick = function () {
    console.log(i)
  }
}
elements[2].onclick(); // 3  原因：for循环结束后i=3,i是全局变量。

// 闭包解决--借助函数作用域摆脱全局作用域的影响？
var elements = [{}, {}, {}]
for (var i = 0; i < elements.length; i++) {
  elements[i].onclick = (function (i) {
    return function () {
      console.log(i)
    }
  })(i)
}
elements[0].onclick()

// let--块级作用域（本质也是闭包机制）
var elements = [{}, {}, {}]
for (let i = 0; i < elements.length; i++) {
  elements[i].onclick = function () {
    console.log(i)
  }
}
elements[0].onclick()

// for 循环会产生两层作用域 ----------------------------------

for (let i = 0; i < 3; i++) {
  let i = 'foo'
  console.log(i)
}

/*
foo
foo
foo
*/

// 理解-for 循环会产生两层作用域
let i = 0

if (i < 3) {
  let i = 'foo'
  console.log(i)
}

i++

if (i < 3) {
  let i = 'foo'
  console.log(i)
}

i++

if (i < 3) {
  let i = 'foo'
  console.log(i)
}

i++

/*
foo
foo
foo
*/
```

###### 变量提升

```
// let 修复了变量声明提升现象 --------------------------------------------

console.log(foo); // 变量声明提升：不报错，已声名，未定义
var foo = 'zce'
/*
undefined
*/

// console.log(foo); // 报错-不允许使用未声明的变量
// let foo = 'zce'
/*
ReferenceError: Cannot access 'foo' before initialization
*/
```



#### 6、ES2015 const

******

**const：**

- 声明恒量/常量
- 在let基础上多了只读特性

```
const name = 'zce'
// 恒量声明过后不允许重新赋值
name = 'jack' // TypeError: Assignment to constant variable. (赋值给常量变量。)
```

```
// 恒量要求声明同时赋值
const name
name = 'zce' // SyntaxError: Missing initializer in const declaration(const声明中缺少初始化式)
```

```
// 恒量只是要求内层指向不允许被修改
const obj = {}
obj = {} // TypeError: Assignment to constant variable. 

// 对于数据成员的修改是没有问题的
obj.name = 'zce'
console.log(obj); // { name: 'zce' }
```

**最佳实践：不用var，主用const，配合let**



### 7、ES2015 数组的解构

*****

**作用：**根据位置快速提取数组中的值

##### 数组解构写法

```
// 数组的解构
const arr = [100, 200, 300]

// ES6前的获取数组元素
// const foo = arr[0]
// const bar = arr[1]
// const baz = arr[2]
// console.log(foo, bar, baz)

// 解构获取全部
const [foo, bar, baz] = arr
console.log('-----',foo, bar, baz)
```

##### 获取某个位置的值：指定位置具体名，其余位置","

```
const [, , baz] = arr
console.log(baz)
```

##### 提取从当前位置开始所有的成员；只能在最后位置使用

```
const [foo, ...rest] = arr
console.log(rest)
```

##### 提取个数少于数组长度

```
const [foo, bar] = arr

console.log(arr.length, foo, bar);
```

##### 提取个数大于数组长度

```
const [foo, bar, baz, more] = arr

console.log(arr.length, more) *// 3 undefined
```

##### 默认值：未提取到对应值，则使用默认值

```
const [foo, bar, baz = 123, more = 'default value'] = arr

console.log(foo, bar, baz, more) *// 100 200 300 default value
```

##### 拆分字符串

```
const path = '/foo/bar/baz'

// const tmp = path.split('/')

// console.log(tmp)

// const rootdir = tmp[1]





const [, rootdir] = path.split('/')

console.log(rootdir)
```



### 8、ES2015 对象的解构

******

**作用：**根据属性名匹配提取对象中相应属性的值，属性名也是提取出的数据存放的变量名
**优点：**简化编写，减少代码体积

```
const obj = { name: 'zce', age: 18 }

// 对象的解构
const { name } = obj
console.log(name)

const { log } = console
log('foo')
log('bar')
log('123')

```

##### 默认值

```
const obj = { name: 'hhh', age: 18 }
const { gender = 'moren' } = obj
console.log(gender)
```

##### 重命名：变量名冲突--重命名解决冲突  语法：“属性名:重命名=默认值”

```
const obj = { name: 'zce', age: 18 }
const { name: objName = 'jack' } = obj
console.log(objName)
```



### 9、 ES2015 模板字符串

*****

作用：定义字符串

语法：反引号

```
const str = `hello es2015, this is a string`
```

支持换行

```
const str = `hello es2015,

this is a \`string\``

console.log(str)
```

插值表达式——可以通过 ${} 插入表达式，表达式的执行结果将会输出到对应位置

```
const name = 'tom'
const msg = `hey, ${name} --- ${1 + 2} ---- ${Math.random()}`
console.log(msg)
```



### 10、ES2015 带标签的模板字符串

******

**作用：** 加工模板字符串、文本多语言化、检查模板字符串中是否存在不安全、小型引擎？

**语法：**模板字符串的标签就是一个特殊的函数，使用这个标签就是调用这个函数

```
const name = 'tom'
const gender = false

function myTagFunc (strings, name, gender) {
  // console.log(strings, name, gender)
  // return '123'
  const sex = gender ? 'man' : 'woman'
  return strings[0] + name + strings[1] + sex + strings[2]
}

const result = myTagFunc`hey, ${name} is a ${gender}.`

console.log(result)
```



### 11、ES2015 字符串的扩展方法

作用：字符串查找

```
const message = 'Error: foo is not defined.'

console.log(
  // message.startsWith('Error')  // 是否是以Error开头
  // message.endsWith('.') // 是否是以.结尾
  message.includes('foo')  // 是否包含foo
)
```





### 12、 ES2015 参数默认值

**********

**作用：**设置参数默认值

**语法：** 

```
// function foo (enable) {
//   // 短路运算很多情况下是不适合判断默认参数的，例如 0 '' false null
//   // enable = enable || true
//   enable = enable === undefined ? true : enable
//   console.log('foo invoked - enable: ')
//   console.log(enable)
// }

// 默认参数一定是在形参列表的最后
function foo (enable = true) {
  console.log('foo invoked - enable: ')
  console.log(enable)
}

foo(false)
```



### 13、 ES2015 剩余参数

******

作用：函数参数不确定时

语法：只能出现在形参最后一位，只能使用一次

```
// function foo () {
//   console.log(arguments)
// }

function foo (first, ...args) {
  console.log(args) 
}

foo(1, 2, 3, 4)// [ 2, 3, 4 ]
```



### 14、 ES2015 展开数组

*******

作用：数组元素个数不固定

语法：

```
const arr = ['foo', 'bar', 'baz']

// console.log(
//   arr[0],
//   arr[1],
//   arr[2],
// )

// 以前用apply(this, arr)
// console.log.apply(console, arr)

console.log(...arr)
```



### 15、ES2015 箭头函数

**********

**作用：**简化函数定义，新增特性。。。

**语法：**

> Fira Code：编程专用的连体字体

```
// (完整参数列表)=>{函数体多条语句，返回值需 return}
// 一个参数 => 一条语句直接返回console.log('inc invoked')
const inc = (n, m) => {
  console.log('inc invoked')
  return n + 1
}
```

**场景：**简化回调函数编写

```
const arr = [1, 2, 3, 4, 5, 6, 7]

// arr.filter(function (item) {
//   return item % 2
// })

// 常用场景，回调函数
arr.filter(i => i % 2)
```

箭头函数不会改变this指向

普通函数中this时钟指向调用该函数的对象

箭头函数外边this是什么，里边拿到的this就是什么

```
// 箭头函数与 this
// 箭头函数不会改变 this 指向

const person = {
  name: 'tom',
  // sayHi: function () {
  //   console.log(`hi, my name is ${this.name}`)
  // }
  sayHi: () => {
    this.a = 'zuoyongyu';
    console.log('sayHi', this); // *****当前作用域  是sayHi{作用域}？  
    console.log(`hi, my name is ${this.name}`)
  },
  sayHiAsync: function () {
    // const _this = this // 保存当前作用域的this，闭包机制
    // setTimeout(function () {
    //   console.log(_this.name) // 这块执行作用域是全局，拿到的是全局this
    // }, 1000)

    console.log(this)
    setTimeout(() => { // 箭头函数中this始终指向当前作用域的this,当前作用域person{}?
      // console.log(this.name)
      console.log('sayHiAsync', this)
    }, 1000)
  }
}

person.sayHi()
person.sayHiAsync()
```





### 17、ES2015 对象字面量的增强

**********

- 属性名与变量名相同，可以省略
- 方法可以省略
- 计算属性名：[任意表达式] 结果作为属性名

```
const bar = '345'

const obj = {
  foo: 123,
  // bar: bar
  // 1、---属性名与变量名相同，可以省略 : bar
  bar,
  // method1: function () {
  //   console.log('method111')
  // }
  // 2、---方法可以省略 : function
  method1 () {
    console.log('method111')
    // ****这种方法就是普通的函数，同样影响 this 指向。
    console.log(this)
  },
  // Math.random(): 123 // 不允许
  // 3、---计算属性名：[任意表达式] 结果作为属性名
  [`a${bar}`]: 123
}

// obj[Math.random()] = 123

console.log(obj) // { foo: 123, bar: '345', method1: [Function: method1], a345: 123 }
obj.method1() // method111 { foo: 123, bar: '345', method1: [Function: method1], a345: 123 } 

```





### 18、ES2015 Object.assign

********

**作用：**后面对象的值覆盖前面对象的值；复制对象；浅拷贝--只对比最外层

**语法：**Object.assign(目标对象, 源对象1, 源对象...)

```

const source1 = {
  a: 123,
  b: 123
}

const source2 = {
  b: 789,
  d: 789
}

const target = {
  a: 456,
  c: 456
}

const result = Object.assign(target, source1, source2)

console.log(target) // { a: 123, c: 456, b: 789, d: 789 }
console.log(result === target) // true
```

### 19、ES2015 Object.is

********

作用：两个值是否相等

> ==    自动转换类型
>
> ===  仅比较值

```
console.log(
  // 0 == false              // => true
  // 0 === false             // => false
  // +0 === -0               // => true
  // NaN === NaN             // => false
  Object.is(+0, -0),     // => false
  Object.is(NaN, NaN)     // => true
)
```



### 20、ES2015 Proxy（代理对象）

**********

**作用：**监视对象的读写过程

ES5：Object.defineProperty

ES2015：Proxy

**语法：**

```
const person = {
  name: 'zce',
  age: 20
}

const personProxy = new Proxy(person, {
  // 监视属性读取
  get (target, property) {
    return property in target ? target[property] : 'default'
    // console.log(target, property)
    // return '读取'
  },
  // 监视属性设置
  set (target, property, value) {
    if (property === 'age') {
      if (!Number.isInteger(value)) {
        throw new TypeError(`${value} is not an int`)
      }
    }
    target[property] = value
    console.log('设置', target, property, value)
  }
})

personProxy.age = 66 // => 设置 { name: 'zce', age: 66 } age 66

personProxy.gender = true // => 设置 { name: 'zce', age: 66, gender: true } gender true

console.log(personProxy.name) // => zce
console.log(personProxy.xxx) // => default
```





### 21、ES2015 Proxy 对比 defineProperty

*********

**作用：**

- 操作对象时做特殊逻辑处理

- 统一操作方式？

**场景：**？

![image-20201117135145310](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20201117135145310.png)



- 优势1：Proxy 可以监视读写以外的操作 e.g删除属性

```
const person = {
  name: 'zce',
  age: 20
}

const personProxy = new Proxy(person, {
  deleteProperty (target, property) {
    console.log('delete', property)
    delete target[property]
  }
})

delete personProxy.age // => delete age
console.log(person) // => { name: 'zce' }
```



- 优势2：Proxy 可以很方便的监视数组操作 

```
const list = []

const listProxy = new Proxy(list, {
  set (target, property, value) {
    console.log('set', property, value)
    target[property] = value
    return true // 表示设置成功
  }
})

listProxy.push(77) // => set 0 77    ？自动显示数组长度set length 1
listProxy.push(100) // => set 1 100  自动显示数组长度set length 2  
console.log('list---', list) // => list--- [ 77, 100 ]
```



- Proxy 不需要侵入对象,不用对对象做任何操作即可监视到其内部成员的读写

```
const person = {}

Object.defineProperty(person, 'name', {
  get () {
    console.log('name 被访问')
    return person._name
  },
  set (value) {
    console.log('name 被设置')
    person._name = value
  }
})
Object.defineProperty(person, 'age', {
  get () {
    console.log('age 被访问')
    return person._age
  },
  set (value) {
    console.log('age 被设置')
    person._age = value
  }
})

person.name = 'jack' // => name 被设置

console.log(person.name) // => name 被访问   jack
```

### 22、ES2015 Reflect

********

**Reflect：**

- Reflect统一的对象操作API

- Reflect属于一个静态类（不能new，只能调用其方法）

- Reflect内部封装了一系列对对象的底层操作（13个）
- Reflect成员方法就是Proxy处理对象的默认实现

**作用：**

- 使用Proxy不定义方法时，Proxy会调用Reflect中的方法作为默认方法；？？？
- 统一提供一套用于操作对象的API

```
const obj = {
  name: 'zce',
  age: 18
}

// 传统：需使用操作符、方法等
console.log('name' in obj)
console.log(delete obj['age'])
console.log(Object.keys(obj))

// Reflect：统一用Reflect API
console.log(Reflect.has(obj, 'name'))
console.log(Reflect.deleteProperty(obj, 'age'))
console.log(Reflect.ownKeys(obj))
```



### 24、ES2015 class 类

***********

作用：？

语法：

```
// class 关键词

// 旧

// function Person (name) {
//   this.name = name
// }

// Person.prototype.say = function () {
//   console.log(`hi, my name is ${this.name}`)
// }

// ES6语法
class Person {
  constructor (name) {
    this.name = name
  }

  say () {
    console.log(`hi, my name is ${this.name}`)
  }
}

const p = new Person('tom')
p.say()
```



### 23、ES2015 静态方法

**********

类型中的方法：

- 实例方法：通过该类型构造的实例对象调用

- 静态方法： 通过类型本身调用

添加静态方法的方法：
- ES6之前--函数
- ES6--**static**添加静态成员

作用：？

语法：？

```
class Person {
  constructor (name) {
    this.name = name
  }

  say () {
    console.log(`hi, my name is ${this.name}`)
  }

  static create (name) {
    console.log(this); // => [class Person]
    return new Person(name)
  }
}

const tom = Person.create('tom')
tom.say() // => hi, my name is tom
```



### 26、**ES2015 类的继承

****

ES6之前--原型

ES6--extends

**作用：**？

**语法：**

```
// extends 继承

class Person {
  constructor (name) {
    this.name = name
  }

  say () {
    console.log(`hi, my name is ${this.name}`)
  }
}

class Student extends Person {
  constructor (name, number) {
    super(name) // 父类构造函数
    this.number = number
  }

  hello () {
    super.say() // 调用父类成员
    console.log(`my school number is ${this.number}`)
  }
}

const s = new Student('jack', '100')
s.hello()
// hi, my name is jack
// my school number is 100
```



### 27、ES2015 Set

******

作用：存放不重复的数据；数组去重

语法：创建集合、遍历集合、set方法

```
// 创建集合
const s = new Set()
s.add(1).add(2).add(3).add(4).add(2) // 链式调用
console.log(s) // Set { 1, 2, 3, 4 }

// 遍历set集合 
// ES6前
s.forEach(i => console.log(i))
// ES2015 
for (let i of s) {
  console.log(i)
}

console.log(s.size) // 集合长度 4
console.log(s.has(100)) // 集合是否存在某值 false
console.log(s.delete(3)) // 删除集合中的某值 true
console.log(s) // 打印集合Set { 1, 2, 4 }
s.clear() // 清除集合中所有
console.log(s) // Set {}

// 应用场景：数组去重
const arr = [1, 2, 1, 3, 4, 1]
// set集合->数组
const result1 = Array.from(new Set(arr)) 
const result = [...new Set(arr)]
console.log(result) // [ 1, 2, 3, 4 ]

// 弱引用版本 WeakSet
// 差异就是 Set 中会对所使用到的数据产生引用
// 即便这个数据在外面被消耗，但是由于 Set 引用了这个数据，所以依然不会回收
// 而 WeakSet 的特点就是不会产生引用，
// 一旦数据销毁，就可以被回收，所以不会产生内存泄漏问题。
```



### 28、ES2015 Map

********

**Map和Object：**

都是键值对集合

Object--键：只能是字符串，若不是内部会用toString()转换

Map--键：任意类型

**作用：**存放任意类型键值对；

**场景：**用对象存储每个学生成绩

**语法：**

```
// Object--键
const obj = {}
obj[true] = 'value'
obj[123] = 'value'
obj[{ a: 1 }] = 'value'

console.log(Object.keys(obj)) // [ '123', 'true', '[object Object]' ]  toStting()后的键
console.log(obj['[object Object]']) // value 

// Map--键
const m = new Map()
const tom = { name: 'tom' }
m.set(tom, 90)

console.log(m) // Map { { name: 'tom' } => 90 }
console.log(m.get(tom)) // 90

// m.has()
// m.delete()
// m.clear()

// 遍历值、键
m.forEach((value, key) => {
  console.log(value, key)
})

// 弱引用版本 WeakMap
// 差异就是 Map 中会对所使用到的数据产生引用
// 即便这个数据在外面被消耗，但是由于 Map 引用了这个数据，所以依然不会回收
// 而 WeakMap 的特点就是不会产生引用，
// 一旦数据销毁，就可以被回收，所以不会产生内存泄漏问题
```



### 	29、ES2015 Symbol

*******

**作用：**为对象添加独一无二的属性名

**语法：**

**场景：**扩展对象，属性名冲突问题；Symbol 模拟实现私有成员



```
// Symbol 数据类型：为对象添加独一无二的属性名

// 场景1：扩展对象，属性名冲突问题
// 方法1：相同属性名加前缀区别=========
// // shared.js ====================================
const cache = {}

// // a.js =========================================
cache['a_foo'] = Math.random()

// // b.js =========================================
cache['b_foo'] = '123'
console.log(cache)


// 方法2：Symbol========================================================
const s = Symbol()
console.log(s)
console.log(typeof s)

// 两个 Symbol 永远不会相等
console.log(
  Symbol() === Symbol()
)

// Symbol 描述文本
console.log(Symbol('foo'))
console.log(Symbol('bar'))
console.log(Symbol('baz'))

// 使用 Symbol 为对象添加用不重复的键
const obj = {}
obj[Symbol()] = '123'
obj[Symbol()] = '456'
console.log(obj) // { [Symbol()]: '123', [Symbol()]: '456' }

// 也可以在计算属性名中使用
const obj1 = {
  [Symbol()]: 123
}
console.log(obj1) // { [Symbol()]: 123 }

// =========================================================

// 案例2：Symbol 模拟实现私有成员

// a.js ======================================

const name = Symbol()
const person = {
  [name]: 'zce',
  say () {
    console.log(this[name])
  }
}
// 只对外暴露 person

// b.js =======================================

// 由于无法创建出一样的 Symbol 值，
// 所以无法直接访问到 person 中的「私有」成员
console.log(person[Symbol()]) // undefined
person.say() // zce
```



### 30、 ES2015 Symbol 补充

*******

##### 1、复用相同Symbol

```
console.log(
  // Symbol() === Symbol()
  Symbol('foo') === Symbol('foo') // false
)

// Symbol 全局注册表 ----------------------------------------------------
const s1 = Symbol.for('foo')
const s2 = Symbol.for('foo')
console.log(s1 === s2) // true
```

##### 2、内置 Symbol 常量 ? 

##### 自定义对象标签 

##### 作用：为对象实现迭代器

```
console.log(Symbol.iterator) // Symbol(Symbol.iterator)
console.log(Symbol.hasInstance) // Symbol(Symbol.hasInstance)

const obj = {
  [Symbol.toStringTag]: 'XObject',
  [Symbol.toLocaleStringTag]: 'ceshi'
}
console.log(obj.toString()); // [object XObject]
console.log(obj.toLocaleString()); // [object XObject]
```

##### 3、Symbol 属性名获取 ：Object.getOwnPropertySymbols（）

Symbol适合作私有属性

```
const obj = {
  [Symbol()]: 'symbol value',
  foo: 'normal value'
}

for (var key in obj) {
  console.log(key)
}
console.log('keys--', Object.keys(obj)) 
console.log('json--',JSON.stringify(obj))
console.log('symbols--',Object.getOwnPropertySymbols(obj)) 

// keys-- [ 'foo' ]
// json-- {"foo":"normal value"}
// symbols-- [ Symbol() ]
```



### 31、ES2015 for...of 循环

***********

遍历数据

**ES6之前：**

for——数组

for...in——键值对

forEach等——数组方法

**ES6：**

for...of——一种数据统一遍历方法

**特点：**

break随时终止遍历

每次拿到的都是当前元素本身

**场景：**

- 数组
  - forEach——无法跳出循环，必须使用 some 或者 every 方法
  - for...of——可以通过 break退出循环

```
const arr = [100, 200, 300, 400]

for (const item of arr) {
  console.log(item)
}

// for...of 循环可以替代 数组对象的 forEach 方法
// forEach——无法跳出循环，必须使用 some 或者 every 方法
const arr = [100, 200, 300, 400]
arr.forEach(item => {
  console.log(item)  
})
// arr.some()
// arr.every()

// for...of——可以通过 break退出循环
for (const item of arr) {
  console.log(item)
  if (item > 100) {
    break
  }
}


```

- 伪数组

- 元素

- set ： 遍历 Set 与遍历数组相同

```
const s = new Set(['foo', 'bar'])

for (const item of s) {
  console.log(item)
}
```

- map：遍历 Map 可以配合数组解构语法，直接获取键值

```
const m = new Map()
m.set('foo', '123')
m.set('bar', '345')

for (const [key, value] of m) {
  console.log(key, value)
}
```

- object：普通对象不能被直接 for...of 遍历

```
const obj = { foo: 123, bar: 456 }

for (const item of obj) {
  console.log(item)
}
```



### 32、ES2015 可迭代接口

***********



<u>vscode中打开浏览器控制台</u>

![image-20201125134922319](C:\Users\hp\AppData\Roaming\Typora\typora-user-images\image-20201125134922319.png)



### 33、ES2015 实现可迭代接口

**********

为object实现迭代接口

### 34、 ES2015 迭代器模式

*********

概念：设计模式——迭代器模式

作用：对外提供统一遍历接口，不用关心数据内部结构

场景：多人协同开发一个任务清单应用

实现方法：

- 回调方法

- 迭代器方法

### 35、ES2015 生成器（Generator）

************

作用：避免异步编程中回调嵌套过深，提供更好的异步编程解决方案

特点：

生成器自动返回生成器对象

生成器对象也实现了iterator   

调用对象.next()，函数的函数体才会开始执行

执行过程遇到yield关键词，执行暂停，且yield后的值作为next结果返回

继续调用next，从暂停位置继续执行直到函数执行完

惰性执行，调一下返回一下

语法：

```
*
```





### 36、 ES2015 生成器应用

*******

场景：发号器

```
function * creatIdMaker () {
	let
}
```

场景：用Generator函数实现iterator

场景：解决异步编程回调嵌套过深问题

### 37、ES2015 ES Modules

*********



### 38、ES2016 概述

##### 1、includes

```

```



##### 2、指数运算符

```
// 旧
console.log(Math.pow(2, 10));
// ES2016
console.log(2 ** 10);

```



### 39、ES2017 概述

******

