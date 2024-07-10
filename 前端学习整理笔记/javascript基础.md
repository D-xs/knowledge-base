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



#### 总结

> JavaScript 中有八种基本的数据类型（前七种为**基本数据类型，也称为原始类型**，而 object 为**复杂数据类型，也称为引用类型**）。

- number 用于任何类型的数字：整数或浮点数。
- string 用于字符串：一个字符串可以包含 0 个或多个字符，所以没有单独的单字符类型。
- boolean 用于 true 和 false。
- undefined 用于未定义的值 —— 只有一个 undefined 值的独立类型。
- object 用于更复杂的数据结构。
- null 用于未知的值 —— 只有一个 null 值的独立类型。



### 2.6 数据类型转换

**在开发中，我们可能会在不同的数据类型之间进行某些操作**

- 比如把一个String类型的数字和另外一个Number类型的数字进行运算；
- 比如把一个String类型的文本和另外一个Number类型的数字进行相加；
- 比如把一个String类型或者Number类型的内容，当做一个Boolean类型来进行判断；



**也就是在开发中，我们会经常需要对数据类型进行转换**

- 大多数情况下，**运算符**和**函数**会自动将赋予它们的值转换为正确的类型，这是一种**隐式转换**；
- 我们也可以，通过**显式**的方式来对数据类型进行转换；



#### String类型的转换

> 其他类型经常需要转换成字符串类型，比如和字符串拼接在一起或者使用字符串中的方法。



- **隐式转换**
  - 一个字符串和另一个字符串进行+操作；
    - 如果+运算符左右两边有一个是字符串，那么另一边会自动转换成字符串类型进行拼接；
  - 某些函数的执行也会自动将参数转为字符串类型；
    - 比如console.log函数；
- **显式转换**
  - 调用String()函数；
  - 调用toString()方法（面向对象再深入）



```js
var num1 = 123
var age = 18
var isAdmin = true

// 1.转换方式一: 隐式转换(用的非常多)
var num1Str = num1 + ""
var ageStr = age + ""
var isAdminStr = isAdmin + ""
console.log(typeof num1Str, typeof ageStr, typeof isAdminStr)

// 2.转换方式一: 显示转换
var num1Str2 = String(num1)
console.log(typeof num1Str2)
```



#### Number类型的转换

>其他类型也可能会转成数字类型。



- 隐式转换

  - 在**算数运算**中，通常**会将其他类型转换成数字类型**来进行运算；

    > 比如 "6" / "2"；
    >
    > 但是如果是+运算，并且其中一边有字符串，那么还是按照字符串来连接的；

- 显式转换

  - **使用Number()函数**来进行显式的转换；

- 其他类型转换数字的规则

  | 值            | 转换后的值                                                   |
  | ------------- | ------------------------------------------------------------ |
  | undefined     | NaN                                                          |
  | null          | 0                                                            |
  | true 和 false | 1 and 0                                                      |
  | string        | 去掉首尾空格后的纯数字字符串中含有的数字。如果剩余字符串为空，则转换结果为 0。否则，将会从剩余字符串中“读取”数字。当类型转换出现 error 时返回 NaN。 |

```js
// 方式一: 隐式转换(用的很少)
var num1 = "8"
var num2 = "4"
var result1 = num1 + num2 // 84
console.log(typeof result1) // string

var result2 = num1 * num2
console.log(result2) // 32

// 方式二: 显示转换(Number())
var result3 = Number(num1) // 8
console.log(typeof result3) // number

// 其他类型转成数字类型的规则:
console.log(Number(undefined)) // NaN
console.log(Number(true)) // 1
console.log(Number(false)) // 0
console.log(Number(null)) // 0
console.log(Number("abc123")) // NaN
console.log(Number("")) // 0
console.log(Number("")) // 0
```



#### Boolean类型的转换

> 布尔（boolean）类型转换是最简单的，它发生在逻辑运算中，但是也可以通过调用 Boolean(value) 显式地进行转换。



- 转换规则

  | 值                          | 转换后 |
  | --------------------------- | ------ |
  | 0，null，undefined，NaN，"" | false  |
  | 其他值                      | true   |

- **注意：包含0的字符串 "0" 是 true** 

```js
// 方式一: 隐式转换
// 分支语句
var isAdmin = true
var num1 = 123 // true

// 方式二: 显示转换
console.log(Boolean(num1), Boolean(undefined))

// 转换有如下的规则: 
// 直观上为空的值, 转成Boolean类型都是false
// 直观上为空的值: 0/""/undefined/null/NaN -> false

// 注意事项
console.log(Boolean("")) // false
console.log(Boolean("0")) // true
```



## 3. 运算符

> **运算符**就是用来做运算的符号。计算机最基本的操作就是执行运算，执行运算时就需要使用运算符来操作，编程语言按使用场景将运算符分成了以下类型：算术运算符/赋值运算符/关系(比较)运算符/逻辑运算符。



### 3.1 运算元

>运算元就是运算符应用的对象，如果一个运算符有**n个运算元**，那么这个运算符称为**n元运算符**。

- 比如说乘法运算 5 * 2，有**两个运算元**；
- 左运算元 5 和右运算元 2；
- 有时候人们也称其为“参数”；



### 3.2 算数运算符

> 算术运算符用在数学表达式中, 它的使用方式和数学中也是一致的。 
>
> 算术运算符是对数据进行计算的符号。



| **运算符** | **运算规则** |  **范例**   | **结果** |
| :--------: | :----------: | :---------: | :------: |
|     +      |     加法     |    2 + 3    |    5     |
|     +      |  连接字符串  | '中' + '国' |  '中国'  |
|     -      |     减法     |    2 - 3    |    -1    |
|     *      |     乘法     |    2 * 3    |    6     |
|     /      |     除法     |    6 / 2    |    3     |
|     %      |     取余     |    5 % 2    |    1     |
|     **     |  幂（ES7）   |   2 ** 3    |    8     |

- **取余运算符是 %，尽管它看起来很像百分数，但实际并无关联**

  ```js
  //	a % b 的结果是 a 整除 b 的余数
  console.log(10 % 3) // 1
  ```

- **求幂运算 a \** b 将 a 提升至 a 的 b 次幂。（ES7中的语法，也叫做ES2016）**

  ```js
  // 在数学中我们将其表示为 a 的 b 次方。
  console.log(2**3) // 8
  console.log(2**4) // 16
  ```

  

### 3.3 赋值运算符

> **=** 被称之为赋值（ assignments ）运算符。

-  **= 是一个运算符，而不是一个有着“魔法”作用的语言结构。**

  ```js
  var name; // undefined
  
  // 语句 x = value 将值 value 写入 x 然后返回 x。
  name = 'dd' // 'dd'
  ```

- **链式赋值（Chaining assignments）** 

  ```js
  let a, b, c;
  a = b = c = 2 + 2;
  console.log(a, b, c); // 4 4 4
  ```

  - 链式赋值**从右到左进行计算**；
  - 首先，对最右边的表达式 2 + 2 求值，然后将其赋给左边的变量：c、b 和 a。
  - 最后，所有的变量共享一个值。



### 3.4 原地修改（Modify-in-place）

> 我们经常需要对一个变量做运算，并将新的结果存储在同一个变量中。

```js
// 原写法
var n = 10;
n = n + 5;
n = n * 2;

// 原地修改
var n = 10;
n += 5;
n *= 2;

// 所有算术和位运算符都有简短的“修改并赋值”运算符：/= 和 -= 等。
```

| **运算符** |   **运算规则**    |    **范例**    | **结果** |
| :--------: | :---------------: | :------------: | :------: |
|     =      |       赋值        |     a = 5      |    5     |
|     +=     |     加后赋值      | a = 5, a += 2  |    7     |
|     -=     |     减后赋值      | a = 5, a -= 2  |    3     |
|     *=     |     乘后赋值      | a = 5; a *= 2  |    10    |
|     /=     |     除后赋值      | a = 5; a /= 2  |   2.5    |
|     %=     | 取模 (余数)后赋值 | a = 5; a %= 2  |    1     |
|    **=     |     幂后赋值      | a = 5; a **= 2 |    25    |



### 3.5 自增、自减运算符

> 对一个数进行**加一、减一**是最常见的数学运算符之一。应用于变量上的操作符
>
> **++** 和 **--**

- 将一个变量进行**自加1**或**自减1**的操作

  ```js
  var currentIndex = 5
  
  // 方法一:
  // currentIndex = currentIndex + 1
  
  // 方法二:
  // currentIndex += 1
  
  // 方法三: 自增
  currentIndex++
  console.log(currentIndex)
  
  // 自减
  // currentIndex -= 1
  currentIndex--
  console.log(currentIndex)
  ```



##### ++ 和 -- 的位置

> ++和--运算符可以在变量前面，也可以在变量后面。



相同点：都是将变量进行自加一或者自减1。

不同点：当自增自减表达式在**其他表达式中参与运算时**，**前置形式**返回**变量自增或自减后的值**，**后置形式**返回**原始值**。

```js
var currentIndex = 5
// 自己自增或者自减是没有区别
// ++currentIndex
// console.log(currentIndex) // 6
// --currentIndex
// console.log(currentIndex) // 5

// 自增和自减表达式本身又在其他的表达式中, 那就有区别
// var result1 = 100 + currentIndex++ // 100 + 5
// console.log(currentIndex)	// 6
// console.log("result1:" + result1)	// 105

var result2 = 100 + ++currentIndex // 100 + 6
console.log(currentIndex) // 6
console.log("result2:" + result2) // 106
```



