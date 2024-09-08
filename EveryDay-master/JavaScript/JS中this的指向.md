# JS中this的指向

`this`的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定`this`到底指向谁，实际上`this`的最终指向的是那个调用它的对象。

## 实例
定义函数与对象并调用，注意只有调用函数才会使`this`指向调用者，但箭头函数除外。
```javascript
function s(){
    console.log(this);
}

// window中直接调用 // 非 use strict
s(); // Window // 等同于window.s()，调用者为window
// window是Window的一个实例 // window instanceof Window //true

// 新建对象s1
var s1 = {
    t1: function(){ 
        console.log(this); // s1, S1调用的console.log
        s(); // Window             // 此次调用仍然相当 window.s()，调用者为window
    },
    t2: () => { // 测试箭头函数，this并未指向调用者
        console.log(this); //window 
        				  //箭头函数不会创建自己的 this，它会捕获定义时的 this，并且永久绑定到它的定义环境中(不会改变)
        				 //箭头函数不会创建自己的 this，它继承定义时的 this。因为 t2 是在全局环境下定义的，this 绑定到 window，所以输出 window。
    },
    t3: { // 测试对象中的对象
      tt1: function() {
           console.log(this);//S1.t3  //同S1.t1 返回调用的上一级 
      }  
    },
    t4: { // 测试箭头函数以及非函数调用this并未指向调用者
      tt1: () => {
           console.log(this); 
      }  
    },
    t5: function(){ // 测试函数调用时箭头函数的this的指向，其指向了上一层对象的调用者
        return {
            tt1: () => {
                console.log(this); //t5返回了一个函数,这个函数是在s1环境下定义的,所以是S1
            }
        }
    }
}
s1.t1(); // s1对象 // 此处的调用者为 s1 所以打印对象为 s1
s1.t2(); // Window
s1.t3.tt1(); // s1.t3对象
s1.t4.tt1(); // Window
s1.t5().tt1(); // s1对象
```

## 箭头函数this指向：

1. **箭头函数定义在对象内部**:
   - 当箭头函数定义在对象的属性方法中，并且这个方法是通过对象调用的，箭头函数的 `this` **不会指向对象本身**，而是指向全局作用域。
   - 这是因为箭头函数在定义时不会创建自己的 `this`，它继承的是定义时所在作用域的 `this`，而在全局作用域中，`this` 通常指向 `window`（在浏览器中）。

   **示例 1**:
   ```javascript
   const obj = {
       name: 'Object Scope',
       method: () => {
           console.log(this.name); // this 指向全局作用域，`this.name` 为 undefined
       }
   };
   obj.method(); // 输出：undefined
   ```
   - 在这个例子中，`method` 是箭头函数，`this` 绑定在**全局作用域**，而不是 `obj`，因此 `this.name` 为 `undefined`。

2. **箭头函数定义在普通函数的内部**:
   - 当箭头函数定义在普通函数内部时，`this` 会继承**定义它的函数中的 `this`**。如果该普通函数的 `this` 指向某个对象，那么箭头函数的 `this` 也会指向该对象。

   **示例 2**:
   ```javascript
   const obj = {
       name: 'Object Scope',
       method() {
           const arrowFunc = () => {
               console.log(this.name); // this 继承自 method 中的 this，指向 obj
           };
           arrowFunc(); // 输出："Object Scope"
       }
   };
   obj.method(); // 输出："Object Scope"
   ```
   - 在这个例子中，`arrowFunc` 是在 `method` 函数中定义的，而 `method` 的 `this` 指向 `obj`，所以 `arrowFunc` 的 `this` 也指向 `obj`。

### 总结：
- **箭头函数定义在对象属性中**：`this` **不会指向对象**，而是继承全局作用域中的 `this`。
- **箭头函数定义在函数内部**：`this` 会继承定义它的**函数作用域中的 `this`**，指向函数执行时的 `this`。

比较特殊的例子，我们调用同一个方法，但是得到的`this`是不同的，要注意实际上`this`的最终指向的是那个调用它的对象。

```javascript
var s1 = {
    t1: function(){
        console.log(this);
    }
}
s1.t1(); // s1对象
var p = s1.t1;
p(); // Window

// 注意此时将方法赋值给了p，此时直接调用p得到的this是Window
// 其实这个例子也并不是很特殊，因为函数也是一个对象，此时p是被赋值了一个函数
console.log(p); // ƒ (){console.log(this);}
// 而此函数是被window调用的，由此，this指向了window
```

## 改变this指向

```
使用 apply、call、bind可以改变this的指向，可以参考
https://github.com/WindrunnerMax/EveryDay/blob/master/JavaScript/apply%E3%80%81call%E3%80%81bind.md
```

## 每日一题

```
https://github.com/WindrunnerMax/EveryDay
```

## 参考
```
http://www.ruanyifeng.com/blog/2018/06/javascript-this.html
```