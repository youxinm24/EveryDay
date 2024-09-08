# Js选择器
JS选择器常用的有`getElementById()`、`getElementsByClassName()`、`getElementsByName()`、`getElementsByTagName()`、`querySelector()`、`querySelectorAll()`。

## getElementById
通过`id`来定位，返回对指定`id`的第一个对象的引用，返回类型为`HTMLDivElement`。

```html
<div id="t1">T1</div>

<script type="text/javascript">
    var t1 = document.getElementById("t1");
    console.log(t1); // <div id="t1">D1</div>
    console.log(Object.prototype.toString.call(t1)); // [object HTMLDivElement]
</script>
```

## getElementsByClassName

通过`class`属性来定位，返回文档中指定`class`属性值的元素的引用，返回类型为`HTMLCollection`。

```html
<div class="t2">D2</div>
<div class="t2">D3</div>

<script type="text/javascript">
    var t2List = document.getElementsByClassName("t2");
    console.log(t2List); // HTMLCollection(2) [div.t2, div.t2]
    // 使用for循环遍历
    for(let i=0,n=t2List.length;i<n;++i) console.log(t2List[i]);
    // HTMLCollection的prototype中没有forEach方法，遍历需要使用Array的prototype中forEach通过call绑定对象实例并传参
    Array.prototype.forEach.call(t2List,v => console.log(v) ); 
    // HTMLCollection的prototype中没有map方法，也需要使用Array的prototype中forEach通过call绑定对象实例并传参
    Array.prototype.map.call(t2List,v => console.log(v) ); 
</script>
```

### **遍历 `HTMLCollection`**

虽然 `HTMLCollection` 类似数组，但它并不是真正的数组，因此不能直接使用数组的原生方法（如 `forEach()`、`map()` 等）。可以通过以下几种方式遍历 `HTMLCollection`：

#### 方法 1：使用 `for` 循环

```javascript
let divs = document.getElementsByTagName('div');
for (let i = 0; i < divs.length; i++) {
    console.log(divs[i]);
}
```

#### 方法 2：使用 `for...of`

`HTMLCollection` 是可迭代对象，可以使用 `for...of` 遍历。

```javascript
let divs = document.getElementsByTagName('div');
for (let div of divs) {
    console.log(div);
}
```

#### 方法 3：将 `HTMLCollection` 转换为数组

通过 `Array.from()` 将 `HTMLCollection` 转换为数组，然后使用数组的原生方法遍历。

```javascript
let divs = document.getElementsByTagName('div');
Array.from(divs).forEach(div => {
    console.log(div);
});
```

#### 方法 4：使用 `call` 和 `forEach`

使用 `Array.prototype.forEach.call` 来临时把 `HTMLCollection` 作为数组处理。

```javascript
let divs = document.getElementsByTagName('div');
Array.prototype.forEach.call(divs, div => {
    console.log(div);
});
```

通过这些方法，`HTMLCollection` 可以像数组一样进行遍历和操作。

## getElementsByName

通过`name`属性来定位，返回文档中指定`name`属性值的元素的引用，返回类型为`NodeList`。

```html
<div name="t3">D4</div>
<div name="t3">D5</div>

<script type="text/javascript">
    var t3List = document.getElementsByName("t3");
    console.log(t3List); // NodeList(2) [div, div]
    // 可直接使用forEach进行遍历
    t3List.forEach( v => console.log(v) ); 
    // NodeList的prototype中没有map方法，使用map的场景也需要借助Array的prototype中map通过call绑定对象实例并传参
    Array.prototype.map.call(t3List,v => console.log(v) ); 
</script>
```

## getElementsByTagName

通过标签的名字来定位，返回文档中指定标签的元素的引用，返回类型为`HTMLCollection`。

```html
<p class="t4">P6</p>
<p class="t4">P7</p>

<script type="text/javascript">
    var t4List = document.getElementsByTagName("p");
    console.log(t4List); // HTMLCollection(2) [p, p]
    Array.prototype.forEach.call(t4List, function(v){console.log(v);}); 
    Array.prototype.map.call(t4List,function(v){console.log(v);} ); 
</script>
```

## querySelector
通过`CSS`选择器来定位，返回文档中匹配指定`CSS`选择器的第一个元素的引用，返回类型为`HTMLDivElement`。