### 3.6 比较运算符

> 用来比较大小的运算符

- 比较运算符的结果都是Boolean类型的

  | **运算符** | **运算规则** | **范例**  | **结果** |
  | :--------: | :----------: | :-------: | :------: |
  |     ==     |     相等     | 1 == '1'  |   true   |
  |    ===     |   严格相等   | 1 === '1' |  false   |
  |     !=     |    不相等    | 3 != '3'  |  false   |
  |    !==     |  严格不相等  | 3 !== '3' |   true   |
  |     >      |     大于     |   4 > 3   |   true   |
  |     <      |     小于     |   4 < 3   |  false   |
  |     >=     |   大于等于   |  4 >= 3   |   true   |
  |     <=     |   小于等于   |  4 <= 3   |  false   |



注意：严格相等和严格不相等运算时，当两边的值的类型不一致时，直接返回false；普通相等和不相等运算时，当两边类型不一致时，首先会将两侧的值先转化为数字，再做比较。

```js
var num1 = 20
var num2 = 30

// 1.比较运算符
var isResult = num1 > num2
console.log(isResult)

// 2.==判断
console.log(num1 == num2) // false
console.log(num1 != num2) // true

// 需求: 获取到比较大的那个值
// var result = 0
// if (num1 > num2) {
//   result = num1
// } else {
//   result = num2
// }



var foo1 = 0
var foo2 = ""

// ==运算符, 在类型不相同的情况下, 会将运算元先转成Number的值, 再进行比较(隐式转换)
// null比较特殊: null在进行比较的时候, 应该是会被当成一个对象和原生类型进行比较的
console.log(Number(foo1))
console.log(Number(foo2))
console.log(foo1 == foo2)

// ===运算符, 在类型不相同的情况, 直接返回false
console.log(foo1 === foo2)
```



### 3.7 逻辑运算符

> 逻辑运算符主要有三个，都是用来做逻辑运算，它可以将**多个表达式或者值放到一起**来获取到一个**最终的结果**；
>
> &&（与）、||（或）、！（非）



| **运算符** | **运算规则** |    **范例**     | **结果** |
| :--------: | :----------: | :-------------: | :------: |
|     &&     | 与：同时为真 |  false && true  |  false   |
|    \|\|    | 或：一个为真 | false \|\| true |   true   |
|     !      |   非：取反   |     !false      |   true   |

```js

var chineseScore = 88
var mathScore = 99

// 1.逻辑与: &&, 并且
// 条件1 && 条件2 && 条件3.....
// 所有的条件都为true的时候, 最终结果才为true
// 案例: 小明语文考试90分以上, 并且数学考试90分以上, 才能去游乐场
if (chineseScore > 90 && mathScore > 90) {
    console.log("去游乐场玩~")
}

// 2.逻辑或: ||, 或者
// 条件1 || 条件2 || 条件3....
// 只要有一个条件为true, 最终结果就为true
// 案例: 如果有一门成绩大于90, 那么可以吃打1小时游戏
if (chineseScore > 90 || mathScore > 90) {
    console.log("打1个小时游戏~")
}

// 3.逻辑非: !, 取反
var isLogin = true
if (!isLogin) {
    console.log("跳转到登录页面")
    console.log("进行登录~")
}

console.log("正常的访问页面")
```



- **逻辑与（&&）的本质**

  1. 拿到第一个运算元, 将运算元转成Boolean类型

  2. 对运算元的Boolean类型进行判断

     - 如果false, 返回运算元(原始值)
     - 如果true, 查找下一个继续来运算
     - 以此类推

  3. 如果查找了所有的都为true, 那么返回最后一个运算元(原始值)

     ```js
     // 本质推导一: 逻辑与, 称之为短路与
     var chineseScore = 80
     var mathScore = 99
     if (chineseScore > 90 && mathScore > 90) {}
     
     // 本质推导二: 对一些对象中的方法进行有值判断
     var obj = {
         name: "why",
         friend: {
             name: "kobe",
             eating: function() {
                 console.log("eat something")
             }
         }
     }
     
     // 调用eating函数
     // obj.friend.eating()
     obj && obj.friend && obj.friend.eating && obj.friend.eating()
     ```

- **逻辑或（||）的本质**

  1. 先将运算元转成Boolean类型

  2. 对转成的boolean类型进行判断

     - 如果为true, 直接将结果(原始值)返回
     - 如果为false, 进行第二个运算元的判断
     - 以此类推

  3. 如果找到最后, 也没有找到为真值的运算元, 那么返回最后一个运算元

     ```js
     // 本质推导一: 之前的多条件是如何进行判断的
     var chineseScore = 95
     var mathScore = 99
     // chineseScore > 90为true, 那么后续的条件都不会进行判断
     if (chineseScore > 90 || mathScore > 90) {}
     
     // 本质推导二: 获取第一个有值的结果
     var info = "abc"
     var obj = {name: "why"}
     var message = info || obj || "我是默认值"
     console.log(message.length)
     ```

- 逻辑非（！）的补充

  - 逻辑非运算符接受一个参数，并按如下运算：
    1. 将操作数转化为布尔类型：true/false；
    2. 返回相反的值；
  - 两个非运算**!!** 有时候用来将某个值转化为布尔类型：
    - 第一个非运算将该值转化为布尔类型并取反，第二个非运算再次取反。
    - 最后我们就得到了一个任意值到布尔值的转化。



### 3.8 条件运算符（也叫三元运算符）

