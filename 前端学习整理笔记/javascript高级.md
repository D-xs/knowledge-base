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



## 三、浏览器工作原理和浏览器内核

### 1. 浏览器工作原理

> 在浏览器中输入查找内容，浏览器是怎样将页面加载出来的？以及JavaScript代码在浏览器中是如何被执行的？

大概流程可观察以下图：

- 首先，用户在浏览器搜索栏中输入服务器地址，与服务器建立连接；
- 服务器返回对应的静态资源（一般为`index.html`）；
- 然后，浏览器拿到`index.html`后对其进行解析；
- 当解析时遇到css或js文件，就向服务器请求并下载对应的css文件和js文件；
- 最后，浏览器对页面进行渲染，执行js代码；

![image-20240924112652808](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924112652808.png)

那么在输入服务器地址，敲下回车那一刻会发生什么？

- 对浏览器输入的地址进行DNS解析，将域名解析成对应的IP地址；
- 然后向这个IP地址发送http请求，服务器收到发送的http请求，处理并响应；
- 最终浏览器得到服务器响应的内容

### 2. 浏览器内核

> 浏览器的**内核**（Rendering Engine 或称为浏览器引擎）是负责处理和渲染网页内容的核心部分。它是浏览器中的重要组件，负责解析 HTML、CSS、JavaScript 以及其他资源（如图像和视频），最终将其显示为用户可见的页面。浏览器的内核一般由两个主要部分组成：**渲染引擎** 和 **JavaScript 引擎**。（浏览器从服务器下载的文件最终都是通过浏览器的内核来进行解析的。）

以webkit浏览器为例：

- WebCore**：**负责HTML解析、布局、渲染等等相关的工作；

- JavaScriptCore**：**解析、执行JavaScript代码；

  ![image-20240924104634504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924104634504.png)

小程序中编写的JavaScript代码就是由JSCore执行的，也就是小程序使用的引擎就是JavaScriptCore：

- 渲染层：由Webview来解析和渲染wxml、wxss等；

- 逻辑层：由JSCore来解析和执行JS代码；

- 以下为小程序的官方架构图：

  ![image-20240924113949615](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924113949615.png)



### 3. **渲染引擎**

> 渲染引擎的主要职责是将 HTML、CSS 以及其他内容解析为网页，并呈现给用户。它负责构建 DOM 树、CSSOM 树，生成渲染树，进行布局和绘制。不同浏览器使用不同的渲染引擎，这导致了不同浏览器之间可能出现的页面显示差异。

#### 1.1常见的渲染引擎有：
- **WebKit**：早期被 Safari 和 Chrome 使用。Chrome 后来从 WebKit 分支出 **Blink**，成为其自家的渲染引擎。

- **Blink**：现在用于 Google Chrome 和基于 Chromium 的浏览器（如 Microsoft Edge）。

- **Gecko**：Mozilla Firefox 使用的渲染引擎。

- **Trident** 和 **EdgeHTML**：分别是 Internet Explorer 和早期版本的 Microsoft Edge 使用的引擎。Edge 后来转向使用 Blink。

  

#### 1.2渲染引擎渲染页面的详细流程:

![image-20240923104656092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923104656092.png)

1. 解析 HTML 文件，构建 DOM 树
   当浏览器接收到 HTML 文件后，渲染引擎首先开始解析 HTML 标记语言。它会根据 HTML 元素构建 **DOM 树**（Document Object Model），这是一个反映 HTML 结构的树形结构。（上图中紫色的DOM三角，是js对DOM的相关操作；）

   ![image-20240923155608981](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923155608981.png)

2. 解析 CSS 文件，构建 CSSOM 树

   在解析HTML文件的过程中，如果遇到CSS的link元素，那么会由浏览器负责下载对应的CSS文件（下载CSS文件是不会影响DOM的解析的）；浏览器下载完CSS文件后，就会对CSS文件进行解析。

   渲染引擎会加载与 HTML 相关联的所有样式资源（包括外部的 CSS 文件和嵌入在 HTML 中的样式）。CSS 文件会被解析为 **CSSOM 树**（CSS Object Model Tree），其中每个 CSS 规则与相应的 HTML 元素绑定。

   ![image-20240923155635570](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923155635570.png)

3. 生成渲染树（Render Tree）

   DOM 树和 CSSOM 树构建完毕后，渲染引擎会将这两者结合，生成 **渲染树**。渲染树的每个节点都是可见元素的“盒子”，并且带有样式信息。

   **注意一**：link元素不会阻塞DOM Tree的构建过程，但是会阻塞Render Tree的构建过程（因为Render Tree在构建时，需要对应的CSSOM Tree；）
   **注意二**：Render Tree和DOM Tree并不是一一对应的关系（比如对于display为none的元素，压根不会出现在render tree中）

   **特点**：不包含不可见的元素（如 `display: none` 的元素）；只包含需要呈现在屏幕上的节点。

   ![image-20240923155704790](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923155704790.png)

4. 布局（Layout）
   生成渲染树后，布局是在渲染树（Render Tree）上运行布局（Layout）以计算每个节点的几何体。**渲染引擎会开始进行 **布局计算，也称为 **Reflow**或 **重排**。这个阶段的目标是确定每个渲染对象的确切位置和尺寸（高度、宽度）。

   **布局过程：**

   ​	1、计算每个盒子的位置（坐标）和尺寸（宽高）。

   ​	2、从页面的根节点开始，逐层遍历所有渲染树节点。

   ​	3、布局依赖父元素的尺寸和位置，因此是自上而下的。

   ![image-20240923160714280](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923160714280.png)

5. 绘制（Painting）

   在布局完成后，浏览器会将渲染树中的节点转换为 **像素**，即将每个节点绘制到屏幕上。绘制过程由渲染引擎中的绘制模块负责，将每个节点的视觉属性（如颜色、背景、边框等）转换为图形。

   **绘制阶段分为多个步骤：**

   ​	* 背景颜色和图片的绘制。

   ​	* 边框的绘制。

   ​	* 文字的绘制。

   ​	* 其他装饰效果的绘制，如阴影、渐变等。

6. 合成层（Compositing Layers）

   在一些复杂的场景下，渲染引擎会将页面分成多个 **层**，称为 **合成层**。这些层可能是由于 CSS3 的某些属性（如 `transform`、`opacity`、`position: fixed` 等）而被单独处理。每个合成层会被独立绘制，然后进行合成，最终生成完整的页面。这是浏览器的一种优化手段。

   - 默认情况下，标准流中的内容都是被绘制在同一个图层（Layer）中的；
   - 而一些特殊的属性，会创建一个新的合成层（**CompositingLayer**），并且新的图层可以利用GPU来加速绘制。（因为每个合成层都是单独渲染的）
   - 那么哪些属性可以形成新的合成层呢？常见的一些属性
     - 3D transforms
     - video、canvas、iframe
     - opacity 动画转换时
     - position: fixed
     - will-change：一个实验性的属性，提前告诉浏览器元素可能发生哪些变化；
     - animation 或 transition 设置了opacity、transform；
   - 分层确实可以提高性能，但是它以内存管理为代价，因此不应作为 web 性能优化策略的一部分过度使用。

   为什么要使用合成层？

   - 分离绘制复杂的元素，可以提高性能，避免重绘整个页面。
   - 合成层独立于其他层更新，可以加速页面交互，例如滚动或动画效果。

   每个合成层的绘制顺序会被确定，浏览器最终将这些层合成为一个完整的图像并呈现出来。

   

   ![image-20240923163439903](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240923163439903.png)

