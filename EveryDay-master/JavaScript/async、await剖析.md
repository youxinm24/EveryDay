# async/await剖析
`JavaScript`是单线程的，为了避免同步阻塞可能会带来的一些负面影响，引入了异步非阻塞机制，而对于异步执行的解决方案从最早的回调函数，到`ES6`的`Promise`对象以及`Generator`函数，每次都有所改进，但是却又美中不足，他们都有额外的复杂性，都需要理解抽象的底层运行机制，直到在`ES7`中引入了`async/await`，他可以简化使用多个`Promise`时的同步行为，`在编程的时候甚至都不需要关心这个操作是否为异步操作`。

## 分析

首先使用`async/await`执行一组异步操作，并不需要回调嵌套也不需要写多个`then`方法，在使用上甚至觉得这本身就是一个同步操作，当然在正式使用上应该将`await`语句放置于` try...catch`代码块中，因为`await`命令后面的`Promise`对象，运行结果可能是`rejected`。

```javascript
function promise(){
    return new Promise((resolve, reject) => {
       var rand = Math.random() * 2; 
       setTimeout(() => resolve(rand), 1000);
    });
}

async function asyncFunct(){
    var r1 = await promise();
    console.log(1, r1);
    var r2 = await promise();
    console.log(2, r2);
    var r3 = await promise();
    console.log(3, r3);
}

asyncFunct();
```
`async/await`实际上是`Generator`函数的语法糖，如`Promises`类似于结构化回调，`async/await`在实现上结合了`Generator`函数与`Promise`函数，下面使用`Generator`函数加`Thunk`函数的形式实现一个与上边相同的例子，可以看到只是将`async`替换成了`*`放置在函数右端，并将`await`替换成了`yield`，所以说`async/await`实际上是`Generator`函数的语法糖，此处唯一不同的地方在于实现了一个流程的自动管理函数`run`，而`async/await`内置了执行器，关于这个例子的实现下边会详述。对比来看，`async`和`await`，比起`*`和`yield`，语义更清楚，`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。

```javascript
function thunkFunct(){
    return function f(funct){
        var rand = Math.random() * 2;
        setTimeout(() => funct(rand), 1000)
    }
}

function* generator(){ 
    var r1 = yield thunkFunct(); // 暂停并等待 `thunkFunct()` 完成
    console.log(1, r1);
    var r2 = yield thunkFunct();
    console.log(2, r2);
    var r3 = yield thunkFunct();
    console.log(3, r3);
}

function run(generator) {
    var g = generator();  // 获取 generator 函数的迭代器对象
    
    // 负责继续执行 generator 函数
    var next = function(data) {
        var res = g.next(data);  // 执行 generator 函数，传递上一步的数据
        if (res.done) return;  // 如果 generator 函数执行完毕，停止
        
        // res.value 是 yield 返回的 thunk 函数
        // 调用 thunk 函数并传递 next 作为回调
        res.value(next);
    };
    
    next();  // 启动执行
}