> **条件（三元）运算符**是 JavaScript 唯一使用三个操作数的运算符：一个条件后跟一个问号（`?`），如果条件为[真值](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)，则执行冒号（`:`）前的表达式；若条件为[假值](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)，则执行最后的表达式。该运算符经常当作 [`if...else`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/if...else) 语句的简捷形式来使用
>
> 
>
> 语法：condition ? exprIfTrue : exprIfFalse
>
> - [`condition`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_operator#condition)
>   - 计算结果用作条件的表达式。
> - [`exprIfTrue`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_operator#expriftrue)
>   - 如果 `condition` 的计算结果为[真值](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy)（等于或可以转换为 `true` 的值），则执行该表达式。
> - [`exprIfFalse`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Conditional_operator#expriffalse)
>   - 如果 `condition` 为[假值](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy)（等于或可以转换为 `false` 的值）时执行的表达式。
>
> 
>
> 描述：除了 `false`，可能的假值表达式还有：`null`、`NaN`、`0`、空字符串（`""`）和 `undefined`。如果 `condition` 是其中任何一个，那么条件表达式的结果就是 `exprIfFalse` 表达式执行的结果。



```js
// 案例一: 比较两个数字
var num1 = 12*6 + 7*8 + 7**4
var num2 = 67*5 + 24**2

// 三元运算符
var result = num1 > num2 ? num1: num2
console.log(result)

// 案例二: 给变量赋值一个默认值(了解)
var info = {
    name: "why"
}
var obj = info ? info: {}
console.log(obj)

// 案例三: 让用户输入一个年龄, 判断是否成年人
var age = prompt("请输入您的年龄:")
age = Number(age)
// if (age >= 18) {
//   alert("成年人")
// } else {
//   alert("未成年人")
// }
var message = age >= 18 ? "成年人": "未成年人"
alert(message)
```





### 3.9 运算符的优先级

> 当一个表达式中有多个运算符时，不同运算符的优先级不一样；**运算符的优先级**决定了表达式中运算执行的先后顺序。优先级高的运算符会作为优先级低的运算符的操作数。[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_precedence)

```js
var num = 5
var result = 2 + 3 * ++num // 2 + 3 * 6
console.log(result) // 20
```



ps：想要优先运算：添加 **（）**分组运算符，优先级最高



## 4. 分支语句

> 在程序开发中，程序有三种不同的执行方式：顺序、分支、循环

- 顺序：**从上向下**，顺序执行代码
- 分支：根据**条件判断**，决定执行代码的**分支**
- 循环：让 **特定代码** **重复** 执行

```js
// 1.顺序执行
var num1 = 10
var num2 = 20

var result = num1 + num2
var result2 = num1 * num2

// 2.分支语句
var isLogin = true
if (isLogin) {
    console.log("访问购物车")
    console.log("访问个人中心")
} else { 
    console.log("跳转到登录页面")
}

// 3.循环语句
var i = 0;
while (i < 10) {
    console.log("执行循环语句")
    i++
}
```



### 4.1 代码块

> **代码块**是多行执行代码的集合，通过一个**花括号{}**放到了一起。
>
> 在开发中，一行代码很难完成某一个特定的功能，我们就会将这些代码放到一个**代码块**中
>
> **在JavaScript中，我们可以通过流程控制语句来决定如何执行一个代码块：**
>
> - 通常会通过一些关键字来告知js引擎代码要如何被执行;
> - 比如**分支语句**、**循环语句**对应的关键字等；

```js
// 一个代码块
{
    var num1 = 10;
    var num2 = 20;
    var result = num1 + num2;
}

// 一个对象
var info = {
    name: "why",
    age: 18
}
```



### 4.2 分支结构

> - 分支结构的代码就是让我们**根据条件**来决定**代码的执行**
> - 分支结构的语句被称为**判断结构**或者**选择结构**
> - 几乎**所有的编程语言都有分支结构**（C、C++、OC、JavaScript等等）
> - **JavaScript中常见的分支结构有**：**if**分支结构、**switch**分支结构



- if分支结构

  - 单分支结构

    > if(...) 语句计算括号里的条件表达式，如果计算结果是 true，就会执行对应的代码块。

    ```js
    // 如果条件成立，那么执行代码块
    // if(条件判断) {
    //    // 执行代码块
    // }
    
    
    // 案例一: 如果小明考试超过90分, 就可以去游乐场
    // 1.定义一个变量来保存小明的分数
    var score = 99
    
    // 2.如果分数超过90分, 去游乐场
    if (score > 90) {
        console.log("去游乐场玩~")
    }
    
    
    // 案例二: 苹果单价5元/斤, 如果购买的数量超过5斤, 那么立减8元
    // 1.定义一些变量保存数据
    var price = 5
    var weight = 7
    var totalPrice = price * weight
    
    // 2.根据购买的重量, 决定是否 -8
    if (weight > 5) {
        totalPrice -= 8
    }
    
    console.log("总价格:", totalPrice)
    
    
    // 案例三: 播放列表中 currentIndex
    // ["鼓楼", "理想", "阿刁"]
    var currentIndex = 2
    
    // 对currentIndex++完操作之后
    currentIndex++
    if (currentIndex === 3) {
        currentIndex = 0
    }
    
    
    
    // 补充一: 如果if语句对应的代码块, 只有一行代码, 那么{}可以省略
    // 案例二: 苹果单价5元/斤, 如果购买的数量超过5斤, 那么立减8元
    // 定义一些变量保存数据
    var price = 5
    var weight = 7
    var totalPrice = price * weight
    
    // 2.根据购买的重量, 决定是否 -8
    if (weight > 5) totalPrice -= 8
    
    console.log("总价格:", totalPrice)
    
    // 补充二: if (…) 语句会计算圆括号内的表达式，并将计算结果转换为布尔型（Boolean）
    // 转换规则和Boolean函数的规则一致；
    // 数字 0、空字符串 “”、null、undefined 和 NaN 都会被转换成 false。
    // 其他值被转换为 true，所以它们被称为“真值（truthy）”；
    var num = 123 // true
    if (num) {
        console.log("num判断的代码执行")
    }
    ```

    ps: **如果if语句对应的代码块, 只有一行代码, 那么{}可以省略**

  

  - 多分支结构

    > if..else.. 
    >
    > if 语句有时会包含一个可选的 “else” 块
    >
    > 如果判断条件不成立，就会执行它内部的代码

    ```js
    var score = 80
    // 条件成立, 专属的代码块
    // 条件不成立, 专属的代码块
    // if (score > 90) {
    //   console.log("去游乐场玩~")
    // } else {
    //   console.log("哈哈哈哈")
    // }
    
    // 案例一: 小明超过90分去游乐场, 否则去上补习班
    if (score > 90) {
        console.log("去游乐场玩~")
    } else {
        console.log("去上补习班~")
    }
    
    // 案例二: 有两个数字, 进行比较, 获取较大的数字
    var num1 = 12*6 + 7*8 + 7**4
    var num2 = 67*5 + 24**2
    console.log(num1, num2)
    
    var result = 0
    if (num1 > num2) {  
        result = num1
    } else {
        result = num2
    }
    console.log(result)
    ```

    

    >if..else if..else..
    >有时我们需要判断多个条件；我们可以通过使用 else if 子句实现；

    ```js
    // 案例: score评级
    // 1.获取用户输入的分数
    var score = prompt("请输入您的分数:")
    score = Number(score)
    
    // 2.判断等级
    // 使用if else的方式来实现
    // if (score > 90) {
    //   alert("您的评级是优秀!")
    // } else {
    //   if (score > 80) {
    //     alert("您的评级是良好!")
    //   } else {
    //   }
    // }
    
    // edge case
    // if (score > 100 || score < 0) {
    //   alert("您输入的分数超过了正常范围")
    // } 
    
    if (score > 90) {
        alert("您的评级是优秀!")
    } else if (score > 80) {
        alert("您的评级是良好!")
    } else if (score >= 60) {
        alert("您的评级是合格!")
    } else {
        alert("不及格!!!")
    }
    ```

- switch分支结构

  > 它是通过判断表达式的结果（或者变量）是否**等于**（这里使用 === 进行比较的）case语句的常量，来执行相应的分支体的；
  >
  > 与if语句不同的是，switch语句只能做值的相等判断（使用全等运算符 ===），而if语句可以做值的范围判断；
  >
  > switch 语句有至少一个 case 代码块和一个可选的 default 代码块。

  ```js
  // 语法
  // switch (表达式/变量) {
  //   case 常量1:
  //     // 语句
  // }
  
  
  // 案例:
  // 上一首的按钮: 0
  // 播放/暂停的按钮: 1
  // 下一首的按钮: 2
  // var btnIndex = 100
  // if (btnIndex === 0) {
  //   console.log("点击了上一首")
  // } else if (btnIndex === 1) {
  //   console.log("点击了播放/暂停")
  // } else if (btnIndex === 2) {
  //   console.log("点击了下一首")
  // } else {
  //   console.log("当前按钮的索引有问题~")
  // }
  
  var btnIndex = 0
  switch (btnIndex) {
      case 0:
          console.log("点击了上一首")
          break
      case 1:
          console.log("点击了播放/暂停")
          // 默认情况下是有case穿透
          break
      case 2:
          console.log("点击了下一首停")
          break
      default:
          console.log("当前按钮的索引有问题~")
          break
  }
  
  ```

  - **case穿透问题：**（不会对后续的case **表达式**进行求值）

    ```js
    switch (undefined) {
      case console.log(1):
      case console.log(2):
    }
    // 仅输出 1
    ```

    -  一条case语句结束后，会自动执行下一个case的语句；
    - 这种现象被称之为case穿透；

  - **break关键字**

    - 通过在每个case的代码块后添加break关键字来**解决case穿透**这个问题；

  - **这里的相等是严格相等（===）。**





## 5. 循环语句

> **循环** 是一种重复运行同一代码的方法。
>
> 如果是对某一个列表进行循环操作，我们通常也会称之为 遍历（traversal）或者迭代（iteration）；



js中的三种循环方式

- while循环；
- do...while循环；
- for循环；



### 5.1 while循环

```js
while(循环条件) {
    // 循环代码块
}

// 死循环
while (true) {
    console.log("Hello World")
    console.log("Hello Coderwhy")
}


// 1.练习一: 打印10次Hello World
// var count = 0
// while (count < 10) {
//   console.log("Hello World:", count)
//   count++ // 10
// }

// 2.练习二: 打印0~99的数字
// var count = 0
// while (count < 100) {
//   console.log(count)
//   count++
// }

// 3.练习三: 计算0~99的数字和
// var count = 0
// var totalCount = 0
// while (count < 100) {
//   totalCount += count
//   count++
// }
// console.log("totalCount:", totalCount)

// 4.练习四: 计算0~99的奇数和
// 如何判断一个数字是奇数还是偶数
// var num = 120
// if (num % 2 !== 0) { // 奇数
//   console.log("num是一个奇数")
// }
// var count = 0
// var totalCount = 0
// while (count < 100) {
//   if (count % 2 !== 0) {
//     totalCount += count
//   }
//   count++
// }

// console.log("所有的奇数和:", totalCount)


// 5.练习五: 计算0~99的偶数和
var count = 0
var totalCount = 0
while (count < 100) {
    if (count % 2 === 0) {
        totalCount += count
    }
    count++
}

console.log("所有的偶数和:", totalCount)

// 算法优化
var count = 0
var totalCount = 0
while (count < 100) {
    totalCount += count
    count += 2
}

console.log("所有的偶数和:", totalCount)
```



### 5.2 do...while循环

> do..while循环和while循环非常像，二者经常可以相互替代**(不常用)**
>
> 但是do..while的特点是不管条件成不成立，do循环体都会先执行一次；



```js
do {
    // 循环代码块
} while(循环条件);

// 练习一: 打印10次Hello World
var count = 0
do {
    console.log("Hello World")
    count++
} while (count < 10);

// 练习二: 计算0~99的数字和
var count = 0
var totalCount = 0
do {
    totalCount += count
    count++
} while (count < 100);
console.log("totalCount:", totalCount)
```



### 5.3  for 循环

> for 循环更加复杂，但它是**最常使用的循环形式**。

```js
for(begin; condition; step) {
    // 循环代码块
}

for(let i = 0; i < 3; i++) {
    alert(i)
}
```



|       语句段       |   例子    |                   执行过程                    |
| :----------------: | :-------: | :-------------------------------------------: |
|       begin        | let i = 0 |              进入循环时执行一次               |
|     condition      |   i < 3   | 在每次循环迭代之前检查，如果为false，停止循环 |
| body（循环代码块） | alert(i)  |             条件为真时，重复运行              |
|        step        |    i++    |            在每次循环体迭代后执行             |

**说明： begin 执行一次，然后进行迭代：每次检查 condition 后，执行 body 和 step**

```js
/*
  * 1.首先, 会先执行var count = 0;
  * 2.根据条件执行代码
    * count < 3
    * alert(count) // 0 1 2
    * count++
*/
for (var count = 0; count < 3; count++) {
    alert(count)
}

// 1.打印10次Hello World
// for (var i = 0; i < 10; i++) {
//   console.log("Hello World")
// }

// 2.打印0~99的数字
for (var i = 0; i < 100; i++) {
    console.log(i)
}

// 3.0~99的数字和
var totalCount = 0
for (var i = 0; i < 100; i++) {
    totalCount += i
}
console.log("totalCount:", totalCount)

// 4.0~99的奇数和
var totalCount = 0
for (var i = 0; i < 100; i++) {
    if (i % 2 !== 0) {
        totalCount += i
    }
}
console.log("totalCount:", totalCount)

// 算法优化
var totalCount = 0
for (var i = 1; i < 100; i+=2) {
    totalCount += i
}
console.log("totalCount:", totalCount)
```



### 5.4 循环的嵌套

> 循环中的循环体里面继续嵌套循环

```js
//	案例一：在屏幕上显示包含很多❤的矩形

// 在屏幕上显示一个❤
// document.write("❤")

// 案例一: 
for (var i = 0; i < 9; i++) {
    document.write("<div>")

    for (var m = 0; m < 10; m++) {
        document.write("❤ ")
    }

    document.write("</div>")
}
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤

//	案例二：在屏幕上显示一个三角的❤图像

for (var i = 0; i < 6; i++) {
    document.write("<div>")

    for (var m = 0; m < i+1; m++) {
        document.write("❤ ")
    }

    document.write("</div>")
}
// ❤
// ❤ ❤
// ❤ ❤ ❤
// ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤
// ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤ ❤

//	案例三：在屏幕上显示一个九九乘法表

for (var i = 0; i < 9; i++) {
    document.write("<div>")

    for (var m = 0; m < i+1; m++) {
        var a = m + 1
        var b = i + 1
        var result = (m+1)*(i+1)
        // document.write(`${a}*${b}=${result} `)
        document.write(a + "*" + b + "=" + result + " ")
    }

    document.write("</div>")
}
```



### 5.5 循环控制

> 在执行循环体的过程中，我们可能需要根据不同的条件来控制循环体的终止（break）或是跳到下一次循环（continue）。



- **break:** 直接跳出循环, 循环结束
  - break 某一条件满足时，退出循环，不再执行后续重复的代码
- **continue：**跳过本次循环次, 执行下一次循环体
  - continue 指令是 break 的“轻量版”。
  - continue 某一条件满足时，不执行后续重复的代码

```js
var names = ["abc", "cba", "nba", "mba", "bba", "aaa", "bbb"]

// 循环遍历数组
// break关键字的使用
// 需求: 遇到nba时, 不再执行后续的迭代
// for (var i = 0; i < 4; i++) {
//   console.log(names[i])
//   if (names[i] === "nba") {
//     break
//   }
// }

// continue关键字的使用: 立刻结束本次循环, 执行下一次循环(step)
// 需求: 不打印nba和cba
for (var i = 0; i < 7; i++) {
    if (names[i] === "nba" || names[i] === "cba") {
        continue
    }
    console.log(names[i])
}
```



## 6. 函数

> 函数由称为**函数体**的**一系列语句组成**。
>
> 在 JavaScript 中，函数是**头等 (\**first-class\**)对象，因为它们可以像任何其他对象**一样具有属性和方法。它们与其他对象的区别在于函数可以被调用。简而言之，它们是[`Function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function)对象。
>
> **函数**其实就是**某段代码的封装**，这段代码帮助我们完成某一个功能；
>
> 默认情况下JavaScript引擎或者浏览器会给我们**提供一些已经实现好的函数；**
>
> 我们也可以**编写属于自己的函数；**



### 6.1 函数的使用步骤

> 1. 定义函数（也称声明函数）
> 2. 使用函数



#### 1. 定义函数

> 定义函数有多种方法：函数声明、函数表达式...
>
> 声明函数的过程是对某些功能的封装过程；



- **函数声明**

  ```js
  function name([param[, param[, ... param]]]) { statements }
  ```

  `name`

  函数名

  > 函数名的命名规则和前面变量名的命名规则是相同的；
  >
  > 函数名尽量是见名知意
  >
  > 函数定义完成后，不会执行

  

  `param`

  传递给函数的参数的名称

  > 这里定义的参数为函数的形参，是用来接受该函数调用时传过来的实参，形参在函数体内当做变量来使用。
  >
  > **形参（参数 parameter）**：**定义** 函数时，小括号中的参数，是用来接收参数用的，在函数内部 **作为变量使用**
  >
  > **实参（参数 argument）**：**调用** 函数时，小括号中的参数，是用来把数据传递到 **函数内部** 用的

  

  `statements`

  组成函数体的声明语句

  > 当函数被调用时，函数体里面声明的语句会依次执行。

  ```js
  // 声明一个函数
  // 制作好一个工具, 但是这个工具默认情况下是没有被使用
  function sayHello() {
      console.log("Hello!")
      console.log("how do you do!")
  }
  
  // 调用一个函数
  sayHello()
  // 函数可以在任何你想要使用的时候, 进行调用
  sayHello()
  
  // 练习一: 定义一个函数, 打印自己的个人信息
  function printInfo() {
      console.log("my name is why")
      console.log("age is 18")
      console.log("height is 1.88")
  }
  
  printInfo()
  printInfo()
  
  // 练习二: 定义一个函数, 在内部计算10和20的和
  function sum() {
      var num1 = 10
      var num2 = 20
      var result = num1 + num2
      console.log("result:", result)
  }
  sum()
  
  // name/age/height称之为函数的参数(形参, 形式参数, parmaters)
  function printInfo(name, age, height) {
      console.log(`my name is ${name}`)
      console.log(`age is ${age}`)
      console.log(`height is ${height}`)
  }
  
  // why/18/1.88称之为函数的参数(实参, 实际参数, arguments)
  printInfo("why", 18, 1.88)
  printInfo("kobe", 30, 1.98)
  
  // 另外一个案例也做一个重构
  function sum(num1, num2) {
      var result = num1 + num2
      console.log("result:", result)
  }
  
  sum(20, 30)
  sum(123, 321)
  
  
  // 练习一: 和某人打招呼
  function sayHello(name) {
      console.log(`Hello ${name}`)
  }
  
  sayHello("Kobe")
  sayHello("James")
  sayHello("Curry")
  
  // 练习二: 给某人唱生日歌
  function singBirthdaySong(name) {
      console.log("happy birthday to you")
      console.log("happy birthday to you")
      console.log(`happy birthday to ${name}`)
      console.log("happy birthday to you")
  }
  
  singBirthdaySong("Kobe")
  singBirthdaySong("Why")
  ```



- **函数表达式**

  ```js
  let function_expression = function [name]([param1[, param2[, ..., paramN]]]) {
     statements
  };
  ```

  `name`

  函数名称。可被省略，此种情况下的函数是匿名函数（*anonymous*）。**函数名称**只是**函数体中的一个本地变量**。

  

  `paramN`

  被传递给函数的一个参数名称。一个函数至多拥有 255 个参数。

  

  `statements`

  构成函数体的语句。

  

  ```js
  // 函数的声明(声明语句)
  foo()
  function foo() {
    console.log("foo函数被执行了~")
  }
  
  // 函数的表达式
  // console.log(message) // undefined
  // var message = "why"
  
  // console.log(bar)
  bar()
  var bar = function() {
    console.log("bar函数被执行了~")
  }
  ```

  

- 函数声明和函数表达式的**异同**

  - 相同点
    - 都是定义一个函数。
    - 都是一个对象，只不过这个对象可以调用。
  - 不同点
    - **语法不同**
      - 函数声明：在主代码流中声明为**单独的语句**的函数。
      - 函数表达式：在一个表达式中或另一个语法结构中创建的函数。
    - **创建时机不同**
      - 函数声明：在函数声明定义函数之前便可被调用。
        - 这是js引擎内部算法的原因
        - 当js引擎**准备**运行脚本时，首先会在脚本中**寻找全局函数声明，并创建这些函数**；
        - 在函数内部使用函数声明的函数，也可以在声明之前调用
      - 函数表达式：函数表达式是在代码**执行到达时**被创建，并且**仅从那一刻起可用**






#### 2. 使用函数

> 通过**函数()**的形式来调用函数
>
> 函数调用时，可以传参，也可以不传；不传参数时，若该函数定义时，有形参定义，那么调用时，对应形参的值为**undefined**





#### 3. 函数返回值

> 当函数执行时，可以通过**return**关键字来返回结果
>
> 程序在函数执行时，一旦遇到**return** 关键字会立即**终止当前函数**
>
> 如果**没有使用return**关键字，函数默认返回**undefined**
>
> 如果使用return关键字，但是后面没有任何值，也返回**undefined**

```js
// var result = prompt("请输入一个数字:")
// 1.理解函数的返回值
// function sayHello(name) {
//   console.log(`Hi ${name}`)
// }

// var foo = sayHello("Kobe")
// console.log("foo:", foo)

// 2.返回值的注意事项
// 注意事项一: 所有的函数, 如果没有写返回值, 那么默认返回undefined
// function foo() {
//   console.log("foo函数被执行~")
// }

// var result = foo()
// console.log("foo的返回值:", result)

// 注意事项二: 我们也可以明确的写上return
// 写上return关键字, 但是后面什么内容都没有的时候, 也是返回undefined
// function bar() {
//   console.log("bar函数被执行~")
//   return
// }
// var result = bar()
// console.log("bar的返回值:", result)

// 注意事项三: 如果在函数执行到return关键字时, 函数会立即停止执行, 退出函数
// function baz() {
//   console.log("Hello Baz")
//   return
//   console.log("Hello World")
//   console.log("Hello Why")
// }

// baz()
```



#### 4. arguments对象

> **`arguments`** 是一个对应于传递给函数的参数的类数组对象。（有length属性，但不是数组）
>
> `arguments`对象是所有（非箭头）函数中都可用的**局部变量**。你可以使用`arguments`对象在函数中引用函数的参数。此对象包含传递给函数的每个参数，第一个参数在索引 0 处。

```js
// 1.arguments的认识
function foo(name, age) {
    console.log("传入的参数", name, age)

    // 在函数中都存在一个变量, 叫arguments
    console.log(arguments)
    // arguments是一个对象
    console.log(typeof arguments)
    // 对象内部包含了所有传入的参数
    // console.log(arguments[0])
    // console.log(arguments[1])
    // console.log(arguments[2])
    // console.log(arguments[3])

    // 对arguments来进行遍历
    for (var i = 0; i < arguments.length; i++) {
        console.log(arguments[i])
    }
}

foo("why", 18, 1.88, "广州市")


// 2.arguments的案例
function sum() {
    var total = 0
    for (var i = 0; i < arguments.length; i++) {
        var num = arguments[i]
        total += num
    }
    return total
}

console.log(sum(10, 20))
console.log(sum(10, 20, 30))
console.log(sum(10, 20, 30, 40))
```



#### 5. 递归调用

> 一种函数调用自身的操作。递归被用于处理包含有更小的子问题的一类问题。一个递归函数可以接受两个输入参数：一个最终状态（终止递归）或一个递归状态（继续递归）。
>
> 必须要有终止条件，否则会报错。

```js
// 下述代码定义了一个函数，其返回运行这段代码的 JavaScript 运行时的最大可用堆栈大小。
const getMaxCallStackSize = (i) => {
    try {
        return getMaxCallStackSize(++i);
    } catch {
        return i;
    }
};

console.log(getMaxCallStackSize(0)); // google: 10449  edge: 10454 firefox: 1513


// 什么是斐波那契数列
// 数列: 1 1 2 3 5 8 13 21 34 55  ... x
// 位置: 1 2 3 4 5 6 7  8  9  10  ... n

// 1.斐波那契的递归实现
function fibonacci(n) {
    if (n === 1 || n === 2) return 1
    return fibonacci(n-1) + fibonacci(n-2)
}


// 2.斐波那契的for循环实现
function fibonacci(n) {
    // 特殊的情况(前两个数字)
    if (n === 1 || n === 2) return 1

    // for循环的实现
    var n1 = 1
    var n2 = 1
    var result = 0
    for (var i = 3; i <= n; i++) {
        result = n1 + n2
        n1 = n2
        n2 = result
    }
    return result
}

console.log(fibonacci(5))
console.log(fibonacci(10))
console.log(fibonacci(20))
```





### 6.2 js中的函数是一等公民

> js中的函数是一等公民（头等函数）。
>
> 一种编程语言要称其函数为头等函数,通常需要满足以下几个条件:
>
> 1. 函数可以赋值给变量
> 2. 函数可以作为参数传递给其他函数
> 3. 函数可以作为其他函数的返回值
> 4. 函数可以存储在数据结构中
> 5. 函数可以在运行时创建
>
> JavaScript满足所有这些条件,因此其函数被称为头等函数。这种特性使得JavaScript在函数式编程范式中非常强大,能够实现高阶函数、闭包等高级概念,增加了语言的表达能力和灵活性。

```js
// 1. 函数可以赋值给变量:
const greet = function(name) {
  console.log(`Hello, ${name}!`);
};

// 2. 函数可以作为参数传递给其他函数
function executeFunction(fn, param) {
  fn(param);
}

executeFunction(greet, "Alice");

// 3. 函数可以作为其他函数的返回值
function createMultiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = createMultiplier(2);
console.log(double(5)); // 输出: 10

// 4. 函数可以存储在数据结构中
const functionArray = [
  function(x) { return x * 2; },
  function(x) { return x + 3; }
];

// 5. 函数可以在运行时创建
const dynamicFunction = new Function("a", "b", "return a + b");
```



### 6.3 函数式编程

> ***使用函数来作为头等公民使用函数, 这种编程方式(范式)称为函数式编程.***
>
> 函数式编程是一种编程范式，它将计算过程视为数学函数的求值，并避免改变状态和可变数据。在JavaScript中，函数式编程的核心概念包括：
>
> 1. 纯函数：给定相同的输入，总是返回相同的输出，且没有副作用。 
> 2. 不可变性：一旦创建，数据就不应被修改。 
> 3. 函数组合：将多个简单函数组合成复杂函数。
> 4. 高阶函数：接受函数作为参数或返回函数的函数。 
> 5. 声明式编程：描述要做什么，而不是如何做。



1. 纯函数

   > 纯函数总是为相同的输入返回相同的输出,并且没有副作用。

   ```js
   // 纯函数
   function add(a, b) {
     return a + b;
   }
   
   // 非纯函数 (有副作用)
   let total = 0;
   function addToTotal(value) {
     total += value;
     return total;
   }
   ```

2. 不可变性

   > 不可变性意味着创建后的数据不应被修改。在JavaScript中,我们可以使用const声明和扩展运算符来实现不可变性。

   ```js
   const originalArray = [1, 2, 3];
   
   // 不改变原数组,而是创建新数组
   const newArray = [...originalArray, 4];
   
   console.log(originalArray); // [1, 2, 3]
   console.log(newArray);      // [1, 2, 3, 4]
   ```

3. 函数组合

   > 函数组合是将多个简单函数组合成一个更复杂的函数。

   ```js
   const compose = (f, g) => x => f(g(x));
   
   const addOne = x => x + 1;
   const double = x => x * 2;
   
   const addOneThenDouble = compose(double, addOne);
   
   console.log(addOneThenDouble(3)); // 8
   ```

4. 高阶函数

   > 高阶函数是那些接受函数作为参数或返回函数的函数。

   ```js
   // 接受函数作为参数
   function applyOperation(x, y, operation) {
     return operation(x, y);
   }
   
   const result = applyOperation(5, 3, (a, b) => a + b);
   console.log(result); // 8
   
   // 返回函数
   function multiplier(factor) {
     return function(number) {
       return number * factor;
     }
   }
   
   const double = multiplier(2);
   console.log(double(4)); // 8
   ```

5. 声明式编程

   > 声明式编程关注的是描述我们想要的结果,而不是如何得到结果的具体步骤。

   ```js
   // 命令式 (how)
   const numbers = [1, 2, 3, 4, 5];
   let sum = 0;
   for (let i = 0; i < numbers.length; i++) {
     sum += numbers[i];
   }
   
   // 声明式 (what)
   const sum = numbers.reduce((acc, curr) => acc + curr, 0);
   ```

6. 柯里化

   > 柯里化是将一个接受多个参数的函数转换为一系列使用一个参数的函数。

   ```js
   // 普通函数
   function add(x, y, z) {
     return x + y + z;
   }
   
   // 柯里化后的函数
   const curriedAdd = x => y => z => x + y + z;
   
   console.log(add(1, 2, 3));          // 6
   console.log(curriedAdd(1)(2)(3));   // 6
   ```

   **应用场景：**

   > 1. 事件处理和参数预设
   > 2. 表单验证
   > 3. API请求封装
   > 4. 样式组件的属性设置
   > 5. 条件渲染优化

   ```js
   // 1. 柯里化可以帮助我们创建可重用的事件处理器，同时预设一些参数。
   const handleClick = (message) => (event) => {
       console.log(message, event.target.id);
   };
   
   // 使用
   document.getElementById('button1').addEventListener('click', handleClick('Button 1 clicked:'));
   document.getElementById('button2').addEventListener('click', handleClick('Button 2 clicked:'));
   
   
   // 2. 柯里化可以用来创建可复用的验证函数
   const validate = (regex) => (message) => (value) => {
       if (!regex.test(value)) {
           return message;
       }
   };
   
   const validateEmail = validate(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)('Invalid email address');
   const validatePhone = validate(/^\d{10}$/)('Invalid phone number');
   
   // 使用
   function validateForm(email, phone) {
       const emailError = validateEmail(email);
       const phoneError = validatePhone(phone);
   
       if (emailError) console.log(emailError);
       if (phoneError) console.log(phoneError);
   }
   
   validateForm('test@example.com', '1234567890');
   
   
   // 3. 柯里化可以用来创建更灵活的API请求函数
   const apiRequest = (baseUrl) => (endpoint) => (method) => (data) => {
       return fetch(`${baseUrl}${endpoint}`, {
           method,
           headers: {
               'Content-Type': 'application/json',
           },
           body: JSON.stringify(data),
       }).then(response => response.json());
   };
   
   const myApi = apiRequest('https://api.example.com');
   const getUsersApi = myApi('/users');
   const createUser = getUsersApi('POST');
   const updateUser = getUsersApi('PUT');
   
   // 使用
   createUser({ name: 'John Doe', email: 'john@example.com' })
       .then(response => console.log(response));
   
   updateUser({ id: 1, name: 'Jane Doe' })
       .then(response => console.log(response));
   
   
   // 4. 在使用CSS-in-JS库（如styled-components）时，柯里化可以用来创建可复用的样式设置函数
   import styled from 'styled-components';
   
   const setFlex = (direction) => (justify) => (align) => `
     display: flex;
     flex-direction: ${direction};
     justify-content: ${justify};
     align-items: ${align};
   `;
   
   const Container = styled.div`
     ${setFlex('column')('center')('flex-start')}
     width: 100%;
     height: 100vh;
   `;
   
   const Row = styled.div`
     ${setFlex('row')('space-between')('center')}
     width: 100%;
     padding: 20px;
   `;
   
   // 使用
   function App() {
       return (
           <Container>
           <Row>
           <p>Item 1</p>
           <p>Item 2</p>
           </Row>
           </Container>
       );
   }
   
   
   // 5. 柯里化可以用来创建更灵活的条件渲染函数
   const when = (predicate) => (whenTrue) => (whenFalse) => (value) =>
   predicate(value) ? whenTrue(value) : whenFalse(value);
   
   const isNotEmpty = (x) => x !== '';
   const isLongEnough = (x) => x.length >= 5;
   
   const renderInput = when(isNotEmpty)(
       when(isLongEnough)
       ((x) => <p style={{color: 'green'}}>{x} is valid</p>)
   ((x) => <p style={{color: 'orange'}}>{x} is too short</p>)
   )
   (() => <p style={{color: 'red'}}>Please enter a value</p>);
   
   // 使用
   function Form() {
       const [value, setValue] = useState('');
   
       return (
           <div>
           <input value={value} onChange={(e) => setValue(e.target.value)} />
   {renderInput(value)}
   </div>
   );
   }
   ```

   

   



#### 1. 好处

- 可预测性：纯函数和不可变数据使得代码行为更加可预测。
- 可测试性：纯函数易于单元测试。
- 并发：不可变数据和无副作用的函数使并发编程更安全。
- 模块化：通过函数组合，可以构建复杂的逻辑。
- 可读性：声明式代码通常更容易理解。
- 可维护性：函数式代码通常更容易维护和重构。



#### 2. 如何学习



##### 1. 理解基础概念

- 学习纯函数、不可变性、高阶函数等核心概念。
- 理解闭包、柯里化、组合等函数式技术。



##### 2. 实践常用方法

- 熟练使用 `map`, `filter`, `reduce` 等数组方法。
- 学习 `Promise` 和异步编程的函数式方法。



##### 3. 学习函数式库

- 尝试使用 Lodash、Ramda 等函数式工具库。
- 了解 RxJS 等响应式编程库。



##### 4. 编码练习

- 重构现有代码，应用函数式原则
- 解决编程问题时尝试使用函数式方法



##### 5. 深入学习资源

- 阅读《JavaScript函数式编程指南》等书籍
- 观看在线课程和视频教程
- 参与开源项目，学习实际应用



##### 6. 渐进式采用

- 在小型项目中逐步引入函数式概念
- 逐步增加函数式编程在日常工作中的应用



> 学习函数式编程是一个gradual的过程。建议您从基础概念开始，逐步在实际项目中应用，并不断深化理解。随着实践的增加，您会发现函数式编程能够帮助您写出更加简洁、可维护的代码。



#### 3. 应用场景（后台管理系统）

```js
// 1. 数据处理和转换
// 后台管理系统经常需要处理和转换大量数据。使用函数式编程的方法可以使这些操作更加清晰和可组合。

// 假设我们有一个用户列表，需要进行筛选、转换和排序
const users = [
    { id: 1, name: 'Alice', age: 30, role: 'admin' },
    { id: 2, name: 'Bob', age: 25, role: 'user' },
    { id: 3, name: 'Charlie', age: 35, role: 'user' },
    // ...更多用户
];

// 使用函数式方法处理数据
const processUsers = (users) => {
    return users
        .filter(user => user.age >= 18)
        .map(user => ({
        ...user,
        displayName: `${user.name} (${user.role})`
    }))
        .sort((a, b) => a.age - b.age);
};

const processedUsers = processUsers(users);


// 2. 表单验证
// 表单验证是后台管理系统的常见需求。使用函数式编程可以创建可组合的验证函数。

// 创建可组合的验证函数
const required = field => value => 
value ? null : `${field} is required`;

const minLength = (field, min) => value => 
value && value.length >= min ? null : `${field} must be at least ${min} characters`;

const isEmail = field => value => 
/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value) ? null : `${field} must be a valid email`;

// 组合验证函数
const validateUser = (user) => {
    const nameValidation = [required('Name'), minLength('Name', 2)];
    const emailValidation = [required('Email'), isEmail('Email')];

    return {
        name: nameValidation.map(validate => validate(user.name)).filter(Boolean),
        email: emailValidation.map(validate => validate(user.email)).filter(Boolean)
    };
};

// 使用
const user = { name: 'A', email: 'invalid-email' };
const errors = validateUser(user);
console.log(errors);


// 3. 状态管理
// 使用函数式编程的原则可以简化状态管理，特别是在使用 Redux 这样的库时。

// Action creators
const setUsers = users => ({ type: 'SET_USERS', payload: users });
const addUser = user => ({ type: 'ADD_USER', payload: user });
const updateUser = user => ({ type: 'UPDATE_USER', payload: user });
const deleteUser = userId => ({ type: 'DELETE_USER', payload: userId });

// Reducer
const initialState = { users: [] };

const userReducer = (state = initialState, action) => {
    switch (action.type) {
        case 'SET_USERS':
            return { ...state, users: action.payload };
        case 'ADD_USER':
            return { ...state, users: [...state.users, action.payload] };
        case 'UPDATE_USER':
            return {
                ...state,
                users: state.users.map(user => 
                                       user.id === action.payload.id ? action.payload : user
                                      )
            };
        case 'DELETE_USER':
            return {
                ...state,
                users: state.users.filter(user => user.id !== action.payload)
            };
        default:
            return state;
    }
};


// 4. API 请求封装
// 使用函数式编程可以创建更加灵活和可组合的 API 请求函数。

const apiRequest = method => endpoint => data => 
fetch(`https://api.example.com${endpoint}`, {
    method,
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data)
}).then(response => response.json());

const get = apiRequest('GET');
const post = apiRequest('POST');
const put = apiRequest('PUT');
const del = apiRequest('DELETE');

// 使用
const getUsers = get('/users');
const createUser = post('/users');
const updateUser = put('/users');
const deleteUser = del('/users');

// 示例
getUsers().then(users => console.log(users));
createUser({ name: 'New User', email: 'new@example.com' })
    .then(newUser => console.log(newUser));


// 5. 权限控制
// 函数式编程可以用于创建灵活的权限控制系统。

const hasPermission = (requiredPermission) => (user) => 
user.permissions.includes(requiredPermission);

const withPermission = (permission) => (Component) => (props) => {
    if (hasPermission(permission)(props.user)) {
        return <Component {...props} />;
    } else {
        return <div>You don't have permission to view this content.</div>;
    }
};

// 使用
const AdminPanel = withPermission('ADMIN')(({ user }) => (
    <div>Welcome, Admin {user.name}!</div>
                                          ));

// 渲染
// const user = { name: 'Alice', permissions: ['USER', 'ADMIN'] };
// <AdminPanel user={user} />


// 6. 日志和错误处理
// 使用函数式编程可以创建可组合的日志和错误处理函数。

const logError = (error) => {
    console.error('An error occurred:', error);
    // 可以在这里添加更多的错误处理逻辑，比如发送到错误追踪服务
};

const withErrorHandling = (fn) => (...args) => {
    try {
        return fn(...args);
    } catch (error) {
        logError(error);
        throw error;
    }
};

// 使用
const riskyOperation = withErrorHandling(() => {
    // 一些可能抛出错误的操作
    throw new Error('Something went wrong');
});

try {
    riskyOperation();
} catch (error) {
    console.log('Error caught in the UI layer');
}
```



### 6.4 IIFE（立即调用函数表达式）

> 是一个在定义后立即被调用的JavaScript函数。



#### 语法

```js

(function() {
    // 函数体
})();

// 或者

(() => {
    // 函数体
})();

```



#### 应用场景

```js
// 1. 创建私有作用域
const counter = (function() {
    let count = 0;
    return {
        increment: function() {
            count++;
            return count;
        },
        decrement: function() {
            count--;
            return count;
        },
        getCount: function() {
            return count;
        }
    };
})();

console.log(counter.getCount()); // 0
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.decrement()); // 1


// 2. 避免全局命名空间污染
(function($) {
    // 这里的 $ 是 jQuery，不会与其他库冲突
    $(document).ready(function() {
        // ...
    });
})(jQuery);


// 3. 模块化代码
const myModule = (function() {
    const privateVariable = 'Hello World';
    function privateMethod() {
        console.log(privateVariable);
    }

    return {
        publicMethod: function() {
            privateMethod();
        }
    };
})();

myModule.publicMethod(); // 输出: Hello World


// 4. 业务逻辑应用
// var btnEls = document.querySelectorAll(".btn")
// for (var i = 0; i < btnEls.length; i++) {
//   var btn = btnEls[i];
//   btn.onclick = function() {
//     console.log(`按钮${i+1}发生了点击`)
//   }
// }

// 使用立即执行函数
var btnEls = document.querySelectorAll(".btn")
for (var i = 0; i < btnEls.length; i++) {
    var btn = btnEls[i];
    (function(m) {
        btn.onclick = function() {
            console.log(`按钮${m+1}发生了点击`)
        }
    })(i)
}

console.log(i)
```



### 6.5 回调函数

> 回调函数是作为参数传递给另一个函数，并在某个事件发生或某个任务完成后被调用的函数。



#### 语法

```js
function doSomething(callback) {
    // 做一些事情
    callback();
}

doSomething(function() {
    console.log('回调函数被调用');
});
```



#### 应用场景

```js
// 1. 异步操作
function fetchData(callback) {
    setTimeout(() => {
        const data = { id: 1, name: 'John' };
        callback(data);
    }, 1000);
}

fetchData((data) => {
    console.log('Data received:', data);
});


// 2. 事件处理
document.getElementById('myButton').addEventListener('click', function() {
    console.log('Button clicked!');
});


// 3. 高阶函数
const numbers = [1, 2, 3, 4, 5];

const doubledNumbers = numbers.map(function(num) {
    return num * 2;
});

console.log(doubledNumbers); // [2, 4, 6, 8, 10]


// 4. 错误处理
function divideNumbers(a, b, callback) {
    if (b === 0) {
        callback(new Error('Cannot divide by zero'));
        return;
    }
    callback(null, a / b);
}

divideNumbers(10, 2, (error, result) => {
    if (error) {
        console.error('Error:', error.message);
    } else {
        console.log('Result:', result);
    }
});


// 5. 迭代器和生成器
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
}

