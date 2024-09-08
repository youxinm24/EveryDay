# Generator函数
生成器`generator`是`ES6`标准引入的新的数据类型，一个`generator`看上去像一个函数，但可以返回多次，通过`yield`关键字，把函数的执行流挂起，为改变执行流程提供了可能，从而为异步编程提供解决方案。

## 方法
* `Generator.prototype.next()`：返回一个由`yield`表达式生成的值。
* `Generator.prototype.return()`：返回给定的值并结束生成器。
* `Generator.prototype.throw()`：向生成器抛出一个错误。

## 实例
使用`function*`声明方式会定义一个生成器函数`generator function`，它返回一个`Generator`对象，可以把它理解成，`Generator`函数是一个状态机，封装了多个内部状态，执行`Generator`函数会返回一个遍历器对象。  
调用一个生成器函数并不会马上执行它里面的语句，而是返回一个这个生成器的迭代器`iterator `对象，他是一个指向内部状态对象的指针。当这个迭代器的`next()`方法被首次（后续）调用时，其内的语句会执行到第一个（后续）出现`yield`的位置为止，`yield`后紧跟迭代器要返回的值，也就是指针就会从函数头部或者上一次停下来的地方开始执行到下一个`yield`。或者如果用的是`yield*`，则表示将执行权移交给另一个生成器函数（当前生成器暂停执行）。  
`next()`方法返回一个对象，这个对象包含两个属性：`value`和`done`，`value`属性表示本次`yield`表达式的返回值，`done`属性为布尔类型，表示生成器后续是否还有`yield`语句，即生成器函数是否已经执行完毕并返回。  

```javascript
function* f(x) {
    yield x + 10;
    yield x + 20;
    return x + 30;
}
var g = f(1);
console.log(g); // f {<suspended>}
console.log(g.next()); // {value: 11, done: false}
console.log(g.next()); // {value: 21, done: false}
console.log(g.next()); // {value: 31, done: true}
console.log(g.next()); // {value: undefined, done: true} // 可以无限next()，但是value总为undefined，done总为true
```
调用`next()`方法时，如果传入了参数，那么这个参数会传给上一条执行的`yield`语句左边的变量。

```javascript
function* f(x) {
    var y = yield x + 10;
    console.log(y);
    yield x + y;
    console.log(x,y);
    return x + 30;
}
var g = f(1);
console.log(g); // f {<suspended>}
console.log(g.next()); // {value: 11, done: false}
console.log(g.next(50)); // {value: 51, done: false} // y被赋值为50
console.log(g.next()); // {value: 31, done: true} // x,y 1,50
console.log(g.next()); // {value: undefined, done: true}
```
### 1. `var g = f(1)` 的时候，`f(1)` 会运行吗？

当你执行 `var g = f(1)` 时，`f(1)` **不会立即执行**，它只会创建一个 `Generator` 对象，准备好以后通过 `g.next()` 来逐步执行。

`Generator` 函数只有在调用 `g.next()` 时才会开始执行。在 `var g = f(1)` 这一步时，`g` 是一个 `Generator` 对象，它还没有运行到 `yield`。

你可以通过 `console.log(g)` 来查看生成的 `Generator` 对象：

```javascript
var g = f(1);
console.log(g); // f {<suspended>} - 表示 generator 函数已经暂停，尚未开始执行。
```

### 2. 每次 `next()`，函数进到哪个步骤？

每次调用 `g.next()`，`Generator` 函数会从上一次暂停的地方继续执行，直到遇到下一个 `yield` 或执行结束。

具体过程如下：

- 第一次 `g.next()`：启动 `Generator` 函数，执行到第一个 `yield x + 10` 处并暂停，返回 `{ value: 11, done: false }`。
- 第二次 `g.next()`：继续执行，从第一个 `yield` 处恢复，执行到 `yield x + 20` 处并暂停，返回 `{ value: 21, done: false }`。
- 第三次 `g.next()`：继续执行，返回值 `x + 30` 并且函数完成，返回 `{ value: 31, done: true }`。
- 第四次及后续 `g.next()`：因为 `Generator` 已经完成，返回 `{ value: undefined, done: true }`。

### 3. 遇到 `yield` 时会停在 `yield` 前面还是后面？

当 `Generator` 函数遇到 `yield` 时，它会 **停在 `yield` 表达式的右侧**，也就是说，`yield` 的右侧的值会被返回，并且函数暂停执行。

例如，在 `yield x + 10` 处，`x + 10` 会被计算并返回给 `next()` 调用者，然后函数会暂停，等待下一次 `next()` 调用继续。

### 代码分析

```javascript
function* f(x) {
    yield x + 10; // 第一次 yield: 暂停在这里，返回 11
    yield x + 20; // 第二次 yield: 暂停在这里，返回 21
    return x + 30; // 第三次: 直接返回 31，完成 Generator 函数
}

var g = f(1);
console.log(g);             // f {<suspended>}
console.log(g.next());       // {value: 11, done: false} - 执行到 yield x + 10
console.log(g.next());       // {value: 21, done: false} - 执行到 yield x + 20
console.log(g.next());       // {value: 31, done: true} - 执行到 return x + 30
console.log(g.next());       // {value: undefined, done: true} - Generator 函数已经完成
```

#### 第一次调用 `g.next()`：
- `yield x + 10` 被执行，`x + 10 = 11`，返回 `{ value: 11, done: false }`，并暂停在 `yield` 处。