7. 展示（Displaying）

   最终，合成好的页面会通过浏览器的绘图接口，显示在用户的屏幕上。渲染引擎会不断重复这一过程，以处理用户的交互、页面的滚动、JavaScript 代码执行等操作。如果页面内容改变，例如某个 JavaScript 改变了 DOM 树中的内容，渲染引擎会重新执行部分或全部的这些步骤。

#### 1.3 回流和重绘

**回流（Reflow）** 和 **重绘（Repaint）** 是浏览器渲染过程中涉及的两个重要概念，它们会影响网页的性能，特别是当网页发生频繁的变化时。了解它们的工作机制可以帮助开发者优化页面的渲染，减少性能瓶颈。

##### 1. **回流（Reflow）** 也称为重排（Layout）

**回流** 是当页面的布局和几何信息（如尺寸、位置）发生变化时，浏览器重新计算元素的布局并调整它们在页面中的位置的过程。回流是一个昂贵的操作，因为它需要浏览器重新计算许多元素的位置和大小。

- **第一次**确定节点的大小和位置，称之为**布局**（layout）。
- 之后对节点的大小、位置修改重新计算称之为**回流**。

###### 触发回流的常见操作：

- **添加或删除 DOM 元素**。
- **修改元素的尺寸**（如 `width`、`height`、`padding`、`border`）。
- **修改元素的位置**（如 `top`、`left`）。
- **修改元素的 `display` 属性**（如从 `none` 变为 `block`）。
- **修改页面的字体大小**。
- **窗口大小变化**（如浏览器窗口的缩放）。
- **读取元素几何信息**（如 `offsetWidth`、`offsetHeight`、`scrollTop` 等），因为浏览器为了提供准确的几何数据，会先触发回流。

###### 回流的性能影响：

- 回流会遍历整个渲染树或渲染树的某个子树，并且可能影响页面的多个部分。
- 页面元素越复杂，回流的性能开销就越大。
- 浏览器通常会通过批量处理回流来优化性能，但频繁触发回流仍会导致性能问题，特别是在动态网页或动画中。

##### 2. **重绘（Repaint）**

**重绘** 是指当元素的外观发生变化，但并不影响布局时，浏览器会重新绘制元素以反映这些变化。与回流不同，重绘不需要重新计算元素的位置和几何信息，因此它比回流的性能开销要小一些。

- **第一次**渲染内容称之为**绘制**（paint）。
- 之后重新渲染称之为**重绘**。

###### 触发重绘的常见操作：

- **修改元素的外观**，如背景颜色、边框颜色、字体颜色等。
- **修改元素的 `visibility` 属性**（从 `hidden` 变为 `visible` 或相反）。

###### 重绘的性能影响：

- 虽然重绘比回流开销小，但如果频繁修改元素的样式（例如通过动画），大量的重绘仍会影响性能，尤其是在低性能设备上。

##### 3. **回流和重绘的区别**

- **回流**：涉及布局的重新计算，影响元素的几何属性，如位置和尺寸，代价更高。
- **重绘**：只涉及元素的视觉变化，不影响布局，代价较低。

##### 4. **如何减少回流和重绘**

为了提高性能，前端开发中应尽量避免频繁触发回流和重绘。以下是一些优化建议：

1. **批量处理 DOM 操作**

   - 在避免频繁的 DOM 操作时，使用 **文档片段（DocumentFragment）** 和 **离线 DOM** 是常见的优化方法。它们的主要目的是减少对实际页面的反复修改，避免多次触发回流和重绘，从而提升性能。

    **文档片段（DocumentFragment）**

   `DocumentFragment` 是一个轻量的、无父级的 DOM 片段。当你在 `DocumentFragment` 中进行 DOM 操作时，它不会触发页面的重排（回流），因为它不是真实 DOM 树的一部分，直到你将它一次性插入到实际的 DOM 中为止。

   **使用 DocumentFragment 的示例**：

   假设我们有一个列表，想动态添加 1000 个列表项。如果直接操作 DOM，会触发 1000 次回流。而使用 `DocumentFragment` 可以一次性将 1000 个列表项插入 DOM 中，只触发一次回流。

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>DocumentFragment Example</title>
   </head>
   <body>
       <ul id="list"></ul>
   
       <script>
           const list = document.getElementById('list');
           const fragment = document.createDocumentFragment();  // 创建文档片段
   
           for (let i = 0; i < 1000; i++) {
               const listItem = document.createElement('li');
               listItem.textContent = `Item ${i + 1}`;
               fragment.appendChild(listItem);  // 将列表项添加到文档片段中
           }
   
           list.appendChild(fragment);  // 一次性将文档片段插入到 DOM 中，只触发一次回流
       </script>
   </body>
   </html>
   ```

   **执行步骤**：

   1. 创建一个 `DocumentFragment` 对象。
   2. 将新的 `li` 元素添加到 `DocumentFragment`。
   3. 最后一次性将 `DocumentFragment` 插入到实际的 DOM 中。

   这减少了对实际 DOM 的操作，只触发了一次回流，而不是 1000 次。

   **离线 DOM**

   **离线 DOM** 是指创建一个不在当前文档中的 DOM 元素（例如一个 `div`），你可以在其上进行大量的 DOM 操作，然后一次性将这个元素或其内容插入到页面中。这和 `DocumentFragment` 的思想类似，但区别在于你操作的是真实 DOM 元素。

   **使用离线 DOM 的示例**：

   和上面例子类似，我们也可以通过创建一个新的 `div` 容器，先将新的元素插入到这个离线的 `div` 中，最后再一次性将 `div` 插入页面。

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Offline DOM Example</title>
   </head>
   <body>
       <ul id="list"></ul>
   
       <script>
           const list = document.getElementById('list');
           const tempDiv = document.createElement('div');  // 创建一个离线的 div
   
           for (let i = 0; i < 1000; i++) {
               const listItem = document.createElement('li');
               listItem.textContent = `Item ${i + 1}`;
               tempDiv.appendChild(listItem);  // 将列表项添加到离线的 div 中
           }
   
           // 将离线 div 的内容一次性插入到实际 DOM 中
           list.innerHTML = tempDiv.innerHTML;
       </script>
   </body>
   </html>
   ```

   **执行步骤**：

   1. 创建一个离线的 `div` 元素。
   2. 将新的 `li` 元素添加到这个 `div` 中。
   3. 最后一次性将 `div` 的内容插入到 `ul` 中。

   这减少了对实际 DOM 的频繁操作，降低了回流次数。

   

   **总结：**

   - **DocumentFragment** 是一个轻量的虚拟 DOM 片段，它不会被插入到页面中，因此不会触发回流。通过批量操作 `DocumentFragment`，然后一次性插入页面，可以减少多次回流。
   - **离线 DOM** 通过在一个不在文档中的 DOM 元素上进行操作（如创建离线的 `div`），然后再一次性插入实际 DOM 中，也能有效避免频繁的回流操作。

   这两种方法都可以大幅度提升动态页面的性能，尤其是在处理大量 DOM 操作时。

   