const gen = numberGenerator();

function handleNext(result) {
    if (!result.done) {
        console.log(result.value);
        gen.next().then(handleNext);
    }
}

gen.next().then(handleNext);
```





### 6.6 js中的参数传递

> 在JavaScript中，参数传递的方式取决于传递的数据类型。

1. 基本数据类型：值传递
   - 包括 number, string, boolean, null, undefined, Symbol
   - 函数接收的是值的拷贝
   - 在函数内部对参数的修改不会影响外部变量
2. 对象类型：共享传递
   - 包括普通对象、数组、函数等
   - 函数接收的是对象引用的拷贝
   - 可以通过引用修改对象的属性，这会影响到原始对象
   - 但是，重新赋值函数内部对应形参对象不会影响原始对象
3. 数组的参数传递（按共享传递的特例）：
   - 数组作为特殊的对象，遵循对象的传递规则。
   - 可以修改数组内容（如push、pop等），会影响原始数组。
   - 重新赋值整个数组不会影响原始数组。

```js
// 1. 基本类型：值传递
function changeNumber(num) {
    num = 100;
    console.log("Inside function:", num);
}

let x = 10;
changeNumber(x);
console.log("Outside function:", x);
// 输出:
// Inside function: 100
// Outside function: 10

// 2. 对象：按共享传递
function changeObject(obj) {
    // 修改属性：会影响原对象
    obj.name = "Changed";
    console.log("Inside function (after modifying):", obj);

    // 重新赋值：不影响原对象
    obj = { name: "New Object" };
    console.log("Inside function (after reassigning):", obj);
}

