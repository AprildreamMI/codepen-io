# 常用DOM操作

## 获取节点

![获取节点](https://zh.javascript.info/article/dom-navigation/dom-links.svg)

## 获取元素节点

![获取元素节点](https://zh.javascript.info/article/dom-navigation/dom-links-elements.svg)

## 创建节点

  - `document.createElement(tag)` — 用给定的标签创建一个元素节点，
  - `document.createTextNode(value)` — 创建一个文本节点（很少使用），
  - `elem.cloneNode(deep)` — 克隆元素，如果 `deep==true` 则与其后代一起克隆。

## 插入节点

> 插入节点或者文本字符串，不是`html`

```javascript
<div id="div"></div>
<script>
    // 可以有多个node参数
  div.before('<p>Hello</p>', document.createElement('hr'));
</script>

// 最终结果
&lt;p&gt;Hello&lt;/p&gt;
<hr>
<div id="div"></div>
```

![](https://zh.javascript.info/article/modifying-document/before-prepend-append-after.svg)

  - `node.append(...nodes or strings)` — 在 `node` 末尾插入，
  - `node.prepend(...nodes or strings)` — 在 `node` 开头插入，
  - `node.before(...nodes or strings)` — 在 `node` 之前插入，
  - `node.after(...nodes or strings)` — 在 `node` 之后插入，
  - `node.replaceWith(...nodes or strings)` — 替换 `node`。
  - `node.remove()` — 移除 `node`。

请注意：如果我们要将一个元素 **移动** 到另一个地方，则无需将其从原来的位置中删除。

**所有插入方法都会自动从旧位置删除该节点。**

```javascript
<div id="first">First</div>
<div id="second">Second</div>
<script>
  // 无需调用 remove
  second.after(first); // 获取 #second，并在其后面插入 #first
</script>

// 结果
Second
First
```



## 插入HTML

![](https://zh.javascript.info/article/modifying-document/insert-adjacent.svg)

为此，我们可以使用另一个非常通用的方法：`elem.insertAdjacentHTML(where, html)`。

该方法的第一个参数是代码字（code word），指定相对于 `elem` 的插入位置。必须为以下之一：

- `"beforebegin"` — 将 `html` 插入到 `elem` 前插入，
- `"afterbegin"` — 将 `html` 插入到 `elem` 开头，
- `"beforeend"` — 将 `html` 插入到 `elem` 末尾，
- `"afterend"` — 将 `html` 插入到 `elem` 后。

## document滚动

```javascript
document.documentElement.scrollTop
document.documentElement.scrollHeight
```

## 宽高

+ 获取`element`可视化宽度/宽度

  `document.documentElement.clientWidth`

  ![](https://developer.mozilla.org/@api/deki/files/185/=Dimensions-client.png)

+ 获取`element`元素宽高

  `HTMLElement.offsetWidth`

  ![获取元素宽高](https://developer.mozilla.org/@api/deki/files/186/=Dimensions-offset.png)

+ 获取浏览器的宽高

  + `window.innerHeight`和`window.outerHeight `

    ![innerHeight vs outerHeight illustration](https://developer.mozilla.org/@api/deki/files/213/=FirefoxInnerVsOuterHeight2.png)

  + `window.innerWidth`和`window.outerWidth`参照上面的横向

## 定位的左右

+ 获取定位`element`元素左右

  `HTMLElement.offsetLeft`

  > **`HTMLElement.offsetLeft`** 是一个只读属性，返回当前元素*左上角*相对于  [`HTMLElement.offsetParent`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) 节点的左边界偏移的像素值。
  >
  > 对块级元素来说，`offsetTop`、`offsetLeft`、`offsetWidth` 及 `offsetHeight` 描述了元素相对于 `offsetParent` 的边界框。

## `classList`对类名的操作

```javascript
const div = document.createElement('div');
div.className = 'foo';

// 初始状态：<div class="foo"></div>
console.log(div.outerHTML);

// 使用 classList API 移除、添加类值
div.classList.remove("foo");
div.classList.add("anotherclass");

// <div class="anotherclass"></div>
console.log(div.outerHTML);

// 如果 visible 类值已存在，则移除它，否则添加它
div.classList.toggle("visible");

// add/remove visible, depending on test conditional, i less than 10
div.classList.toggle("visible", i < 10 );

console.log(div.classList.contains("foo"));

// 添加或移除多个类值
div.classList.add("foo", "bar", "baz");
div.classList.remove("foo", "bar", "baz");

// 使用展开语法添加或移除多个类值
const cls = ["foo", "bar"];
div.classList.add(...cls);
div.classList.remove(...cls);

// 将类值 "foo" 替换成 "bar"
div.classList.replace("foo", "bar");
```

## 可见阈值

```javascript
h2 {
      font-size: 72px;
      font-family: Helvetica;
      /* letter-spacing: -0.12em; */
      width: 290px;
      font-weight: normal;
      /* position: relative; */
      left: 50%;
      position: sticky;
      top: -1px;
      margin: 100px 0;
      padding: 0;
    }
  </style>
</head>
<body>
    <h2 style="--scale: 1">Only 11.5mm. Now that's thin.</h2>
  <script>
    let isPinned = false
    const h2 = document.querySelector('h2')
    const observer = new IntersectionObserver(([e]) => {
      // e.target.classList
    }, {
      /* 规定了一个监听目标与边界盒交叉区域的比例值，
      可以是一个具体的数值或是一组0.0到1.0之间的数组。
      若指定值为0.0，则意味着监听元素即使与根有1像素交叉，
      此元素也会被视为可见. 若指定值为1.0，则意味着整个元素都是可见的 */
      // 此处设为1，就是只要稍微有一点不可见，就会触发，因为回调函数的触发条件为
      // 元素的可见比超过/或低于指定阈值
      // 此处不可见是被滚动了所以上面的top:-1，就是为了可以让h2被滚动掉1px
      // 而1px的不可见，不可见比小于1,就触发了回调
      threshold: 1
    })
      </script>
</body>
```

## 获取样式`getComputedStyle`

> https://www.runoob.com/w3cnote/window-getcomputedstyle-method.html

**getComputedStyle** 和 **element.style** 的相同点就是二者返回的都是 CSSStyleDeclaration 对象，取相应属性值得时候都是采用的 CSS 驼峰式写法，均需要注意 float 属性。

**而不同点就是：**

- `element.style` 读取的只是元素的**内联样式**，即写在元素的 style 属性上的样式；而 `getComputedStyle` 读取的样式是最终样式，包括了**内联样式**、**嵌入样式**和**外部样式**。
- `element.style` 既支持读也支持写，我们通过 `element.style` 即可改写元素的样式。而 `getComputedStyle` 仅支持读并不支持写入。我们可以通过使用 `getComputedStyle` 读取样式，通过 `element.style` 修改样式

**我们可以通过使用 \**getComputedStyle\** 读取样式，通过 \**element.style\** 修改样式。**

+ `getComputedStyle(h2).getPropertyValue('margin-top')`
+ `h2.style.setProperty('--scale', (1 - scrolled))`

```javascript
let marginTop = parseInt(getComputedStyle(h2).getPropertyValue('margin-top'))

        // 已经滚到顶了，然后拿到已经滚的距离，先把上面的marginTop减去，再跟height算滚动比例
        let scrolled = (html.scrollTo - marginTop) / height

        h2.style.setProperty('--scale', )
```

## 特性操作

>  操作特性，HTML标签上的属性`attribute`
>
> HTML 特性有以下几个特征：
>
> - 它们的名字是大小写不敏感的（`id` 与 `ID` 相同）。
> - 它们的值总是字符串类型的。

\+ `elem.hasAttribute(name)` — 检查特性是否存在。

\+ `elem.getAttribute(name)` — 获取这个特性值。

\+ `elem.setAttribute(name, value)` — 设置这个特性值。

\+ `elem.removeAttribute(name)` — 移除这个特性。

\+ `elem.attributes` — 读取所有特性：属于内建 Attr 类的对象的集合，具有 name 和 value 属性。

### 非标准的特性-`dataset`

所有以 “data-” 开头的特性均被保留供程序员使用。它们可在 dataset 属性中使用。

```javascript
 // 读取

 alert(order.dataset.orderState); // new

 // 修改

 order.dataset.orderState = "pending"; // (*)

```

