# JavaScript闭包

函数和对其词法环境`lexical environment`的引用捆绑在一起构成闭包，也就是说，**闭包可以让你从内部函数访问外部函数作用域**。在`JavaScript`，函数在每次创建时生成闭包。在本质上，闭包是将函数内部和函数外部连接起来的桥梁。

## 定义闭包
为了定义一个闭包，首先需要一个函数来套一个匿名函数。闭包是需要使用局部变量的，定义使用全局变量就失去了使用闭包的意义，最外层定义的函数可实现局部作用域从而定义局部变量，函数外部无法直接访问内部定义的变量。

```JavaScript
function student(){
    var name = "Ming";
    var sayMyName = function(){ // sayMyName作为内部函数，有权访问父级函数作用域student中的变量
        console.log(name);
    }
    console.dir(sayMyName); // ... [[Scopes]]: Scopes[2] 0: Closure (student) {name: "Ming"} 1: Global ...
    return sayMyName; // return是为了让外部能访问闭包，挂载到window对象也可以 
}
var stu = student(); 
stu(); // Ming
```
可以看到定义在函数内部的`name`变量并没有被销毁，我们仍然可以在外部使用函数访问这个局部变量，使用闭包，可以把局部变量驻留在内存中，从而避免使用全局变量。全局变量污染会导致应用程序不可预测性，每个模块都可调用必将引来灾难。

闭包调用一个函数的返回值,但是会维持这个函数一直存在,包含他的定义域,所以说是将函数内部和外部连接起来的桥梁

## 词法环境

闭包共享相同的函数定义，但是保存了不同的词法环境`lexical environment`。

```JavaScript
function student(name){
    var sayMyName = function(){
        console.log(name);
    }
    return sayMyName;
}
var stu1 = student("Ming"); 
var stu2 = student("Yang"); 
stu1(); // Ming
stu2(); // Yang
```

## 模拟私有方法
在面向对象的语言中，例如`Java`、`PHP`等，都是支持定义私有成员的，即只有类内部能够访问，而无法被外部类访问。`JavaScript`并未原生支持定义私有成员，但是可以使用闭包来模拟实现，私有方法不仅仅有利于限制对代码的访问，还提供了管理全局命名空间的强大能力，避免非核心的方法弄乱了代码的公共接口部分。

```JavaScript
function student(){
    var HP = 100;
    var addHP = function(){
        return ++HP;
    }
    var decHP = function(){
        return --HP;
    }
    return {
        addHP,
        decHP
    };
}
var stu = student();
console.log(stu.HP); // undefined 不允许直接访问
console.log(stu.addHP()); // 101
console.log(stu.decHP()); // 100
```

## 回调机制
`Js`的闭包为回调机制提供了支持，无论函数是否立马被调用，这个闭包都不会被释放。而且在`Js`里，无论把`callback`函数作为参数传递给其他函数，或者作为返回值返回，以便于之后调用，都是合法的。

```javascript
function localContext(){
    var localVal = 1;
    var callback = function(){
        console.log(localVal); // ... [[Scopes]]: Scopes[2] 0: Closure (localContext) {localVal: 1} 1: Global ...
    }
    console.dir(callback);
    setTimeout(callback, 1000); // 1
}
localContext();
```
在本例中，`callback`函数与其词法环境构成了闭包，其词法环境中存在的变量`localVal = 1`在函数`callback`作为回调函数传递时并没有被立即释放，而可以在回调执行时继续使用，这就是闭包为回调机制提供了支持。

## 循环创建闭包
在`ECMAScript 2015`引入`let`关键字之前，只有函数作用域和全局作用域，函数作用域中又可以继续嵌套函数作用域，在`for`并未具备局部作用域，于是有一个常见的闭包创建问题。

```JavaScript
function counter(){
    var arr = [];
    for(var i = 0 ; i < 3 ; ++i){
        arr[i] = function(){
            return i;
        }
    }
    return arr;
}

var coun = counter();
for(var i = 0 ; i < 3 ; ++i){
    console.log(coun[i]()); // 3 3 3
}
```
可以看到运行输出是`3 3 3`，而并不是期望的`0 1 2`，原因是这三个闭包在循环中被创建的时候，共享了同一个词法作用域，这个作用域由于存在一个`i`由`var`声明，由于变量提升，具有函数作用域，当执行闭包函数的时候，由于循环早已执行完毕，`i`已经被赋值为`3`，所以打印为`3 3 3`

### 匿名函数新建函数作用域来解决

```JavaScript
function counter(){
    var arr = [];
    for(var i = 0 ; i < 3 ; ++i){
        (function(i){
            arr[i] = function(){
                return i;
            }
        })(i); //立即执行函数,创建多个作用域,避免用同一个函数级的var 变量
    }
}

var coun = counter();
for(var i = 0 ; i < 3 ; ++i){
    console.log(coun[i]()); // 0 1 2
}
```

### 使用let关键字
```JavaScript
function counter(){
    var arr = [];
    for(let i = 0 ; i < 3 ; ++i){
        arr[i] = function(){
            return i;
        }
    }
    return arr;
}

var coun = counter();
for(var i = 0 ; i < 3 ; ++i){
    console.log(coun[i]()); // 0 1 2
}
```