let person = { name: "Original" };
changeObject(person);
console.log("Outside function:", person);
// 输出:
// Inside function (after modifying): { name: "Changed" }
// Inside function (after reassigning): { name: "New Object" }
// Outside function: { name: "Changed" }

// 3. 数组：按共享传递（特殊的对象）
function changeArray(arr) {
    // 修改数组：会影响原数组
    arr.push(4);
    console.log("Inside function (after push):", arr);

    // 重新赋值：不影响原数组
    arr = [5, 6, 7];
    console.log("Inside function (after reassigning):", arr);
}

let numbers = [1, 2, 3];
changeArray(numbers);
console.log("Outside function:", numbers);
// 输出:
// Inside function (after push): [1, 2, 3, 4]
// Inside function (after reassigning): [5, 6, 7]
// Outside function: [1, 2, 3, 4]
```



#### 按共享传递

> 1. **对象的引用被传递**： 当你将一个对象传递给函数时，实际上传递的是该对象的引用（内存地址）。
>
> 2. **可以修改对象的内容**： 在函数内部，你可以通过这个引用修改对象的属性，这些修改会反映到原始对象上。
>
> 3. **不能改变引用本身**： 虽然你可以修改对象的内容，但是你不能改变引用本身指向的对象。如果你在函数内部给参数赋予一个新的对象，这只会改变局部变量，而不会影响原始引用。

```js
// 示例1：修改对象属性
function modifyObject(obj) {
    obj.property = "modified";
}