```html
<div>
    <div class="t5">D8</div>
</div>

<script type="text/javascript">
    var t5 = document.querySelector("div .t5");
    console.log(t5); // <div class="t5">D8</div>
    console.log(Object.prototype.toString.call(t5)); // [object HTMLDivElement]
</script>
```

## querySelectorAll
通过`CSS`选择器来定位，返回文档中匹配指定`CSS`选择器的所有元素的引用，返回类型为`NodeList`。

```html
<div>
    <div id="t6">D9</div>
    <div>D10</div>
    <div>D11</div>
</div>

<script type="text/javascript">
     var t6List = document.querySelectorAll("#t6 ~ div");
    console.log(t6List); // NodeList(2)[div, div]
    t6List.forEach(function(v){console.log(v);}); 
    Array.prototype.map.call(t6List,function(v){console.log(v);} ); 
</script>
```

在 JavaScript 中，`HTMLDivElement`、`HTMLCollection` 和 `NodeList` 都是与 DOM（文档对象模型）相关的类型，它们帮助开发者操作网页上的元素。以下是它们的详细解释：

### 1. **其他常见的 DOM 对象**

除了 `HTMLDivElement`、`HTMLCollection` 和 `NodeList`，DOM 中还有很多其他类型的对象，主要包括不同类型的元素节点和集合。以下是一些常见的 DOM 对象：

- **`HTMLElement`**: 表示所有 HTML 元素的基类对象，所有具体的元素（如 `<div>`, `<p>`, `<span>`）都继承自 `HTMLElement`。
  
- **`HTMLInputElement`**: 代表 `<input>` 元素的对象，可以用来操作表单中的输入元素。
  
- **`HTMLAnchorElement`**: 代表 `<a>` （超链接）元素的对象，用来操作超链接相关的属性，如 `href`、`target` 等。

- **`HTMLImageElement`**: 代表 `<img>` 元素的对象，用来处理图像的加载和展示。

- **`HTMLTableElement`**: 代表 `<table>` 元素的对象，用来操作表格结构。

- **`HTMLButtonElement`**: 代表 `<button>` 元素的对象，控制按钮相关的行为和属性。

- **`HTMLFormElement`**: 代表 `<form>` 元素的对象，主要用于提交表单数据和处理表单事件。

- **`Document`**: 代表整个 HTML 或 XML 文档，是所有 DOM 节点的入口点，可以使用 `document.getElementById()`、`document.querySelector()` 等方法来获取其他 DOM 元素。

- **`Window`**: 代表浏览器的窗口对象，用来控制整个浏览器窗口和事件。

### 2. **`HTMLDivElement`**

`HTMLDivElement` 是一个表示 `<div>` 元素的对象。它继承自 `HTMLElement`，代表 HTML 文档中的一个 `<div>` 元素。当你在 JavaScript 中通过 `document.createElement('div')` 或者通过选择一个 `<div>` 元素时，你会得到一个 `HTMLDivElement` 对象。

**例子**:
```javascript
let div = document.createElement('div'); // 创建一个 <div> 元素
console.log(div instanceof HTMLDivElement); // true
```

- `HTMLDivElement` 代表 `<div>` 元素，并且拥有所有 `HTMLElement` 的属性和方法，比如 `innerHTML`、`classList` 等。

### 3. **`HTMLCollection`**

`HTMLCollection` 是一种类似数组的对象，代表一个动态更新的 HTML 元素集合。它通常是通过 `document.getElementsByTagName()`、`document.getElementsByClassName()` 或者 `children` 属性返回的，包含指定标签或类名的所有元素。`HTMLCollection` 只包含元素节点。

**特点**：

- **动态**：如果 DOM 结构改变，`HTMLCollection` 会自动更新。
- **类似数组**：可以使用 `length` 属性和索引访问元素，但不能直接使用数组的方法（如 `forEach`），必须转换成数组。

**例子**:
```javascript
let divs = document.getElementsByTagName('div');
console.log(divs instanceof HTMLCollection); // true
console.log(divs.length); // 获取页面中所有 <div> 的数量
```

### 4. **`NodeList`**

`NodeList` 是另一种类似数组的对象，包含文档中的节点列表。它可以包含多种类型的节点（不仅仅是元素节点，还包括文本节点、注释节点等）。`NodeList` 通常通过 `querySelectorAll()`、`childNodes`、`getElementsByName()` 返回。

