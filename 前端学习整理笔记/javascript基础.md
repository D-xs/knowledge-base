## 1. 基本语法

### 1.1 编写位置

```html
<!-- 1.编写位置一: 编写在html内部(了解) -->
<a href="#" onclick="alert('百度一下')">百度一下</a>
<a href="javascript: alert('百度一下')">百度一下</a>

<!-- 2.编写位置二: 编写在script元素之内 -->
<a class="google" href="#">Google一下</a>

<script>
  var googleAEl = document.querySelector(".google")
  googleAEl.onclick = function() {
    alert("Google一下")
  }
</script>

<!-- 3.编写位置三: 独立的js文件 -->
<a class="bing" href="#">bing一下</a>
<script src="./js/bing.js"></script>
```



### 1.2 noscript标签的使用

> 当浏览器禁用script脚本的时候，noscript标签中的内容则会被渲染到界面中

```html
<noscript>
  <h1>您的浏览器不支持JavaScript, 请打开或者更换浏览器~</h1>
</noscript>

<script>
  alert("您的浏览器正在运行JavaScript代码")
</script>
```



### 1.3 js脚本的加载顺序

> 浏览器解析**html**时，是从上往下依次解析，当遇到script脚本时，会阻塞**dom**的解析（该脚本没有加**defer**和**async**属性时），会去下载js脚本，当js脚本下载完并执行完后才会继续构建dom。一般script元素是
>
> 作为body元素的最后子元素的。



### 1.4 交互方式

> 1. alert
> 2. console.log
> 3. document.write
> 4. prompt

```html
<script>
  // 1.交互方式一: alert函数
  alert("Hello World");

  // 2.交互方式二: console.log函数, 将内容输出到控制台中(console)
  // 使用最多的交互方式
  console.log("Hello Coderwhy");

  // 编写的JavaScript代码出错了
  // message.length

  // 3.交互方式三: document.write()
  document.write("Hello Kobe");

  // 4.交互方式四: prompt函数, 作用获取用户输入的内容
  var result = prompt("请输入你的名字: ");
  alert("您刚才输入的内容是:" + result);

</script>
```



### 1.5 注释

1. 单行注释
2. 多行注释
3. 文档注释 

```html
<script>
  // 1.单行注释

  // 2.多行注释
  /* 
     我是一行注释
     我是另外一行注释
    */

  // 3.文档注释
  /**
     * 和某人打招呼
     * @param {string} name 姓名
     * @param {number} age 年龄
     */
  function sayHello(name, age) {

  }

  sayHello()
</script>
```

## 2. 变量和数据结构



### 2.1 变量

> 变量：变量可以看作存放数据的容器，可以想象成一个盒子。
>
> 补充: **js**中的变量可以存放任意类型的数据（在程序运行中）。变量拥有这种行为时，这种编程语言称为"**动态类型的编程语言**"
>
> 在计算机程序中，数据需要存储到变量当中。计算机程序 = 数据结构 + 算法；数据结构相当于数据，算法相当于处理数据的一些逻辑。



### 2.2 变量声明

> 变量必须先声明，后使用（但js中是有部分不同的）
>
> 使用var 、let、const 关键字进行声明

```html
<script>

  // 定义一个变量
  // 第一步： 变量的声明（告诉js引擎接下来我要定义一个变量）
  // var关键字
  // 第二步: 变量的赋值(使用=赋值即可)
  // var currentTime = "16:00"

  // 其他的写法一:
  // var currentTime;
  // currentTime = "16:02";
  // currentTime = "17:00";

  // 其他的写法二: 同时声明多个变量(不推荐, 阅读性比较差)
  // var name, age, height
  // name = "why"
  // age = 18
  // height = 1.88
  var name = "why", age = 18, height = 1.88;

  // 补充:
  // 1.当我们打印变量时, 实际上是在打印变量中保存的值
  // 2.console.log(参数1, 参数2, 参数3...........)
  console.log(name, age, height);

</script>
```

### 2.3 变量的命名规则和命名规范



#### 命名规则

> 命名规则是必须要遵守的：表示在声明变量时，我们需要给这个变量（盒子）起一个名字，后续访问的时候可以根据这个变量名来访问。