let myObj = { property: "original" };
modifyObject(myObj);
console.log(myObj.property); // 输出: "modified"

// 示例2：重新赋值整个对象
function reassignObject(obj) {
    obj = { property: "new object" };
}

let anotherObj = { property: "original" };
reassignObject(anotherObj);
console.log(anotherObj.property); // 输出: "original"

// 示例3：数组操作
function modifyArray(arr) {
    arr.push(4);         // 修改原数组
    arr = [5, 6, 7];     // 创建新数组并赋值给局部变量arr
}

let myArray = [1, 2, 3];
modifyArray(myArray);
console.log(myArray); // 输出: [1, 2, 3, 4]
```







## 7. 作用域

> 作用域表示定义了变量的可见范围。
>
> 作用域（Scope）：定义了变量的可见性和可访问性的代码区域。
>
> 全局作用域（Global Scope）：整个程序中都可以访问的最外层作用域。
>
> 函数作用域（Function Scope）：在函数内部定义的作用域，只在该函数内部可见。
>
> 块级作用域（Block Scope）：在代码块（如if语句、for循环）内部定义的作用域。
>
> 词法作用域（Lexical Scope）：也称为静态作用域，指的是作用域是由代码中函数声明的位置来决定的，而不是由函数调用的位置决定。



```js
// 1.作用域的理解:message在哪一个范围内可以被使用, 称之为message的作用域(scope)
// 全局变量: 全局作用域
var message = "Hello World"
if (true) {
    console.log(message)
}
function foo() {
    console.log("在foo中访问", message)
}
foo()

