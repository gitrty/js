## 闭包



## this指向

### 1、介绍：

- 函数在定义时，是不知道 this 的指向的
- 只有在函数执行时，才有 this 这个概念
- **谁调用这个函数，this就指向谁，如果没有被调用，在全局自执行，则this指向window**

### 2、改变this指向

#### call

语法：

```js
fn.call( 规定this指向 , 参数1 , 参数2 , ... )
```

可通过 call 来改变this指向，call 会改变 this 指向并**立即执行一次函数**

例：

```js
function fn( n , m ){
    console.info(this)
}

fn()    // window
fn.call(document,2,4)  // document
```

#### apply

语法：

```js
fn.apply( 规定this指向 , [ 参数1 , 参数2 , ...] )
```

参数传递为数组形式，其他使用方式与 call 相同，也会**立即调用一次函数**

#### bind 

兼容性 ： IE8+

语法：

```js
fn.bind( 规定this指向 , 参数1 , 参数2 , ... )
```

改变this的指向，返回值为一个改变过this指向的原函数，**不会立即调用一次函数**

例：

```js
function fn( n , m ){
    console.info(this)
}

var x = fn.bind(document,2,4)  
console.info(x)    // function fn(n,m){ console.info(this) }
x()  // document
x()  // document
```

#### 注意

```js
错误写法 ： fn().call(obj)   // 函数不能加(),会执行
正确写法 ： fn.call(obj)
```

#### 手写bind函数（了解）

```js
Function.prototype.mybind = function () {
    let self = this
    let that = arguments[0]
    let arg = Array.from(arguments).slice(1, arguments.length)
    return function () {
        self.call(that, ...arg, ...arguments)
    }
}

function a(n, m, z ) {
    console.info(this)  // document
    console.info(n);    // 1
    console.info(m);    // 2
    console.info(z);    // 55
}

// 柯里化 
let x = a.mybind(document, 1, 2)
x(55)
```

###### 函数柯里化

 柯里化就是利用了函数闭包的特性 ,将接受多个参数的函数,将变为接受单一参数的函数,在函数内部return一个函数,内部函数用来接受剩余参数,并返回结果   

作用：固定部分参数，再调用时传入剩余参数

#### 事件触发函数改变this指向

```js
var obj = { name:"铁柱" }

function fn(){
    console.info(this)
}

// 方式1 - bind
document.onclick = fn.bind(obj)

// 方式2 - call （或apply）
document.onclick = function(){
    fn.call(obj)
}
```