**特点**：
- `NodeList` 可能是 **静态** 的（即，它不会随着 DOM 的变化而改变）或 **动态** 的（会随着 DOM 的改变而更新）。例如，`childNodes` 返回的是动态的 `NodeList`，而 `querySelectorAll()` 返回的是静态的 `NodeList`。
- 与 `HTMLCollection` 不同，`NodeList` 可以使用一些数组方法（如 `forEach()`）。

**例子**:
```javascript
let divs = document.querySelectorAll('div'); // 使用 CSS 选择器查找所有 <div>
console.log(divs instanceof NodeList); // true
divs.forEach(div => console.log(div)); // 可以使用 forEach 遍历
```

### 区别总结

- **`HTMLDivElement`**：表示一个具体的 `<div>` 元素对象。
- **`HTMLCollection`**：表示一个动态的 HTML 元素集合（只包含元素节点，像数组但不是真正的数组）。
- **`NodeList`**：表示一个节点列表，可以包含不同类型的节点（包括元素、文本等）。可以是静态或动态，并且支持某些数组方法。

## 代码示例

```html
<!DOCTYPE html>
<html>
<head>
    <title>Js选择器</title>
    <meta charset="utf-8">
</head>
<body>

    <div id="t1">D1</div>

    <div class="t2">D2</div>
    <div class="t2">D3</div>

    <div name="t3">D4</div>
    <div name="t3">D5</div>

    <p class="t4">P6</p>
    <p class="t4">P7</p>

    <div>
        <div class="t5">D8</div>
    </div>

    <div>
        <div id="t6">D9</div>
        <div>D10</div>
        <div>D11</div>
    </div>

</body>
<script type="text/javascript">
    var t1 = document.getElementById("t1");
    console.log(t1); // <div id="t1">D1</div>
    console.log(Object.prototype.toString.call(t1)); // [object HTMLDivElement]
    console.log("");

    var t2List = document.getElementsByClassName("t2");
    console.log(t2List); // HTMLCollection(2) [div.t2, div.t2]
    // 使用for循环遍历
    for(let i=0,n=t2List.length;i<n;++i) console.log(t2List[i]);
    // HTMLCollection的prototype中没有forEach方法，遍历需要使用Array的prototype中forEach通过call绑定对象实例并传参
    Array.prototype.forEach.call(t2List,v => console.log(v) ); 
    // HTMLCollection的prototype中没有map方法，也需要使用Array的prototype中forEach通过call绑定对象实例并传参
    Array.prototype.map.call(t2List,v => console.log(v) ); 
    console.log("");

    var t3List = document.getElementsByName("t3");
    console.log(t3List); // NodeList(2) [div, div]
    // 可直接使用forEach进行遍历
    t3List.forEach( v => console.log(v) ); 
    // NodeList的prototype中没有map方法，使用map的场景也需要借助Array的prototype中map通过call绑定对象实例并传参
    Array.prototype.map.call(t3List,v => console.log(v) ); 
    console.log("");

    var t4List = document.getElementsByTagName("p");
    console.log(t4List); // HTMLCollection(2) [p, p]
    Array.prototype.forEach.call(t4List, function(v){console.log(v);}); 
    Array.prototype.map.call(t4List,function(v){console.log(v);} ); 
    console.log("");

    var t5 = document.querySelector("div > .t5");
    console.log(t5); // <div class="t5">D8</div>
    console.log(Object.prototype.toString.call(t5)); // [object HTMLDivElement]
    console.log("");

    var t6List = document.querySelectorAll("#t6 ~ div");
    console.log(t6List); // NodeList(2) [div, div]
    t6List.forEach(function(v){console.log(v);}); 
    Array.prototype.map.call(t6List,function(v){console.log(v);} ); 
    console.log("");
</script>
</html>
```

## 每日一题

```
https://github.com/WindrunnerMax/EveryDay
```

## 参考

```
数组的遍历 https://github.com/WindrunnerMax/EveryDay/blob/master/JavaScript/Js%E9%81%8D%E5%8E%86%E6%95%B0%E7%BB%84%E6%80%BB%E7%BB%93.md
ES6箭头函数 https://github.com/WindrunnerMax/EveryDay/blob/master/JavaScript/ES6%E6%96%B0%E7%89%B9%E6%80%A7.md
原型与原型链 https://github.com/WindrunnerMax/EveryDay/blob/master/JavaScript/%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.md
CSS选择器 https://github.com/WindrunnerMax/EveryDay/blob/master/CSS/CSS%E9%80%89%E6%8B%A9%E5%99%A8.md
```