#### 第二次调用 `g.next()`：
- 从第一个 `yield` 处继续执行，`yield x + 20` 被执行，`x + 20 = 21`，返回 `{ value: 21, done: false }`，并再次暂停。

#### 第三次调用 `g.next()`：
- 从第二个 `yield` 处继续执行，遇到 `return x + 30`，返回 `{ value: 31, done: true }`，表示 `Generator` 函数执行完成。

#### 第四次及之后的 `g.next()`：
- 函数已经完成，继续调用 `next()` 会返回 `{ value: undefined, done: true }`。

### 总结

- `var g = f(1)` 创建了一个 `Generator` 对象，但 `f(1)` 不会立即执行。
- 每次调用 `g.next()`，`Generator` 函数会继续执行到下一个 `yield` 语句或函数结束。
- 遇到 `yield` 时，函数会暂停，并返回 `yield` 右侧的值。

若显式指明`return`方法给定返回值，则返回该值并结束遍历`Generator`函数，若未显式指明`return`的值，则返回`undefined`

```javascript
function* f(x) {
    yield x + 10;
    yield x + 20;
    yield x + 30;
}
var g = f(1);
console.log(g); // f {<suspended>}
console.log(g.next()); // {value: 11, done: false}
console.log(g.next()); // {value: 21, done: false}
console.log(g.next()); // {value: 31, done: false} // 注意此处的done为false
console.log(g.next()); // {value: undefined, done: true}
```
`yield*`表达式表示`yield`返回一个遍历器对象，用于在`Generator`函数内部，调用另一个 `Generator`函数。

```javascript
function* callee() {
    yield 100;
    yield 200;
    return 300;
}
function* f(x) {
    yield x + 10;
    yield* callee();
    yield x + 30;
}
var g = f(1);
console.log(g); // f {<suspended>}
console.log(g.next()); // {value: 11, done: false}
console.log(g.next()); // {value: 100, done: false}
console.log(g.next()); // {value: 200, done: false}
console.log(g.next()); // {value: 31, done: false}
console.log(g.next()); // {value: undefined, done: true}
```
## 应用场景

### 异步操作的同步化表达

```javascript
var it = null;

function f(){
    var rand = Math.random() * 2;
    setTimeout(function(){
        if(it) it.next(rand);
    },1000)
}
function success(r1,r2,r3){
    console.log(r1,r2,r3); // 0.11931234806372775 0.3525336021860719 0.39753321774160844
}
// 成为线性任务而解决嵌套
function* g(){ 
    var r1 = yield f();
    console.log(r1);
    var r2 = yield f();
    console.log(r2);
    var r3 = yield f();
    console.log(r3);
    success(r1,r2,r3);
}

it = g();
it.next();
```

是的，将 `Promise` 对象与 `Generator` 函数结合使用，可以让异步代码看起来像同步执行。通过在 `yield` 中返回 `Promise` 对象，再使用 `next()` 调用，可以将异步代码线性表达。这通常是通过将 `Generator` 函数与 `next()` 调用结合起来手动处理，也可以通过 `async/await` 简化这一流程。

下面是一个例子，展示如何通过 `Generator` 函数处理异步 `Promise` 对象：

### 示例：在 `yield` 里返回 `Promise`

```javascript
function* generator() {
    const result1 = yield new Promise(resolve => setTimeout(() => resolve(10), 1000));
    console.log(result1);  // 10
    
    const result2 = yield new Promise(resolve => setTimeout(() => resolve(20), 1000));
    console.log(result2);  // 20
    
    const result3 = yield new Promise(resolve => setTimeout(() => resolve(30), 1000));
    console.log(result3);  // 30
}

function run(gen) {
    const g = gen();  // 初始化 generator 函数

    function handle(result) {
        if (result.done) return;  // 结束时退出

        // result.value 是一个 Promise，处理完成后继续下一步
        result.value.then(res => {
            handle(g.next(res));  // 使用 Promise 结果作为 next() 的传参
        });
    }

    handle(g.next());  // 开始 generator 的执行
}

run(generator);
```

### 解释：
1. **`yield` 返回 Promise 对象**：在 `yield` 语句中返回一个 `Promise`，每次 `yield` 暂停 Generator 函数的执行，等待 Promise 解析。
2. **`run` 函数**：`run` 函数会递归调用 `g.next()` 来驱动 Generator 的执行，每当一个 `Promise` 解析完之后，就将结果通过 `next()` 传回给 Generator，并继续执行下一个 `yield`。
3. **线性表达**：通过这种方式，虽然代码中执行了多个异步操作，但 Generator 函数看起来像是同步的顺序执行。

### 总结：
- 通过 `yield` 暂停 `Generator` 函数，并在 `yield` 中返回 `Promise` 对象。
- 调用 `next()` 时，异步操作完成后可以将结果传回 Generator 函数，逐步线性执行每个异步操作，保持代码的同步风格。

### 整理

```
长轮询 https://www.cnblogs.com/wjyz/p/11102379.html
延迟执行 无限序列 http://www.hubwiz.com/exchange/57fb046ce8424ba757b8206d
斐波那契数列 https://www.liaoxuefeng.com/wiki/1022910821149312/1023024381818112
迭代器 https://zhuanlan.zhihu.com/p/24729321?utm_source=tuicool&utm_medium=referral
线性方式 https://blog.csdn.net/astonishqft/article/details/82782422?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
```

## 每日一题

```
https://github.com/WindrunnerMax/EveryDay
```


## 参考

```
https://www.runoob.com/w3cnote/es6-generator.html
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator
```