2. **避免逐项修改样式**

   - 当需要修改元素的多个样式时，避免逐项修改。例如：
     ```javascript
     // 避免逐项修改
     element.style.width = "100px";
     element.style.height = "200px";
     element.style.backgroundColor = "red";
     ```
     应使用 **class** 或修改 **`style.cssText`** 一次性修改多个样式：
     ```javascript
     element.className = "new-style"; // 或者使用 class
     // 或者
     element.style.cssText = "width: 100px; height: 200px; background-color: red;";
     ```


3. **避免频繁读取导致回流的属性**
   - 读取某些属性（如 `offsetHeight`、`offsetWidth`）会强制浏览器进行回流，以确保获取到的几何信息是准确的。应避免在频繁更新页面时立即读取这些属性，尤其是在循环中。
   - 尽量避免通过getComputedStyle获取尺寸、位置等信息；


4. **使用 CSS3 动画和 GPU 加速**
   - 某些 CSS 属性，如 `transform` 和 `opacity`，可以通过 GPU 加速来进行动画，而不会触发回流和重绘。因此，优先使用这些属性进行动画操作，而不是直接修改几何属性或其他会影响布局的属性。


5. **使用 `will-change` 提示优化**

   - 可以使用 `will-change` CSS 属性来提示浏览器即将发生的变化，这样浏览器可以提前优化处理这些元素。例如：
     ```css
     .box {
       will-change: transform;
     }
     ```

6. **对某些元素使用position的absolute或者fixed**

   - 使用这两个定位方式，可以将这些元素从文档的标准流（normal flow）中移除，减少它们对其他元素布局的影响，从而降低回流和重绘的开销。

##### 总结

- **回流**：当页面的布局或几何信息发生变化时触发，性能开销较大，需避免频繁触发。
- **重绘**：当页面元素的外观发生变化但不涉及布局时触发，性能开销较小，但仍需注意避免过度重绘。
- **回流**一定会**引起重绘**，所以回流是一件很消耗性能的事情。
- **优化方法**：批量处理 DOM 操作、减少几何属性的读取、使用 CSS 动画、避免逐项修改样式等，都可以减少回流和重绘，提升网页的性能。



#### 1.4 script元素和页面解析的关系

`<script>` 元素和页面解析的关系直接影响网页的加载顺序、性能和用户体验。JavaScript 的加载、解析和执行与 HTML 文档的解析是紧密相关的，不同的 `script` 加载方式会影响浏览器的解析和渲染流程。以下是 `script` 元素和页面解析之间的关键关系：

 ##### 1. 默认情况下，JavaScript 阻塞 HTML 解析

当浏览器遇到一个普通的 `<script>` 标签时，HTML 解析会暂停，直到 JavaScript 文件下载完成并执行后才会继续。这是因为 JavaScript 可能会动态修改 DOM 结构（如通过 `document.write`），因此浏览器必须等待脚本执行完毕，以避免修改了未完全解析的 HTML 内容。

**流程**：
1. 浏览器开始解析 HTML 文档。
2. 遇到 `<script>` 标签，暂停 HTML 解析。
3. 下载并执行 JavaScript。
4. 执行完成后继续解析 HTML。

```html
<!DOCTYPE html>
<html>
<head>
    <script src="script.js"></script>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

在上述例子中，页面的解析会在遇到 `<script>` 标签时暂停，等到 `script.js` 下载并执行完后，才会继续解析剩下的 HTML（如 `<h1>` 元素）。

##### 2. `defer` 属性：延迟执行且不阻塞解析

`defer` 属性告诉浏览器在解析完整个 HTML 文档后再执行 JavaScript 文件。这种方式不会阻塞 HTML 的解析，但脚本的执行顺序仍然按照它们在文档中出现的顺序。

- **特点**：
  - 不阻塞 HTML 解析。
  - 在 DOM 树完全构建后执行，优先级低于文档解析。
  - 如果脚本提前下载好了，它会等待DOM Tree构建完成，在**DOMContentLoaded**事件之前先执行defer中的代码；
  - 通常用于需要在文档解析后操作DOM的JavaScript代码，并且对多个script文件有顺序要求的，适合用于页面加载完成后才执行的脚本。

```html
<!DOCTYPE html>
<html>
<head>
    <script src="script.js" defer></script>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

在这个例子中，浏览器会继续解析整个页面，直到 HTML 文档全部解析完毕后，再执行 `script.js`，从而加快页面的初始加载速度。

##### 3. `async` 属性：异步加载且不阻塞解析

`async` 属性会告诉浏览器异步下载 JavaScript 文件，不会阻塞 HTML 的解析。一旦脚本下载完毕，它会立即执行，而不等待 HTML 完全解析。这意味着脚本的执行顺序不一定与文档中的顺序一致。

- **特点**：
  - 不阻塞 HTML 解析。
  - 一旦脚本下载完成，立即执行，可能在 HTML 解析完成之前。
  - async不能保证在**DOMContentLoaded**之前或者之后执行；
  - 适合用于与页面无强关联、独立执行的脚本，如广告、分析工具等。

```html
<!DOCTYPE html>
<html>
<head>
    <script src="script.js" async></script>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

在这个例子中，`script.js` 会异步加载，不阻塞 HTML 的解析。然而，脚本的执行可能在页面完全解析之前发生。

##### 4. 行内脚本（Inline Script）

当 JavaScript 直接嵌入到 HTML 中时，解析器会在遇到 `<script>` 标签时立即执行该代码，仍然会阻塞 HTML 的解析。

```html
<!DOCTYPE html>
<html>
<head>
    <script>
        console.log('This will execute immediately.');
    </script>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

在此示例中，`console.log` 会在浏览器继续解析 `<h1>` 元素之前执行。

##### 5. 放置 `<script>` 标签的位置

将 `<script>` 标签放在 `<body>` 标签的底部（如 `<body>` 结束标签之前），可以减少页面加载初期的阻塞效果。这是因为 HTML 解析和渲染可以先完成，最后才下载并执行 JavaScript 文件。这种方式和 `defer` 属性的效果类似。

```html
<!DOCTYPE html>
<html>
<head>
    <title>Example</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <script src="script.js"></script>
</body>
</html>
```

此时，JavaScript 只有在页面结构已经加载完成后才会被加载和执行。

##### 总结

- **默认 `<script>`**：阻塞 HTML 解析，等待脚本加载和执行。
- **`defer`**：不会阻塞解析，页面解析完成后按顺序执行脚本。
- **`async`**：不会阻塞解析，脚本加载完成后立即执行，可能在解析结束之前。
- **放置位置**：将 `<script>` 放在 `<body>` 底部可以减少阻塞，提高加载性能。