## 内存泄漏



内存泄露是指你用不到（访问不到）的变量，依然占据着内存空间，不能被再次利用起来。  
闭包引用的变量应该是需要使用的，不应该属于内存泄漏，但是在`IE8`浏览器中`JScript.dll`引擎使用会出现一些问题，造成内存泄漏。  
对于各种引擎闭包内存回收具体的表现参阅 [这篇文章](https://www.cnblogs.com/rubylouvre/p/3345294.html)



## 内存回收

要释放一个闭包，**核心在于打破闭包对外部变量的引用**，从而让 JavaScript 引擎的垃圾回收机制（GC）能够正确地回收那些不再使用的内存资源。

### **闭包的内存管理：**

闭包的强大之处在于它能够保持对外部作用域变量的引用，使得这些变量在函数外部也能被访问。但是，这也意味着这些变量不会被垃圾回收，因为它们仍然被引用。

要释放闭包，就需要确保没有任何地方继续引用闭包的函数或其作用域链中的变量。

### **如何释放闭包：**

1. **将闭包函数设为 `null` 或移除对它的引用**：
   如果不再需要使用闭包函数，将它的引用置为 `null` 或移除所有对它的引用，就能够使垃圾回收机制清理这个闭包及其相关的内存。

   ```javascript
   function outer() {
     let count = 0;
     function inner() {
       count++;
       console.log(count);
     }
     return inner;
   }
   
   let counter = outer();  // 形成闭包
   counter(); // 输出 1
   counter = null;         // 释放闭包
   ```

   - **解释**：当 `counter = null;` 时，`counter` 不再引用 `inner` 函数，且没有其他地方引用 `inner`。因此，`inner` 的闭包作用域（包含 `count` 变量）可以被垃圾回收。

2. **在函数内部显式清除引用**：
   可以通过函数内部的逻辑主动清理闭包所持有的变量。例如，可以手动设置变量为 `null` 或清空对象引用。

   ```javascript
   function outer() {
     let count = 0;
     function inner() {
       count++;
       console.log(count);
     }
     inner.clear = function() {
       count = null;  // 清除闭包对 count 的引用
     };
     return inner;
   }
   
   let counter = outer();
   counter();  // 输出 1
   counter.clear(); // 清除闭包内的变量
   ```

   - **解释**：这里通过 `counter.clear()` 方法，手动将 `count` 设置为 `null`，使得闭包不再引用这个变量，垃圾回收器可以回收相关的内存。

3. **使用立即执行函数表达式（IIFE）并仅使用闭包一次**：
   当闭包只需要在某个地方使用一次时，可以通过**立即执行函数表达式（IIFE）**来避免产生不必要的长期引用。这样，在函数执行后，闭包的作用域链也会随即被销毁。

   ```javascript
   (function() {
     let count = 0;
     count++;
     console.log(count);
   })();  // 输出 1，闭包执行完后立即释放
   ```

   - **解释**：在这个例子中，闭包执行完之后没有任何地方持有它的引用，闭包自动被释放。

### **避免内存泄漏的建议：**

1. **慎重使用全局变量**：全局变量不会被垃圾回收，因此要尽量减少全局闭包的使用。
  
2. **清理事件处理器或回调函数**：闭包经常用于事件处理器或回调函数中。如果不再需要这些处理器，应该手动移除它们。例如，清理 DOM 事件监听器时：
  
   ```javascript
   const button = document.querySelector('button');
   function handleClick() {
     console.log('Button clicked');
   }
   button.addEventListener('click', handleClick);
    
   // 释放闭包
   button.removeEventListener('click', handleClick);
   ```

3. **使用弱引用（WeakRef）**：在某些情况下，可以使用 `WeakRef`（ES2021 引入的功能）创建对对象的弱引用，从而避免闭包导致内存泄漏。

### **垃圾回收机制和闭包：**
JavaScript 引擎的垃圾回收机制会自动回收没有被引用的内存。当闭包不再被引用或使用时，它也会自动被回收。所以，只要确保没有其他引用保持对闭包或闭包内变量的引用，垃圾回收机制就会自动释放这些资源。

### **总结：**

- **释放闭包**最关键的是**清除对闭包函数的引用**，这样 JavaScript 的垃圾回收器可以正确回收它所占的内存。
- 可以通过手动设置闭包为 `null`、显式清理变量或移除事件监听器来避免不必要的内存占用。
- 垃圾回收机制会在闭包没有引用时自动回收资源，但我们仍需要避免长期未清理的闭包导致的内存泄漏。



## 性能考量

如果不是某些特定任务需要使用闭包，在其它函数中创建函数是不明智的，因为闭包在处理速度和内存消耗方面对脚本性能具有负面影响。  
在创建新的对象或者类时，方法通常应该关联于对象的原型，而不是定义到对象的构造器中。原因是这将导致每次构造器被调用时，方法都会被重新赋值一次。

## 每日一题

```
https://github.com/WindrunnerMax/EveryDay
```

## 参考
```
https://zhuanlan.zhihu.com/p/22486908
https://www.cnblogs.com/Renyi-Fan/p/11590231.html
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures
```