- 变量名以**数字**、**字母**、**美元符号（$）**、**下划线（_）**组成

- 不能以**数字**开头
- 不能使用**关键字**和**保留字**作为变量名
- 严格区分大小写



#### 命名规范

> 命名规范只是大家普遍认定比较好的约束，建议遵守
>
- 多个单词使用驼峰标识；
- 赋值 = 两边都加上一个空格；
- 一条语句结束后加上分号; 也有很多人的习惯是不加；
- 变量应该做到见名知意；
```html
<script>
  // 规则: 必须遵守

  // 规范: 建议遵守
  // 1.多个单词, 建议使用驼峰标识(小驼峰)
  // 小驼峰(currentTime)/大驼峰(CurrentTime)
  // 2.推荐=的左右两边加上空格
  var currentTime = "16:18"
  var currentPrice = "¥100"

</script>
```



### 2.4 变量使用注意事项

- 如果一个变量未声明（declaration）就直接使用，那么会报错；

- 如果一个变量有声明，但是没有赋值，那么默认值是undefined
- 如果没有使用var声明变量也可以，但是不推荐（事实上会被添加到window对象上）

```html
<script>
  // 注意事项一: 如果一个变量未声明就直接使用, 那么会直接报错
  // var currentAge = age
  // var name = "why"
  // console.log(message) // 浏览器报错

  // 注意事项二: 如果一个变量有声明, 但是没有赋值操作
  // var info
  // console.log(info) // undefined

  // 注意事项三: 在JavaScript中也可以不使用var在全局声明一个变量(不推荐)
  // 如果不使用var来声明一个变量, 也是可以声明成功的, 并且这个变量会被加入window对象
  address = "广州市"
  console.log(address)

</script>
```



### 2.5 数据类型

> 什么是数据类型，js中的值都具有特定的类型。不同的数据类型有不同的用途



#### js中的数据类型

- Number
- String
- Boolean
- Undefined
- Null
- Symbol
- BigInt
- Object



#### typeof 运算符

> 因为ES的类型系统是松散的，所以需要一种手段来确定任意变量的数据类型。**typeof**运算符就运应而生了



#### 对一个值使用 typeof 操作符会返回下列字符串之一

- "undefined"表示值未定义
- "boolean"表示值为布尔值
- "string"表示值为字符串
- "number"表示值为数值
- "object"表示值为对象(而不是函数)或 null
- "function"表示值为函数
- “symbol”表示值为符号



#### Number类型

> 数字类型的值，表示整数和小数

- 基本使用

  ```js
  // 1.Number类型的基本使用
  var age = 18
  var height = 1.88
  ```

- 特殊值

  ```js
  // 2.特殊的数值
  // Infinity 表示正无穷大，-Infinity表示负无穷大
  var num1 = Infinity
  var num2 = 1 / 0
  console.log(num1, num2)
  ```

- NaN 不是一个数字

  >typeof NaN 为 'number'，isNaN函数用来检测一个值是不是NaN

  ```js
  // NaN: not a number(不是一个数字)
  var result = 3 * "abc"
  console.log(result) // NaN
  console.log(isNaN(result)) // true
  ```

- 进制表示

  ```js
  // 3.进制表示
  var num3 = 100 // 十进制
  // 了解
  var num4 = 0x100 // 十六进制
  var num5 = 0o100 // 八进制
  var num6 = 0b100 // 二进制
  console.log(num4, num5, num6) // 256 64 4
  ```

- 值表示范围

  ```js
  // 5.数字可以表示的范围
  var max = Number.MAX_VALUE
  var min = Number.MIN_VALUE
  console.log(max, min) // 1.7976931348623157e+308 5e-324
  ```

- 运算

  > 数字类型可以值可以做各种运算
  >
  > 例如：+ - * / % **等

#### String类型

> 在开发中我们经常会有一些文本需要表示，这个时候我们会使用字符串String
>
> 例如：人的姓名：dd。地址：德阳市。简介：认真是一种可怕的力量

- **JavaScript** **中的字符串必须被括在引号里，有三种包含字符串的方式。**

  - 单引号 **''**
  - 双引号 **""**
  - 反引号 **``**（es6之后）