// 2.ES5之前是没有块级作用域(var定义的变量是没有块级作用域)
{
    var count = 100
    console.log("在代码块中访问count:", count)
}
console.log("在代码块外面访问count:", count)
// for循环的代码块也是没有自己的作用域
for (var i = 0; i < 3; i++) {
    var foo = "foo"
    }
console.log("for循环外面访问foo:", foo)
console.log("for循环外面访问i:", i) // 3

// 3.ES5之前函数代码块是会形成自己的作用域
// 意味着在函数内部定义的变量外面是访问不到的
function test() {
    var bar = "bar"
    }

test()
// console.log("test函数外面访问bar:", bar)

// 函数有自己的作用域: 函数内部定义的变量只有函数内部能访问到
function sayHello() {
    var nickname = "kobe"
    console.log("sayHello函数的内部:", nickname)

    function hi() {
        console.log("hi function~")
        console.log("在hi函数中访问nickname:", nickname)
    }
    hi()
}
sayHello()
// console.log("sayHello外面访问nickname:", nickname)
```



```js
// 1.全局变量(global variable): 在全局(script元素中)定义一个变量, 那么这个变量是可以在定义之后的任何范围内被访问到的, 那么这个变量就称之为是一个全局变量.
var message = "Hello World"

// 在函数中访问message
function sayHello() {
    // 外部变量(outer variable): 在函数内部去访问函数之外的变量, 访问的变量称之为外部变量
    console.log("sayHello中访问message:", message)

    // 2.局部变量(local variable): 在函数内部定义的变量, 只有在函数内部才能进行访问, 称之为局部变量
    var nickname = "coderwhy"

    function hi() {
        console.log("hi function~")
        // message也是一个外部变量
        console.log("hi中访问message:", message)
        // nickname也是一个外部变量
        console.log("hi中访问nickname:", nickname)
    }
    hi()
}




sayHello()
```



```js
// 变量的访问顺序

// var message = "Hello World"

function sayHello() {
    // var message = "Hello Coderwhy"

    function hi() {
        // var message = "Hi Kobe"
        console.log(message)
    }
    hi()
}