选择适当的方式加载 JavaScript，可以显著改善页面的加载性能和用户体验。

![image-20240924113435234](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924113435234.png)

### 4. **JavaScript 引擎**

> JavaScript 引擎专门负责执行网页中的 JavaScript 代码。在现代网页中，JavaScript 用于控制动态行为，如用户交互、动画、数据请求等。JavaScript 引擎是浏览器内核的一个关键部分，负责解释和执行 JavaScript 脚本。

#### 常见的 JavaScript 引擎有：
- **V8**：V8引擎使用C++编写的Google**开源高性能**JavaScript和WebAssembly引擎，它用于Chrome和Node.js等，可以独立运行，也可以嵌入到任何C++的应用程序中。所以说V8并不单单只是服务于JavaScript的，还可以用于WebAssembly（一种用于基于堆栈的虚拟机的二进制指令格式），并且可以**运行在多个平台**。
- **SpiderMonkey**：Mozilla 开发的 JavaScript 引擎，用于 Firefox 浏览器。
- **JavaScriptCore（Nitro）**：Apple 的 WebKit 中使用的 JavaScript 引擎，主要用于 Safari。
- **Chakra**：Microsoft 开发，用于 Internet Explorer 和旧版本的 Edge 浏览器。

#### 内核的作用和工作流程

当浏览器加载网页时，内核会执行以下步骤：
1. **解析**：内核从服务器获取 HTML 文件，解析它并生成 DOM 树。
2. **样式计算**：解析 CSS 文件，构建 CSSOM 树。
3. **布局和渲染**：将 DOM 树和 CSSOM 树结合起来生成渲染树，计算页面中各个元素的布局，并绘制在屏幕上。
4. **执行 JavaScript**：JavaScript 引擎解析并执行页面中的 JavaScript 代码，可能会修改 DOM 树或 CSSOM 树，从而影响页面的渲染。

#### 内核和浏览器的区别

浏览器是整个应用程序，而内核只是其中负责渲染网页内容的部分。浏览器还包括其他组件，例如：
- **用户界面**：如地址栏、前进和后退按钮等。
- **网络层**：负责处理 HTTP 请求和响应。
- **数据存储**：管理 cookies、localStorage 等数据存储机制。

浏览器内核的工作是幕后进行的，它处理了大量技术细节，确保用户在访问网页时得到无缝的体验。



#### V8引擎的工作原理

V8 是 Google 开发的开源 JavaScript 引擎，用于执行 JavaScript 代码。它最初被设计用于 Chrome 浏览器，但后来扩展到了 Node.js 等其他环境中。V8 的核心目标是通过高效地编译和执行 JavaScript 代码来提升性能。以下是 V8 引擎执行 JavaScript 的基本原理：

![image-20240924141100767](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924141100767.png)

1. **解释与编译**

   JavaScript 是一种解释型语言，传统的 JavaScript 引擎（如早期的解释器）会逐行解析和执行代码。V8 引擎的特别之处在于，它将 JavaScript 编译成机器码来提升执行效率。它采用了**即时编译（JIT）**的方式，这意味着在执行过程中，代码会被动态编译为机器码。

   - **Full-Codegen**: 在 V8 的早期版本中，V8 使用 Full-Codegen，将 JavaScript 代码直接转化为非优化的机器码。这种方式相对简单，但性能有限。
   - **Ignition（解释器）**: Ignition 是 V8 的现代解释器，它将 JavaScript 源代码编译为一种高效的中间字节码，并且不需要立即生成机器码。
   - **TurboFan（优化编译器）**: V8 使用 TurboFan 对常用和热点代码进行优化，将这些代码编译为高效的机器码。它会通过运行时分析对代码进行优化，并将常用的代码段进一步优化。

2. **执行流程**

![image-20240924110621490](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924110621490.png)

