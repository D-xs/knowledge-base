# javaScript高级



## 一、 this指向

> 在 JavaScript 中，`this` 的指向取决于函数的调用方式。以下是几种常见的 `this` 指向情况：

### 1. **全局作用域中的函数调用**
   - **`this` 指向：全局对象**
   - 在非严格模式下，全局作用域中的函数的 `this` 指向 `window`（浏览器环境）或 `global`（Node.js 环境）。
   - 在严格模式下，`this` 会是 `undefined`。

   ```javascript
function showThis() {
    console.log(this);
}

showThis(); // 浏览器中输出: window
   ```

   ```javascript
'use strict';
function showThis() {
    console.log(this);
}

showThis(); // 输出: undefined
   ```

### 2. **对象方法调用**
   - **`this` 指向：调用该方法的对象**
   - 如果函数作为对象的方法调用，`this` 会指向调用该方法的对象。

   ```javascript
const obj = {
    name: 'Alice',
    greet: function() {
        console.log(this.name);
    }
};

obj.greet(); // 输出: Alice，this 指向 obj 对象
   ```

### 3. **构造函数调用**
   - **`this` 指向：新创建的实例对象**
   - 当使用 `new` 关键字调用构造函数时，`this` 指向由该构造函数创建的新的实例对象。

   ```javascript
function Person(name) {
    this.name = name;
}

const person1 = new Person('Bob');
console.log(person1.name); // 输出: Bob
   ```

### 4. **`call`/`apply`/`bind` 调用**
   - **`this` 指向：指定的对象**
   - 通过 `call` 或 `apply` 方法调用函数时，`this` 被显式地指定为第一个参数。
   - `bind` 返回一个新的函数，该函数在调用时 `this` 始终指向指定的对象。

   ```javascript
function greet() {
    console.log(this.name);
}

const obj1 = { name: 'Alice' };
const obj2 = { name: 'Bob' };

greet.call(obj1); // 输出: Alice
greet.apply(obj2); // 输出: Bob

const boundGreet = greet.bind(obj1);
boundGreet(); // 输出: Alice
   ```

### 5. **箭头函数**
   - **`this` 指向：定义箭头函数时的外层作用域**
   - 箭头函数没有自己的 `this`，它会继承定义时外层作用域的 `this`。

   ```javascript
const obj = {
    name: 'Alice',
    greet: () => {
        console.log(this.name);
    }
};

obj.greet(); // 输出: undefined (因为 this 指向的是全局作用域)
   ```

   但是在对象或方法中嵌套箭头函数时，它会继承外层函数的 `this`。

   ```javascript
const obj = {
    name: 'Alice',
    greet: function() {
        const inner = () => {
            console.log(this.name);
        };
        inner();
    }
};

obj.greet(); // 输出: Alice (因为箭头函数继承了外层函数的 this)
   ```

### 6. **事件处理函数**
   - **`this` 指向：触发事件的 DOM 元素**
   - 在事件处理函数中，`this` 默认指向触发该事件的 DOM 元素。

   ```javascript
   const button = document.querySelector('button');
   button.addEventListener('click', function() {
     console.log(this); // 输出: 被点击的 button 元素
   });
   ```

总结：

- 普通函数调用：全局对象（严格模式下为 `undefined`）。
- 对象方法调用：调用该方法的对象。
- 构造函数调用：新创建的实例对象。
- `call`/`apply`/`bind`：显式指定的对象。
- 箭头函数：继承定义时的外层作用域。
- 事件处理函数：触发事件的 DOM 元素。



## 二、this绑定规则的优先级

> 在 JavaScript 中，`this` 的绑定规则存在优先级，不同调用方式之间可能会产生冲突。以下是 `this` 绑定规则的优先级从高到低的顺序：

### 1. **`new` 绑定（构造函数调用）**
   当使用 `new` 关键字调用一个函数时，会创建一个新的对象，并将 `this` 绑定到这个新创建的对象上。此时 `new` 绑定的优先级最高，其他绑定规则会被忽略。

   ```javascript
function Person(name) {
    this.name = name;
}

const person = new Person('Alice'); // this 指向新创建的 person 对象


// 3.2. new优先级高于bind
function foo() {
    console.log("foo:", this)
}
var bindFn = foo.bind("aaa")
new bindFn()
   ```