run(generator);
```

### **流程分析**

1. **调用 `run(generator)`：**
   - 首先调用 `generator()`，获得 `g`，这是一个 `Generator` 的迭代器对象。
   - 然后定义 `next` 函数，负责推进 `Generator` 函数的执行。
   - 调用 `next()` 启动 `generator` 函数的执行。
2. **第一次 `g.next()` 执行：**
   - `generator` 函数启动，执行到 `var r1 = yield thunkFunct();`，这时函数暂停，`yield` 语句返回 `thunkFunct()`，即 `thunkFunct` 返回的函数。
   - `res.value(next)` 执行这个返回的 `thunk` 函数，等待 1 秒后调用 `next(rand)`。
3. **`thunk` 函数在 1 秒后调用 `next(rand)`：**
   - `rand` 是 `Math.random() * 2` 生成的随机数，`next(rand)` 会继续 `generator` 的执行，并将 `rand` 赋值给 `r1`。
   - `generator` 函数继续执行，打印 `1, r1`。
4. **第二次 `g.next()`：**
   - `generator` 函数再次执行到 `var r2 = yield thunkFunct();`，再次暂停并返回 `thunkFunct`，类似第一次的流程。
5. **第三次执行：**
   - `generator` 函数再执行到第三次 `yield thunkFunct()`，最后一次暂停和恢复，直到 `generator` 完全执行完毕。

## 手写语法糖async

要使用 `Generator` 实现类似 `async/await` 的功能，可以手动管理 `Promise` 的执行顺序和异步调用。通过 `Generator` 函数的 `yield`，我们可以暂停函数的执行，等待异步操作完成后再继续执行。为了实现这个效果，需要一个自动运行 `Generator` 的调度器来处理 `Promise`。

这是通过 `Generator` 模拟 `async/await` 的常见方法：

### 1. 基本思路

- 使用 `Generator` 函数来 `yield` 一个 `Promise`，在 `Promise` 完成时恢复执行。
- 一个调度器（runner function）负责执行 `Generator` 并处理 `Promise`。

### 2. 实现示例

下面是一个基于 `Generator` 模拟 `async/await` 的简单例子：

```javascript
function asyncGenerator(generator) {
    const gen = generator();

    function handleNext(nextValue) {
        const result = gen.next(nextValue); //在next()传入值会代替上一个yield的返回值 

        // 如果 generator 完成了，直接返回
        if (result.done) return result.value;

        // 否则，处理返回的 Promise 并继续执行
        return Promise.resolve(result.value).then(  //这个地方是看Promise会不会返回另一个Promise
            res => handleNext(res),  // 成功时递归调用  //继续调用next ,并传入上一个的Promise值为返回值
            err => gen.throw(err)    // 失败时抛出异常
        );
    }

    // 开始执行 generator
    return handleNext();
}
```

### 3. 使用方式

现在我们可以使用这个 `asyncGenerator` 函数来执行异步任务，就像使用 `async/await` 一样。

```javascript
function* myAsyncTask() {
    const res1 = yield new Promise(resolve => setTimeout(() => resolve('Result 1'), 1000));
    console.log(res1);  // 打印 'Result 1'

    const res2 = yield new Promise(resolve => setTimeout(() => resolve('Result 2'), 1000));
    console.log(res2);  // 打印 'Result 2'

    const res3 = yield new Promise(resolve => setTimeout(() => resolve('Result 3'), 1000));
    console.log(res3);  // 打印 'Result 3'

    return 'All done!';
}

