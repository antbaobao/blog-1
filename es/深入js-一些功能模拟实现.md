# js功能函数的模拟实现

这里的模拟实现，目的是在于提升对重要函数的理解

[1 call、apply](#1-call、apply)  
[2 bind](#2-bind)  
[3 reduce](#3-reduce)  
[4 promise](#4-promise)  

## 1 call、apply

call: 在调用的时候，修改调用对象的this指向，可以附带多个参数，可以有返回值

模拟思路：主要是修改this的指向，让其指向call的第一个参数

实现方式：让调用的函数对象作为传入的第一个参数的属性，执行完成之后删除它

```javascript
// call
Function.prototype.call2 = Function.prototype.call2 || function (context) {
    var context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

    var result = eval('context.fn(' + args +')');

    delete context.fn
    return result;
}

// apply
Function.prototype.apply = Function.prototype.apply || function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }

    delete context.fn
    return result;
}
```

## 2 bind

需要解决的几个问题：

1. bind返回的是一个函数
2. 当bind返回的函数作为构造器调用时，其this指向指向实例对象，不指向bind方法的第一个参数对象
3. 调用bind，可以传入参数作为预置参数

```javascript
// bind
Function.prototype.bind2 = Function.prototype.bind2 || function (context) {

    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var self = this; // 保存最原始的函数对象
    var args = Array.prototype.slice.call(arguments, 1);//预置参数

    var fNOP = function () {};

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP(); // 继承 fNOP
    return fBound;
}
```

## 3 reduce

基本应用

```javascript
//Array.prototype.reduce
var numbers = [65, 44, 12, 4];
function getSum(total, num) {
    return total + num;
}
console.log(numbers.reduce(getSum)) // 65 + 44 + 12 + 4 = 125
```

可以看出 reduce 是将数组中的每个元素依次在其传入的函数中执行，执行结果作为下一次执行的第一个参数。

****

模拟实现

```javascript
// 第一版 reduce 模拟
function myReduce(func, initData) {
    let result = this[0], start = 1;
    if(typeof initData !== 'undefined') {
        result = initData;
        start = 0;
    }
    for(let i = start;i<this.length;i++) {
        result = func(result, this[i])
    }
    return result;
}

// test
var numbers = [65, 44, 12, 4];
function getSum(total, num) {
    return total + num;
}
console.log(myReduce.call(numbers, getSum)) // 125
```

## 4 promise

time：2018.10.09

项目中很多地方用到 promise 的地方，比如按需加载 import 返回的是 promise，请求数据 fetch、axios 返回的也是 promise，还有 generator, async 与 promise 的结合使用等

先看看基本使用情况

```javascript
// promise 使用
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});
promise.then(function() {
  console.log('resolved.');
});
console.log('Hi!');
// Promise
// Hi!
// resolved
```

特点：

1. 构造函数内部内容会立即执行
2. 3种状态：pending, fulfilled, rejected
3. promise 函数执行返回新的 promise
4. promise 函数参数：resolve, reject。都是函数，链式调用参数传递通过 `resolve(params)` 传递
5. 原型方法有：then, catch, finally
6. 构造函数方法有：all, race, resolve, reject

****

模拟实现

暂时不做