- **前后的引号类型必须一致：**

  - 如果在字符串里面本身包括单引号，可以使用双引号；
  - 如果在字符串里面本身包括双引号，可以使用单引号；

-  **除了普通的可打印字符以外，一些有特殊功能的字符可以通过转义字符的形式放入字符串中：**

  - | 转义字符 | 表示符号 |
    | -------- | -------- |
    | \\'      | 单引号   |
    | \\"      | 双引号   |
    | \\\      | 反斜杠   |
    | \\n      | 换行符   |
    | \\r      | 回车符   |
    | \\t      | 制表符   |
    | \\b      | 退格符   |

- **<字符串>本身有的方法和属性**

  - **length**属性：表示字符串的长度
  - 字符串拼接：使用 **+** 符号

```js
// 1.String基本使用
var name = "coderwhy"
var address = "广州市"
var intro = "认真是一种可怕的力量!"

// 2.别的引号的使用
// 单引号
var message1 = 'Hello World'
// 双引号
var message2 = "Hello World"
// 反引号(ES6新增语法)
// ${变量/表达式}
var message3 = `Hello World, ${name}, ${2 + 3}`

// 3.转义字符: 字符串本身中包含引号
var message4 = 'my name is "coderwhy"'
console.log(message4)

var message5 = 'my name \\ \'\' is "coderwhy"'
console.log(message5)

// 4.<字符串>本身有的方法和属性
var message = "Hello World"
console.log(message.length)

// 字符串操作
var nickname = "coderwhy"
var info = "my name is "
var infoStr = `my name is ${nickname}` // 推荐(babel)
var infoStr2 = info + nickname
console.log(infoStr, infoStr2)

```



#### Boolean类型

> 布尔类型的值一般用来做逻辑判断使用

- 布尔类型仅包含两个值：**true** 和 **false**
- Boolean（布尔）类型用于表示**真假**
- 布尔（英语：**Boolean**）是计算机科学中的逻辑数据类型，以发明布尔代数的数学家**乔治·布尔**为名

```js
// 1.Boolean基本使用
var isLogin = false
var isAdmin = true
```



#### Undefined类型

> 如果我们声明一个变量，但是没有对其进行初始化时，它默认就是undefined；

```js
var message
var info = undefined // 不推荐

console.log(message, info)
```

**注意事项：**

- 最好在变量**定义的时候进行初始化**，而不只是声明一个变量；
- **不要显式的将一个变量赋值为undefined**
  - 如果变量刚开始什么都没有，我们可以初始化为0、空字符串、null等值；



#### Object类型

> Object 类型是一个特殊的类型，我们通常把它称为**引用类型或者复杂类型**
>
> 1. 其他的数据类型我们通常称之为 “原始类型”，因为它们的值质保函一个单独的内容（字符串、数字或者其他）；
>
> 2. Object往往可以表示一组数据，是其他数据的一个集合；
> 3. 在JavaScript中我们可以使用 花括号{} 的方式来表示一个对象；

```js
// 1.Object类型的基本使用
// var name = "why"
// var age = 18
// var height = 1.88
var person = {
  name: "why",
  age: 18,
  height: 1.88
}
console.log(person)

// 2.对象类型中某一个属性
console.log(person.name)
```



#### Null类型

> null类型通常用来表示一个对象为空，所以通常我们在给一个对象进行初始化时，会赋值为null；

- Null 类型同样只有一个值，即特殊值 null。

- null和undefined的关系

  > 1. undefined通常只有在一个变量声明但是未初始化时，它的默认值是undefined才会用到；
  > 2.  并且我们不推荐直接给一个变量赋值为undefined，所以很少主动来使用；
  > 3.  null值非常常用，当一个变量准备保存一个对象，但是这个对象不确定时，我们可以先赋值为null；

  

```js
// 3.Null类型
// 3.1. 其他类型的初始化
var age = 0
var num = 0
var message = "" // 空字符串
var isAdmin = false

// Null存在的意义就是对 对象进行初始化的, 并且在转成Boolean类型时, 会转成false
var book = null
console.log(typeof book) // object
```