// 调用我们的 asyncGenerator，模拟 async/await 的行为
asyncGenerator(myAsyncTask).then(finalResult => {
    console.log(finalResult);  // 打印 'All done!'
});
```

### 4. 解释

- `myAsyncTask` 是一个 `Generator` 函数，它在每个 `yield` 处暂停，等待 `Promise` 解析（就像 `await`）。
- `asyncGenerator` 函数负责运行 `Generator`，并确保每次 `yield` 返回的 `Promise` 被正确处理。
  - `handleNext` 函数递归调用自身，处理每个 `Promise` 的结果。
  - 一旦 `Promise` 被解决（成功或失败），它会恢复 `Generator` 的执行，继续进行下一个异步操作，直到 `Generator` 完成。

### 5. 与 `async/await` 类似

这个模式基本上模仿了 `async/await` 的行为：
- 每次 `yield` 类似于 `await`，它会暂停 `Generator`，等待异步操作完成。
- 异步操作完成后，`Generator` 恢复执行并处理返回值。

### 总结

- 使用 `Generator` 和 `Promise` 可以实现 `async/await` 的效果。
- 需要一个调度器来自动执行 `Generator` 函数，并正确处理 `Promise`。

## 实现

`async`函数内置了执行器，能够实现函数执行的自动流程管理，通过`Generator yield Thunk`、`Generator yield Promise`实现一个自动流程管理，只需要编写`Generator`函数以及`Thunk`函数或者`Promise`对象并传入自执行函数，就可以实现类似于`async/await`的效果。

### Generator yield Thunk

自动流程管理`run`函数

1.首先需要知道在调用`next()`方法时，如果传入了参数，那么这个参数会传给上一条执行的`yield`语句左边的变量

2.在这个函数中，第一次执行`next`时并未传递参数，而且在第一个`yield`上边也并不存在接收变量的语句，无需传递参数，

3.接下来就是判断是否执行完这个生成器函数，在这里并没有执行完，那么将自定义的`next`函数传入`res.value`中，这里需要注意`res.value`是一个函数，可以在下边的例子中将注释的那一行执行，然后就可以看到这个值是`f(funct){...}`，此时我们将自定义的`next`函数传递后，

4.就将`next`的执行权限交予了`f`这个函数，在这个函数执行完异步任务后，会执行回调函数，在这个回调函数中会触发生成器的下一个`next`方法，

5.并且这个`next`方法是传递了参数的，上文提到传入参数后会将其传递给上一条执行的`yield`语句左边的变量，那么在这一次执行中会将这个参数值传递给`r1`，然后在继续执行`next`，不断往复，直到生成器函数结束运行，这样就实现了流程的自动管理。

```javascript
function thunkFunct(){
    return function f(funct){
        var rand = Math.random() * 2;
        setTimeout(() => funct(rand), 1000)
    }
}

function* generator(){ 
    var r1 = yield thunkFunct();
    console.log(1, r1);
    var r2 = yield thunkFunct();
    console.log(2, r2);
    var r3 = yield thunkFunct();
    console.log(3, r3);
}

function run(generator){
    var g = generator();

    var next = function(data){
        var res = g.next(data);
        if(res.done) return ;
        // console.log(res.value);
        res.value(next);
    }

    next();
}

run(generator);
```

### Generator yield Promise

相对于使用`Thunk`函数来做流程自动管理，使用`Promise`来实现相对更加简单，`Promise`实例能够知道上一次回调什么时候执行，通过`then`方法启动下一个`yield`，不断继续执行，这样就实现了流程的自动管理。

```javascript
function promise(){
    return new Promise((resolve,reject) => {
        var rand = Math.random() * 2;
        setTimeout( () => resolve(rand), 1000);
    })
}

function* generator(){ 
    var r1 = yield promise();
    console.log(1, r1);
    var r2 = yield promise();
    console.log(2, r2);
    var r3 = yield promise();
    console.log(3, r3);
}

function run(generator){
    var g = generator();

    var next = function(data){
        var res = g.next(data);
        if(res.done) return ;
        res.value.then(data => next(data));
    }

    next();
}

run(generator);
```

```javascript
// 比较完整的流程自动管理函数
function promise(){
    return new Promise((resolve,reject) => {
        var rand = Math.random() * 2;
        setTimeout( () => resolve(rand), 1000);
    })
}

function* generator(){ 
    var r1 = yield promise();
    console.log(1, r1);
    var r2 = yield promise();
    console.log(2, r2);
    var r3 = yield promise();
    console.log(3, r3);
}

function run(generator){
    return new Promise((resolve, reject) => {
        var g = generator();
        
        var next = function(data){
            var res = null;
            try{
                res = g.next(data);
            }catch(e){
                return reject(e);
            }
            if(!res) return reject(null);
            if(res.done) return resolve(res.value);
            Promise.resolve(res.value).then(data => {
                next(data);
            },(e) => {
                throw new Error(e);
            });
        }
        
        next();
    })
   
}

run(generator).then( () => {
    console.log("Finish");
});
```



## 每日一题

```
https://github.com/WindrunnerMax/EveryDay
```

## 参考

```
https://segmentfault.com/a/1190000007535316
http://www.ruanyifeng.com/blog/2015/05/co.html
http://www.ruanyifeng.com/blog/2015/05/async.html
```