sayHello()
```



**外部变量和局部变量的概念：**

- 定义在函数内部的变量，被称之为**局部变量**（Local Variables）。
- 定义在函数外部的变量，被称之为**外部变量**（Outer Variables）。



**全局变量：**

- 在函数之外声明的变量（在script中声明的），称之为全局变量。
- 全局变量**在任何函数中都是可见的**。
- 通过**var声明的全局变量会在window对象**上添加一个属性（了解）；



ps：

- 查找变量
  - 通过作用域链进行查找；若找不到，js引擎报错。
- 查找对象的属性
  - 通过对象的原型链进行查找；若找不到，返回**undefined**
- 上述规则都是**就近原则**





## 8. 对象

> 对象是包含相关数据和/或功能的集合。它们通常由多个变量和函数组成，这些变量和函数称为对象的属性和方法。



### 1. 创建对象

- 对象字面量

  ```js
  
  //	两个术语: 函数/方法
  //		函数(function): 如果在JavaScript代码中通过function默认定义一个结构, 称之为是函数.
  //		方法(method): 如果将一个函数放到对象中, 作为对象的一个属性, 那么将这个函数称之为方法.
  
  // key: 字符串类型也可以是symbol类型, 但是在定义对象的属性名时, 大部分情况下引号都是可以省略的
  var person = {
      // key: value
      name: "why",
      age: 18,
      height: 1.88,
      "my friend": {
          name: "kobe",
          age: 30
      },
      run: function() {
          console.log("running")
      },
      eat: function() {
          console.log("eat foods")
      },
      study: function() {
          console.log("studying")
      }
  }
  ```

- 构造函数

  ```js
  function Person(name, age) {
      this.name = name;
      this.age = age;
      this.greet = function() {
          console.log("Hello!");
      };
  }
  
  const john = new Person("John", 30);
  ```

- Object.create() 方法

  ```js
  const personProto = {
      greet: function() {
          console.log("Hello!");
      }
  };
  
  const person = Object.create(personProto);
  person.name = "John";
  person.age = 30;
  ```

  

### 2. 操作对象



#### 1. 访问对象属性

- 点符号

  ```js
  console.log(person.name); // "John"
  ```

- 括号符号

  ```js
  console.log(person["age"]); // 30
  
  // 方括号 + 变量方式访问
  const myAge = "age";
  console.log(person[myAge]); // 30
  ```



#### 2. 修改对象属性

```js
person.age = 31;
person["name"] = "John Doe";
```



#### 3. 添加新属性

```js
person.job = "Developer";
```



#### 4. 删除属性

```js
delete person.job;
```



#### 5. 方法

> 当函数作为对象的属性值时，就说这个函数是这个对象的方法。
>
> **方法是作为对象属性的函数。**

```js
const person = {
    name: "John",
    greet: function() {
        console.log(`Hello, my name is ${this.name}`);
    }
};

person.greet(); // 输出: "Hello, my name is John"
```



#### 6. getter和setter

```js
const person = {
    firstName: "John",
    lastName: "Doe",
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    set fullName(name) {
        const parts = name.split(' ');
        this.firstName = parts[0];
        this.lastName = parts[1];
    }
};

console.log(person.fullName); // "John Doe"
person.fullName = "Jane Smith";
console.log(person.firstName); // "Jane"
```



#### 7. Object.keys(), Object.values(), Object.entries()

```js
const person = { name: "John", age: 30 };

console.log(Object.keys(person)); // ["name", "age"]
console.log(Object.values(person)); // ["John", 30]
console.log(Object.entries(person)); // [["name", "John"], ["age", 30]]
```



#### 8. 对象解构

```js
const { name, age } = person;
console.log(name); // "John"
console.log(age);  // 30
```



#### 9. 展开运算符

```js
const person1 = { name: "John", age: 30 };
const person2 = { ...person1, job: "Developer" };
console.log(person2); // { name: "John", age: 30, job: "Developer" }
```





### 3. 遍历对象

```js
// 下述案例所要用到的对象
const person = {
    name: "John",
    age: 30,
    job: "Developer",
    city: "New York"
};
```



#### 1. for...in循环

> `for...in` 循环遍历对象的所有可枚举属性，包括原型链上的属性。

```js
for (let key in person) {
  if (person.hasOwnProperty(key)) {
    console.log(key + ": " + person[key]);
  }
}

// 注意：使用 hasOwnProperty() 方法可以确保只遍历对象自身的属性，而不是原型链上的属性。
```



#### 2. Object.keys()

> `Object.keys()` 返回一个由对象的自身可枚举属性组成的数组。

```js
Object.keys(person).forEach(key => {
  console.log(key + ": " + person[key]);
});
```



#### 3. Object.values()

> `Object.values()` 返回一个由对象的自身可枚举属性值组成的数组。

```js
Object.values(person).forEach(value => {
  console.log(value);
});
```



#### 4. Object.entries()

> `Object.entries()` 返回一个由对象的自身可枚举属性的键值对数组组成的数组。

```js
Object.entries(person).forEach(([key, value]) => {
  console.log(key + ": " + value);
});
```



#### 5. Object.getOwnPropertyNames()

> `Object.getOwnPropertyNames()` 返回一个由对象的所有自身属性的名称组成的数组，包括不可枚举属性。

```js
Object.getOwnPropertyNames(person).forEach(key => {
  console.log(key + ": " + person[key]);
});
```



#### 6. Reflect.ownKeys()

> `Reflect.ownKeys()` 返回一个由对象的所有自身属性的键组成的数组，包括符号属性和不可枚举属性。

```js
Reflect.ownKeys(person).forEach(key => {
  console.log(key + ": " + person[key]);
});
```



#### 7. Object.getOwnPropertyDescriptors()

> 这个方法返回对象的所有自身属性描述符。虽然它不是直接用于遍历，但可以用来获取所有属性，包括 getter 和 setter。

```js
const descriptors = Object.getOwnPropertyDescriptors(person);
for (let key in descriptors) {
  console.log(key + ": " + person[key]);
}
```



#### 8. 比较和建议

**for...in**: 最古老的方法，但需要注意处理原型链属性。

**Object.keys()**: 常用且简洁，只遍历自身可枚举属性。

**Object.values()**: 当只需要值而不需要键时很有用。

**Object.entries()**: 同时需要键和值时很方便，特别是在需要对对象进行转换时。

**Object.getOwnPropertyNames()**: 需要包括不可枚举属性时使用。

**Reflect.ownKeys()**: 最全面，包括符号属性，但使用较少。

**Object.getOwnPropertyDescriptors()**: 需要属性的完整描述符时使用。



**性能考虑**

> 在大多数情况下，这些方法的性能差异不大。但如果处理大型对象或需要频繁遍历，可以考虑使用 `for...in` 或 `Object.keys()` 来获得更好的性能。



**实际应用示例**

- 创建对象的浅拷贝

  ```js
  function shallowCopy(obj) {
    const copy = {};
    Object.keys(obj).forEach(key => {
      copy[key] = obj[key];
    });
    return copy;
  }
  
  const personCopy = shallowCopy(person);
  console.log(personCopy);
  ```

- 过滤对象属性

  ```js
  function filterObject(obj, predicate) {
    return Object.entries(obj).reduce((acc, [key, value]) => {
      if (predicate(value, key)) {
        acc[key] = value;
      }
      return acc;
    }, {});
  }
  
  const adult = filterObject(person, (value, key) => key === 'age' ? value >= 18 : true);
  console.log(adult);
  ```





#### 9. 可枚举属性

> 对象自身的可枚举属性是指（同时满足以下两个条件）：
>
> 1. 直接定义在对象上的属性（不是从原型链继承的）
> 2. 其内部 `enumerable` 标志设置为 `true` 的属性
>
> 这些属性在使用某些方法（如 `for...in` 循环或 `Object.keys()`）时会被包括在内。



**属性特性**

每个对象属性都有一些特性（也称为属性描述符），包括：

- `value`: 属性的值
- `writable`: 是否可以修改属性的值
- `enumerable`: 是否可枚举
- `configurable`: 是否可以删除该属性或修改其特性



**可枚举 vs 不可枚举**

1. **可枚举属性**：

   - 在 `for...in` 循环中会被遍历

   - 会被 `Object.keys()` 方法返回

   - 会被 `JSON.stringify()` 序列化

2. **不可枚举属性**：

   - 不会在 `for...in` 循环中被遍历

   - 不会被 `Object.keys()` 方法返回

   - 不会被 `JSON.stringify()` 序列化（除非显式指定）



**示例**

```js
const person = {
    name: "John",
    age: 30
};

// 添加一个不可枚举属性
Object.defineProperty(person, 'ssn', {
    value: '123-45-6789',
    enumerable: false
});

console.log(Object.keys(person)); // ['name', 'age']
console.log(person.ssn); // '123-45-6789'

for (let key in person) {
    console.log(key); // 只输出 'name' 和 'age'
}

console.log(JSON.stringify(person)); // {"name":"John","age":30}

// 在这个例子中，name 和 age 是可枚举属性，而 ssn 是不可枚举属性。
```





**检查属性是否可枚举**

> 可以使用 `propertyIsEnumerable()` 方法来检查一个属性是否可枚举

```js
console.log(person.propertyIsEnumerable('name')); // true
console.log(person.propertyIsEnumerable('ssn'));  // false
```



**获取所有属性（包括不可枚举属性）**

> 获取对象的所有属性，包括不可枚举的，可以使用 `Object.getOwnPropertyNames()`

```js
console.log(Object.getOwnPropertyNames(person)); // ['name', 'age', 'ssn']
```



**实际应用**

1. **隐藏内部实现细节**： 不可枚举属性通常用于存储不希望被外部轻易访问或修改的内部数据。
2. **避免意外遍历**： 在某些情况下，你可能不希望某些属性在遍历对象时被包括进来，使用不可枚举属性可以避免这种情况。
3. **与 API 设计相关**： 在设计库或框架时，可以使用不可枚举属性来存储内部状态或元数据，而不影响公共接口。



**创建不可枚举属性**

```js
// 创建属性时设置其为不可枚举
const obj = {};
Object.defineProperty(obj, 'hiddenProp', {
  value: 'I am hidden',
  enumerable: false
});

// 修改现有属性的可枚举性
Object.defineProperty(person, 'age', {
  value: person.age,
  enumerable: false
});
```