### 2. **`call` / `apply` / `bind` 显式绑定**
   `call`、`apply` 和 `bind` 可以显式地指定 `this` 的绑定对象。显式绑定的优先级次于 `new` 绑定。如果函数是通过 `new` 关键字调用的，显式绑定会被忽略。

   - `call` 和 `apply` 会立即执行函数，并传递指定的 `this`。
   - `bind` 会创建一个新的函数，`this` 被永久绑定到指定的对象，调用时不再改变。

   ```javascript
function greet() {
    console.log(this.name);
}

const obj = { name: 'Bob' };
greet.call(obj); // this 指向 obj，输出: Bob

// 4.bind/apply优先级
// bind优先级高于apply/call
function foo() {
    console.log("foo:", this)
}
var bindFn = foo.bind("aaa")
bindFn.call("bbb")
   ```

### 3. **隐式绑定（对象方法调用）**
   如果函数作为某个对象的方法调用，`this` 会隐式绑定到该对象上。隐式绑定优先级低于 `new` 和显式绑定。

   ```javascript
const obj = {
    name: 'Charlie',
    greet: function() {
        console.log(this.name);
    }
};

obj.greet(); // this 指向 obj，输出: Charlie
   ```

   **隐式绑定丢失：**
   如果将对象方法赋值给一个变量，隐式绑定会丢失，`this` 将会回退到默认绑定（在非严格模式下为全局对象，严格模式下为 `undefined`）。

   ```javascript
   const greet = obj.greet;
   greet(); // this 指向全局对象，非严格模式下输出: undefined
   ```

### 4. **默认绑定**
   当没有任何绑定规则适用时，`this` 会采用默认绑定。在非严格模式下，默认绑定会将 `this` 指向全局对象（浏览器中是 `window`，Node.js 环境是 `global`）；而在严格模式下，`this` 会是 `undefined`。

   ```javascript
function greet() {
    console.log(this.name);
}

const name = 'Global';
greet(); // 非严格模式下，this 指向 window，输出: Global
   ```

### 5. **箭头函数绑定（词法作用域绑定）**
   箭头函数不会创建自己的 `this`，它会捕获定义时外层作用域中的 `this`，并且无法通过 `new`、`call`、`apply` 或 `bind` 改变其 `this` 指向。因此，箭头函数的 `this` 绑定具有非常特殊的行为，优先级最低，但也最稳定，因为它的 `this` 是在定义时确定的，之后不再改变。

   ```javascript
const obj = {
    name: 'David',
    greet: () => {
        console.log(this.name);
    }
};

obj.greet(); // this 指向定义箭头函数时的外层作用域（全局），输出: undefined
   ```



### 6. 其他特殊情况

   我们讲到的规则已经足以应付平时的开发，但是总有一些语法，超出了我们的规则之外。（神话故事和动漫中总是有类似这样的人物）

```js
// 1. 如果在显示绑定中，我们传入一个null或者undefined，那么这个显示绑定会被忽略，使用默认规则：
function foo() {
    console.log(this)
}

var obj = {
    name: 'why'
}

foo.call(obj) // why
foo.call(null) // window
foo.call(undefined) // window

var bar = foo.bind(null)
bar() // window


// 2. 创建一个函数的 间接引用，这种情况使用默认绑定规则
//		- 赋值(obj2.foo = obj1.foo)的结果是foo函数；
//		- foo函数被直接调用，那么是默认绑定；
function foo() {
    console.log(this)
}

var obj1 = {
    name: 'obj1',
    foo: foo
}

var obj2 = {
    name: 'obj2'
}

obj1.foo() // obj1对象
(obj2.foo = obj1.foo)() // window
```



### 综合优先级排序（从高到低）：

1. **`new` 绑定**：`new` 操作符优先创建新对象并绑定 `this`。
2. **显式绑定（`call`/`apply`/`bind`）**：通过 `call`、`apply` 和 `bind` 显式指定 `this` 的指向。
3. **隐式绑定**：通过对象方法调用时，`this` 会绑定到调用该方法的对象上。
4. **默认绑定**：在没有其他绑定规则时，`this` 指向全局对象（非严格模式）或 `undefined`（严格模式）。
5. **箭头函数**：箭头函数的 `this` 是在定义时绑定的，无法通过任何方式改变。

这个优先级规则帮助我们理解在不同场景下 `this` 的指向如何确定。例如，使用 `new` 关键字时，尽管有显式绑定，`this` 仍然会优先指向新创建的对象。