> V8的底层架构主要有三个核心模块（Parse、Ignition和TurboFan），接下来对上面架构图进行详细说明。

   当 JavaScript 代码在 V8 中运行时，整个执行流程大致如下：
   - **Parsing（解析）**: V8 首先通过 Parser（解析器）解析 JavaScript 源代码，生成抽象语法树（AST）。AST 是代码的结构化表示，便于分析和后续处理。（如果函数没有被调用，那么是不会被转换成AST的；）

     - 该过程主要对JavaScript源代码进行词法分析和语法分析；

     - 词法分析：对代码中的每一个词或符号进行解析，最终会生成很多tokens（一个数组，里面包含很多对象）；

       - 比如，对`const name = 'curry'`这一行代码进行词法分析：

         ```js
         // 首先对const进行解析，因为const为一个关键字，所以类型会被记为一个关键词，值为const
         tokens: [
           { type: 'keyword', value: 'const' }
         ]
         
         // 接着对name进行解析，因为name为一个标识符，所以类型会被记为一个标识符，值为name
         tokens: [
           { type: 'keyword', value: 'const' },
           { type: 'identifier', value: 'name' }
         ]
         
         // 以此类推...
         ```

       - 语法分析：在词法分析的基础上，拿到tokens中的一个个对象，根据它们不同的类型再进一步分析具体语法，最终生成AST；

       - 以上即为简单的JS词法分析和语法分析过程介绍，如果想详细查看我们的JavaScript代码在通过Parse转换后的AST，可以使用[AST Explorer](https://astexplorer.net/)工具：

         ![image-20240924114709524](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924114709524.png)

       - AST在前端应用场景特别多，比如将TypeScript代码转成JavaScript代码、ES6转ES5、还有像vue中的template等，都是先将其转换成对应的AST，然后再生成目标代码；

       - 参考官方文档：https://v8.dev/blog/scanner

   - **Ignition（解释字节码）**: 解析后，代码被传递给 Ignition 解释器。Ignition 将 AST 转换为字节码，并开始解释执行。

     - 字节码（Byte-code）：是一种包含执行程序，由一序列 op 代码/数据对组成的二进制文件，是一种中间码。
     - 将JS代码转成AST是便于引擎对其进行操作，前面说到JS代码最终是转成机器码给CPU执行的，为什么还要先转换成字节码呢？
       - 因为JS运行所处的环境是不一定的，可能是windows或Linux或iOS，不同的操作系统其CPU所能识别的机器指令也是不一样的。字节码是一种中间码，本身就有跨平台的特性，然后V8引擎再根据当前所处的环境将字节码编译成对应的机器指令给当前环境的CPU执行。

     - 参考官方文档：https://v8.dev/blog/ignition-interpreter

   - **TurboFan模块**：一个编译器，可以将字节码编译为CPU认识的机器码。

     - 在了解TurboFan模块之前可以先考虑一个问题，如果每执行一次代码，就要先将AST转成字节码然后再解析成机器指令，是不是有点损耗性能呢？强大的V8早就考虑到了，所以出现了TurboFan这么一个库；
     - TurboFan可以获取到Ignition收集的一些信息，如果一个函数在代码中被多次调用，那么就会被标记为**热点函数**，然后经过TurboFan转换成优化的机器码，再次执行该函数的时候就直接执行该机器码，提高代码的执行性能；
     - 图中还存在一个`Deoptimization`（去优化）过程，其实就是**机器码被还原成ByteCode**，比如，在后续执行代码的过程中传入**热点函数的参数类型发生了变化**（如果给sum函数传入number类型的参数，那么就是做加法；如果给sum函数传入String类型的参数，那么就是做字符串拼接），可能之前优化的机器码就不能满足需求了，就会逆向转成字节码，字节码再编译成正确的机器码进行执行；
     - 从这里就可以发现，如果在编写代码时给函数传递固定类型的参数，是可以从一定程度上优化我们代码执行效率的，所以TypeScript编译出来的JavaScript代码的性能是比较好的；
     - 参考官方文档：https://v8.dev/blog/turbofan-jit






**补充：V8引擎执行过程**

> V8引擎的官方在Parse过程提供了以下这幅图，最后就来详细了解一下Parse具体的执行过程。

- ①Blink内核将JS源码交给V8引擎；

- ②Stream获取到JS源码进行**编码转换**；

- ③Scanner进行词法分析，将代码转换成tokens；

- ④经过语法分析后，tokens会被转换成AST，中间会经过Parser和PreParser过程：

  - Parser：直接解析，将tokens转成AST树；
  - PreParser：预解析（为什么需要预解析？）
    - 因为并不是所有的JavaScript代码，在一开始时就会执行的，如果一股脑对所有JavaScript代码进行解析，必然会影响性能，所以V8就实现了**Lazy Parsing（延迟解析）**方案，对不必要的函数代码进行预解析，也就是先解析急需要执行的代码内容，对函数的**全量解析**会放到函数被调用时进行。

- ⑤生成AST后，会被Ignition转换成字节码，然后转成机器码，最后就是代码的执行过程了；

  ![image-20240924115901693](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924115901693.png)

**总结：**

V8 引擎通过现代的解释与即时编译技术，结合垃圾回收与运行时优化，使得 JavaScript 的执行效率得到了大幅提升。它的工作原理涉及字节码解释、机器码生成、内存管理、类型反馈等机制，并通过不断的监控与优化，保证了代码的高效执行。





#### JavaScript的执行过程

> 编写一段JavaScript代码，它是如何执行的呢？简单来说，JS引擎在执行JavaScript代码的过程中需要先解析再执行。那么在解析阶段JS引擎又会进行哪些操作，接下来就一起来了解一下JavaScript在执行过程中的详细过程，包括执行上下文、GO、AO、VO和VE等概念的理解。



##### 1.初始化全局对象

> 首先，JS引擎会在执行代码之前，也就是解析代码时，会在我们的堆内存创建一个全局对象：Global Object（简称GO），观察以下代码，在全局中定义了几个变量：

示例代码：

```js
var name = 'curry'
var message = 'I am a coder'
var num = 30
```

JS引擎内部在解析以上代码时，会创建一个全局对象（伪代码如下）：

- 所有的**作用域（scope）**都可以访问该全局对象；
- 对象里面会包含一些**全局的方法和类**，像Math、Date、String、Array、setTimeout等等；
- 其中有一个**window属性**是指向该全局对象自身的；
- 该对象中会收集我们上面全局定义的变量，并设置成undefined；
- 全局对象是非常重要的，我们平时之所以能够使用这些全局方法和类，都是在这个全局对象中获取的；



```js
var GlobalObject = {
  Math: '类',
  Date: '类',
  String: '类',
  setTimeout: '函数',
  setInterval: '函数',
  window: GlobalObject,
  ...
  name: undefined,
  message: undefined,
  num: undefined
}
```

##### 2.执行上下文栈（调用栈）

> 了解了什么是全局对象后，下面就来聊聊代码具体执行的地方。JS引擎为了执行代码，引擎内部会有一个**执行上下文栈（Execution Context Stack，简称ECS）**，它是用来执行代码的**调用栈**。

**（1）ECS如何执行？先执行谁呢？**

- 无疑是先执行我们的全局代码块；
- 在执行全局代码前会构建一个**全局执行上下文（Global Execution Context，简称GEC）**；
- 一开始GEC就会被放入到ECS中执行；

**（2）那么全局执行上下文（GEC）包含那些内容呢？**

- 第一部分：

  执行代码前。

  - 在转成抽象语法树之前，会将**全局定义的变量、函数**等加入到**Global Object**中，也就是上面初始化全局对象的过程；
  - 但是**并不会真正赋值**（表现为undefined），所以这个过程也称之为**变量的作用域提升（hoisting）**；

- 第二部分：

  代码执行。

  - 对变量进行赋值，或者执行其它函数等；

下面就通过一幅图，来看看GEC被放入ECS后的表现形式：

![image-20240924150438917](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924150438917.png)



##### 3.调用栈调用GEC的过程

> 接下来，将全局代码复杂化一点，再来看看调用栈调用全局执行上下文（GEC）的过程。

示例代码：

```js
var name = 'curry'

console.log(message)

var message = 'I am a coder'

function foo() {
  var name = 'foo'
  console.log(name)
}

var num1 = 30
var num2 = 20

var result = num1 + num2

foo()
```

调用栈调用过程：

- 1.初始化全局对象。

  - 这里需要注意的是函数存放的是地址，会指向函数对象，与普通变量有所不同；

  - 从上往下解析JS代码，当解析到foo函数时，因为foo不是普通变量，并不会赋为undefined，JS引擎会在堆内存中开辟一块空间存放foo函数，在全局对象中引用其地址；

  - 这个开辟的函数存储空间最主要存放了该函数的**父级作用域**和函数的**执行体代码块**；

    ![image-20240924150918154](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924150918154.png)

- 2.构建一个全局执行上下文（GEC），代码执行前将VO的内存地址指向GlobalObject（GO）。

  ![image-20240924151031988](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924151031988.png)

- 3.将全局执行上下文（GEC）放入执行上下文栈（ECS）中。

  ![image-20240924151124904](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924151124904.png)

- 4.从上往下开始执行全局代码，依次对GO对象中的全局变量进行赋值。

  - 当执行`var name = 'curry'`时，就从VO（对应的就是GO）中找到name属性赋值为curry；

  - 接下来执行`console.log(message)`，就从VO中找到message，注意**此时的message还为undefined**，因为message真正赋值在下一行代码，所以就直接打印undefined（也就是我们经常说的变量作用域提升）；

  - 后面就依次进行赋值，执行到`var result = num1 + num2`，也是从VO中找到num1和num2两个属性的值进行相加，然后赋值给result，result最终就为50；

  - 最后执行到`foo()`，也就是需要去执行foo函数了，这里的操作是比较特殊的，涉及到**函数执行上下文**，下面来详细了解；

    ![image-20240924151744655](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924151744655.png)



##### 4.函数执行上下文

> 在执行全局代码遇到函数如何执行呢？

- 在执行的过程中遇到函数，就会根据函数体创建一个**函数执行上下文（Functional Execution Context，简称FEC）**，并且加入到执行上下文栈（ECS）中。
- 函数执行上下文（FEC）包含三部分内容：
  - AO：在解析函数时，会创建一个**Activation Objec（AO）**；
  - 作用域链：由**函数VO和父级VO组成**，查找是一层层往外层查找；
  - this指向：this绑定的值，在函数执行时确定；
- 其实全局执行上下文（GEC）也有自己的作用域链和this指向，只是它对应的作用域链就是自己本身，而this指向为window。

继续来看上面的代码执行，当执行到`foo()`时：

- 先找到foo函数的存储地址，然后**解析foo函数**，生成函数的AO；
- 根据AO生成函数执行上下文（FEC），并将其放入执行上下文栈（ECS）中；
- 开始执行foo函数内代码，依次找到AO中的属性并赋值，当执行`console.log(name)`时，就会去foo的VO（对应的就是foo函数的AO）中找到name属性值并打印；

![image-20240924151857646](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924151857646.png)

##### 5.变量环境和记录

> 上文中提到了很多次VO，那么VO到底是什么呢？下面从ECMA新旧版本规范中来谈谈VO。

在早期ECMA的版本规范中：每一个执行上下文会被关联到一个**变量环境（Variable Object，简称VO）**，在源代码中的**变量和函数声明**会被作为属性添加到VO中。对应函数来说，参数也会被添加到VO中。

- 也就是上面所创建的GO或者AO都会被关联到变量环境（VO）上，可以通过VO查找到需要的属性；
- 规定了VO为Object类型，上文所提到的GO和AO都是Object类型；

在最新ECMA的版本规范中：每一个执行上下文会关联到一个**变量环境（Variable Environment，简称VE）**，在执行代码中**变量和函数的声明**会作为**环境记录（Environment Record）**添加到变量环境中。对于函数来说，参数也会被作为环境记录添加到变量环境中。

- 也就是相比于早期的版本规范，对于变量环境，已经去除了VO这个概念，提出了一个新的概念VE；
- 没有规定VE必须为Object，不同的JS引擎可以使用不同的类型，作为一条环境记录添加进去即可；
- 虽然新版本规范将变量环境改成了VE，但是JavaScript的执行过程还是不变的，只是关联的变量环境不同，将VE看成VO即可；



##### 6.全局代码执行过程（函数嵌套）

> 了解了上面相关的概念和调用流程之后，就来看一下存在函数嵌套调用的代码是如何执行的，以及执行过程中的一些细节，以下面代码为例：

```js
var message = 'global'

function foo(m) {
  var message = 'foo'
  console.log(m)

  function bar() {
    console.log(message)
  }

  bar()
}

foo(30)
```

- 初始化全局对象（GO），执行全局代码前创建GEC，并将GO关联到VO，然后将GEC加入ECS中：

  - foo函数存储空间中指定的父级作用域为全局对象；

    ![image-20240924152054916](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924152054916.png)

- 开始执行全局代码，从上往下依次给全局属性赋值：

  - 给message属性赋值为global；

    ![image-20240924152200279](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924152200279.png)

- 执行到foo函数调用，准备执行foo函数前，创建foo函数的AO：

  - bar函数存储空间中指定父级作用域为foo函数的AO；

    ![image-20240924152304455](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924152304455.png)

- 创建foo函数的FEC，并加入到ECS中，然后开始执行foo函数体内的代码：

  - 根据foo函数调用的传参，给形参m赋值为30，接着给message属性赋值为foo；

  - 所以，m打印结果为30；

    ![image-20240924152346504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924152346504.png)

- 执行到bar函数调用，准备执行bar函数前，创建bar函数的AO：

  - bar函数中没有定义属性和声明函数，以空对象表示；

    ![image-20240924152723115](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924152723115.png)

- 创建bar函数的FEC，并加入到ECS中，然后开始执行bar函数体内的代码：

  - 执行`console.log(message)`，会先去bar函数自己的VO中找message，没有找到就往上层作用域的VO中找；

  - 这里bar函数的父级作用域为foo函数，所以找到foo函数VO中的message为foo，**打印结果为foo**；

    ![image-20240924153120011](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924153120011.png)

- 全局中所有代码执行完成，bar函数执行上下文出栈，bar函数AO对象失去了引用，进行销毁。

- 接着foo函数执行上下文出栈，foo函数AO对象失去了引用，进行销毁，同样，foo函数AO对象销毁后，bar函数的存储空间也失去引用，进行销毁。



**总结**

- 函数在执行前就已经确定了其父级作用域，与函数在哪执行没有关系，以函数声明的位置为主；
- 执行代码查找变量属性时，会沿着**作用域链**一层层往上查找（沿着VO往上找），如果一直找到全局对象中还没有该变量属性，就会报错未定义；
- 上文中提到了很多概念名词，下面来总结一下：

| 名词 | 解释                                                         |
| :--- | :----------------------------------------------------------- |
| ECS  | 执行上下文栈（Execution Context Stack），也可称为调用栈，以栈的形式调用创建的执行上下文 |
| GEC  | 全局执行上下文（Global Execution Context），在执行全局代码前创建 |
| FEC  | 函数执行上下文（Functional Execution Context），在执行函数前创建 |
| VO   | Variable Object，早期ECMA规范中的变量环境，对应Object        |
| VE   | Variable Environment，最新ECMA规范中的变量环境，对应环境记录 |
| GO   | 全局对象（Global Object），解析全局代码时创建，GEC中关联的VO就是GO |
| AO   | 函数对象（Activation Object），解析函数体代码时创建，FEC中关联的VO就是AO |





##### 7. JavaScript内存管理

**1.什么是内存管理？**

> 在了解JavaScript的内存管理之前，可以先大致熟悉一下什么是内存管理，不管什么样的编程语言，在其代码执行的过程中都是需要为其分配内存的。

不管什么样的编程语言，以及它用什么方式来管理内存，其内存的管理都具备以下的**生命周期**：

- 申请内存：分配其需要的内存。
- 使用内存：使用分配的内存。
- 释放内存：使用完毕后，对其进行释放。

但是不同的编程语言对内存的**申请和释放**会有不同的实现，主要分为**手动和自动管理内存**：

- 手动管理内存：像C、C++等一些接近底层的编程语言，都是需要手动来申请和释放内存（malloc函数用于申请内存、free函数用于释放内存）。
- 自动管理内存：像Java、JavaScript、Python等一些高级编程语言，都是自动帮助我们管理内存的。

**2.JavaScript的内存分配**

> 通过上面对内存管理的简单介绍可以知道，JavaScript是自动管理内存的，所以在我们编写JS代码定义变量时就会为其分配内存。

根据JavaScript不同的数据类型，会对其分配到不同的内存空间中，数据类型主要分为**基本数据类型**和**复杂数据类型**：

- 对于基本数据类型的内存分配会在执行时，直接在栈空间中进行分配。
  - 基本数据类型（也称值类型）：string、number、boolean、undefined、null、symbol；
- 对于复杂数据类型的内存分配会在堆内存中开辟一块空间，变量引用其内存地址。
  - 复杂数据类型（也称引用类型）：object、function、array；

以下代码在内存结构中的表现形式如下：

```js
const name = 'curry'
const age = 30
const info = {
  name: 'kobe',
  age: 24
}
```

![image-20240924154724340](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924154724340.png)

**3.JavaScript的垃圾回收机制**

> 在管理内存的生命周期中是包括内存的释放，因为我们的内存大小是有限的，所以当代码执行完毕，不再需要内存的时候，那么就需要对其进行内存释放，以便腾出更多的内存空间给其它的应用程序使用。
>
> 垃圾回收的核心思想是识别和释放不再被引用的对象。JavaScript 引擎会定期检查内存，确定哪些对象是“可达的”，哪些是“不可达的”。可达的对象是指在程序运行过程中仍然可以访问到的对象。

- 而在手动管理内存的编程语言中，需要自己通过一些方式来释放不再需要的内存，这样就需要编写专门用于管理内存的代码，不仅影响编写代码的效率，管理不当也有可能产生**内存泄露**。
- 所以大部分现代的编程语言都是有自己的垃圾回收机制的，那么什么是垃圾回收机制？
  - **垃圾回收（Garbage Collection，简称GC）**，就是对于那些不再使用的数据，都可以称之为垃圾，需要通过回收来释放内存空间；
  - 在JavaScript的运行环境JS引擎中就存在垃圾回收的功能模块，这个功能模块就称为**垃圾回收器**；
- 那么这里就可以提出一个疑问，GC是如何找到不再使用的数据，并对其进行内存回收呢？
  - 这里就用到了GC算法，下面介绍**两种常见的GC算法**；

**4.两种常见的GC算法**

4.1.引用计数

> 什么是引用计数？
>
> 当一个对象有一个引用指向它时，那么这个对象的引用就加1，并且将其引用次数保存起来，而当一个对象的引用为0时，那么这个对象就可以被销毁了（回收）。

示例代码：

- person1的引用次数为3；
- person2和person3的引用次为1；

```js
let person1 = { name: 'curry' }

let person2 = {
  name: 'kobe',
  friend: person1
}

let person3 = {
  name: 'klay',
  friend: person1
}
```

内存表现：

![image-20240924155426511](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924155426511.png)

如果接着执行`person3 = null`，那么person3的引用指向次数就会减1，变为0，从而销毁。而person3销毁后person1也会失去person3的指向，引用指向次数也会减1，变为2。

**缺点**：但是引用计数这个GC算法，存在一个很大的弊端，就是当出现**循环引用**时，就无法进行正确的回收，导致内存泄露，如下示例代码：

- curry的好朋友是klay，巧合的是klay的好朋友是curry，这样就出现了对象的循环引用；

```js
let person1 = {
  name: 'curry',
  friend: person2
}

let person2 = {
  name: 'klay',
  friend: person1
}
```

内存表现：

- 即使执行`person1 = null; person2 = null`，person1和person2对象的引用次数依然为1；

- 所以引用计数就无法很好的处理这种情况了；

  ![image-20240924155705967](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924155705967.png)

4.2.标记清除

> 什么是标记清除？
>
> 这个算法设置了一个**根对象（root object如全局对象、当前活动函数的变量等）**，GC会**定期**从这个根对象开始往下查找有引用到的对象，而对于那些没有引用到的对象，也就是没有查找到的对象，就认为是需要进行回收的对象。

标记清除的一大优势就是可以很好的解决循环引用的问题，如下图：

- 标记清除算法首先会从root object往下开始查找引用到的对象；

- 而对于object6和object7进行了循环引用了的对象，是查找不到的，就会被视为回收对象，从而被GC回收；

- 目前的JS引擎的GC核心采用的比较多的算法就是标记清除，类似于V8引擎不单单只是用了标记清除，同时也结合了一些其它的算法来应对更多的情况；

  ![image-20240924155823963](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924155823963.png)

4.3 分代垃圾回收

为了提高效率，现代 JavaScript 引擎（如 V8）采用分代垃圾回收策略。内存中的对象根据其生命周期被分为两类：

- 新生代（New Space）

  - 存储新创建的对象，生命周期通常较短。

  - 垃圾回收器频繁检查新生代，通常使用复制算法来回收内存。

- 老生代（Old Space）

  - 存储经过多次垃圾回收仍然存活的对象，生命周期较长。

  - 垃圾回收器较少检查老生代，使用标记-清除算法进行回收。



4.4 标记整理（Mark-Compact） 和“标记－清除”相似；

- 不同的是，回收期间同时会将保留的存储对象搬运汇集到连续的内存空间，从而整合空闲空间，避免内存碎片化；



4.5 增量收集（Incremental collection）

- 如果有许多对象，并且我们**试图一次遍历并标记整个对象集，则可能需要一些时间，并在执行过程中带来明显的延迟。**

- 所以引擎试图将垃圾收集工作分成几部分来做，然后将这几部分会逐一进行处理，这样会有许多微小的延迟而不是一个大的

  延迟



4.6 闲时收集（Idle-time collection）

- 垃圾收集器**只会在 CPU 空闲时尝试运行，以减少可能对代码执行**的影响。



4.7 V8引擎详细的内存图

![image-20240924161552688](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924161552688.png)





##### 8. 闭包

**1.函数在JavaScript中的地位**

> 在介绍闭包之前，可以先聊聊函数在JavaScript中的地位，因为闭包的存在是与函数息息相关的。

- JavaScript之所以可以称之为支持**头等函数**的编程语言，是因为JavaScript中函数是**一等公民**；
- 函数不仅在JavaScript中扮演着重要的角色，而且可以使用的非常灵活；
- 函数不仅可以作为另一个函数的**参数**，也可以作为另一个函数的**返回值**；
- 这样使用的函数也称之为**高阶函数**，像JS的数组中就实现了许多高阶函数（map、filter、reduce等）；

**2.JavaScript中闭包的定义**

> 闭包的概念出现于60年代，最早实现闭包的程序是Scheme，那么就可以理解为什么JavaScript中有闭包了，因为JavaScript中大量的设计是源自于Scheme的。而在不同的地方对JavaScript闭包的定义是不一样的，但是整体核心还是一致的，只是用不同的话来描述JavaScript闭包，以下是摘抄自三个地方的定义。

维基百科中对闭包的定义：

- 闭包（Closure），又称**词法闭包**（Lexical Closure）或**函数闭包**（function closures），是在支持**头等函数**的编程语言中，实现词法绑定的一种技术；
- 闭包在实现上是一个**结构体**，它存储了**一个函数**和**一个关联的环境**（相当于一个符号查找表）；
- 闭包跟函数最大的区别在于，当捕捉闭包的时候，它的**自由变量**会在捕捉时被确定，这样即使脱离了捕捉时的上下文，它也能照常运行；

MDN中对闭包定义：

- 一个**函数**和对其**周围状态**（lexical environment，词法环境）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是闭包（closure）；
- 也就是说，闭包让你可以**在一个内层函数中访问到其外层函数的作用域**；
- 在JavaScript中，每当创建一个函数，闭包就会在函数创建的同时被创建出来；

《JavaScript高级程序设计》中对闭包的定义：

- 闭包指的是那些引用了**另一个函数作用域中变量**的函数，通常是在**嵌套函数**中实现的；

对闭包定义的总结：

- 以上对闭包的三种定义，都提到了函数、环境、作用域和变量，总结为就是：一个**函数**，如果它可以访问**外层作用域**的**自由变量**，那么这个函数就是一个闭包；
- 闭包由两部分组成：**内层函数+可以访问的外层自由变量**；
- 广义角度：JavaScript中的函数都是闭包（都可以形成闭包）；
- 狭义角度：JavaScript中的一个函数，如果访问了外层作用域的变量，那么它是一个闭包；

**3.闭包是如何形成的？**

> 看了一大堆闭包的定义，那么到底什么情况下就形成了闭包呢？

（1）**产生闭包的条件**：简单来说，满足以下几个条件就可以说产生了闭包。

- 函数嵌套；
- 内层函数引用了外层函数作用域中的变量；
- 外层函数执行；

（2）**常见的闭包。**

- 将一个函数作为另一个函数返回值，例如：

  ```js
  function foo() {
    var name = 'foo'
  
    return function bar() {
      console.log(name)
    }
  }
  
  var fn = foo()
  fn()
  ```

- 将一个函数作为实参传递另一个函数，例如：

  ```js
  function showDelay(msg) {
    setTimeout(function() {
      console.log(msg)
    })
  }
  
  showDelay('我形成了闭包')
  ```

**4.闭包的访问和执行过程**

> 下面介绍闭包在访问和执行过程中的内存表现，进一步深入对闭包的了解，以如下代码为例：

示例代码：

```js
function foo() {
  var name = 'foo'

  return function bar() {
    console.log(name)
  }
}

var fn = foo()
fn()
```

- 首先，在执行全局代码之前，会在内存中创建一个全局对象（GO），将全局执行上下文压入栈中，这时的fn还未被赋值；

  ![image-20240924175301985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924175301985.png)

- 当执行到`var fn = foo()`时，在调用foo之前创建foo的活动对象（AO），创建foo函数执行上下文，并将其压入栈中，接着执行foo函数，执行完成后fn指向bar函数内存地址；

  ![image-20240924175519985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924175519985.png)

- foo函数执行完成后，foo函数执行上下文会弹出栈，而按道理foo的活动对象（AO）是需要被销毁的，那到底有没有销毁，我们接着看；

- 接着执行`fn()`，因为fn是指向bar函数的，执行之前会先创建bar的活动对象（AO），然后执行`console.log(name)`，而name会先去自己的AO中查找，发现没有找到就会去到上层作用域（父级作用域）中查找，最终找到`foo`并打印，这里bar函数的上层作用域就是foo函数的作用域对应foo的活动对象（AO）；

  ![image-20240924175454920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924175454920.png)

- bar函数执行完成后，bar函数的执行上下文弹出栈，对应bar的活跃对象（AO）被销毁，而foo的活跃对象（AO）还一直存留在内存中；

  ![image-20240924175558835](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924175558835.png)

- 但是bar函数的父级作用域是在什么时候确定的呢？

  - 在编译bar函数时就已经确定了bar函数的父级作用域——foo的活动对象（AO）早在编译时就加入到了bar函数的作用域链中；
  - 为什么执行完foo函数后还可以访问其name变量，就可以回答上面的问题了，foo的活动对象（AO）是没有被销毁的；
  - 因为bar函数的作用域链中依然对foo的活动对象（AO）有引用，导致其不能正常销毁；

根据上面闭包的访问和执行过程结合闭包的定义做一个总结：

- 在维基百科中定义的“闭包在实现上是一个结构体，它存储了一个函数和一个关联的环境（相当于一个符号查找表）”，以及MDN中定义的“一个函数和对其周围状态（lexical environment，词法环境）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是闭包（closure）”，其函数和关联的环境、函数和对其周围状态的引用，对应的就是上面的bar函数和上层作用域中的name；

  ![image-20240924175636910](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924175636910.png)

- 而对于维基百科中提到的“闭包跟函数最大的区别在于，当捕捉闭包的时候，它的**自由变量**会在捕捉时被确定，这样即使脱离了捕捉时的上下文，它也能照常运行”，也就是在捕捉bar函数时，同时捕捉到对name这个自由变量的引用，执行完foo函数后，将bar函数赋值给fn，最后执行fn时，也是能正常访问到name的；

- 了解了其访问执行过程后，可以发现本应该被销毁的foo的活跃对象（AO），在代码执行完后最终没能被销毁，而这样的情况称之为**内存泄露**，下面就来谈谈闭包的内存泄露；

- 如果对上面的执行过程不清楚，可以先看看这篇文章：[JavaScript的执行过程（深入执行上下文、GO、AO、VO和VE等概念）](https://www.cnblogs.com/MomentYY/p/15785719.html)

**5.闭包的内存泄露**

> 闭包会保留它们包含函数的作用域，所以比其它函数更占用内存，而过渡使用闭包可能导致内存过度占用，也就是内存泄露，而除了内存泄露这个概念还有一个内存溢出的概念，下面就先了解一下这两者的区别和关系：

- 内存溢出：程序运行出现错误，当程序运行需要的内存超过了剩余的内存时，就会抛出内存溢出的错误（比如，死循环）。
- 内存泄露：占用的内存没有及时释放，内存泄露积累过多就容易导致内存溢出（比如，意外的全局变量、没有及时清理计时器或回调、**闭包**）

那具体怎么解决闭包产生的内存泄露呢？

- 针对于上面的代码，可以在最后执行`fn = null`；
- 因为将fn设置为null时，就不再对bar函数有引用，bar函数失去了全部的引用就会被销毁，对应foo的活动对象（AO）也就失去了引用，在下一次的垃圾回收（GC）检测中，就会被销毁掉；

**6.使用浏览器查看闭包**

> 闭包其实是可以在浏览器中观察到的，在查看闭包之前先来讨论一个问题，外层函数的活跃对象（AO）不会被销毁，是不是里面所有的属性都不会被销毁呢?

如果将上面的代码改成下面这样，多增加两个变量age和message，但是bar函数中并没有对age和message有引用：



```js
function foo() {
  var name = 'foo'
  var age = 18
  var message = 'hello bibao'

  return function bar() {
    console.log(name)
  }
}

var fn = foo()
fn()
```

- 形成闭包后，name是一定不会被销毁的，这个上面已经验证过了；

- 具体age和message有没有被销毁，可以在代码中打上断点，在Chrome浏览器查看对应的闭包；

  ![image-20240924175733589](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240924175733589.png)

- 观察上面的结果是没有age和message属性的，这个就涉及到JS引擎的实现了，像V8引擎就对其进行了优化，对于闭包内层函数没有使用到的自由变量，是不会被保存的，这样就大大提升了内存的使用率；