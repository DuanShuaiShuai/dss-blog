---
title: 变换莫测的 this
---
## 1. 作用域链

- 函数在执行的时候会形成一个 "函数调用栈"
- 函数的定义与函数的运行的内存关系
  - js 在 v8 中虽然有编译型语言的特征, 但是它依旧是解释性语言
    - js 的函数是惰性执行
    - 如果 函数不被调用, 其不会再内存中创建任何东西
      - js 中函数在执行的时候 js 引擎会给函数分配内存
      - js 中的函数如果不执行, 那么 js 引擎就会忽略他, 不分配任何内存
  - js 的函数在执行之前不会分配内存
  - js 中的函数在执行的时候会分配内存
  - js 中的函数执行完成以后, 若内存中的数据不被再引用, 则这段内存会被 js 引擎回收掉
js 函数在运行的时候 ( ing ), 会有一个与之相关的内存被申请处理

问题:

1. 一个函数如果被连续的调用两次, 那么会分配几个内存? 
  它会分配两段内存, 其内存是没有关系.
2. 这个内存中是用来干什么呢? 这个内存被称为 活动对象 ( active object )
  这个活动对象中存储的就是函数的参数, 与函数内定义的变量等.

## 1.变量的搜索原则

- 由于一个活动对象中可以有多个数据( 标识符 ), 我们将其抽象的想象成一排盒子
- 多个活动对象是有一个链接关系, 由于一个复杂的代码结构移动存在很多活动对象链接起来, 那么可以形成一个链式结构, 常常将所有的活动对象的整体称为 "作用域链"
- 变量搜索原则
   - 凡是需要访问某一个变量, 首先会在当前作用域( 活动对象 )上去查找是否存在该变量
   - 如果存在, 则使用该变量
   - 若不存在, 则进入到上一级作用域 ( 也就是与之相关联的上一个活动对象 )来查找.
   - 若在上一级作用域查找到了, 则直接使用
   - 若依旧没有, 则继续向上一级作用域查找
   - 总会找到全局作用域 ( 全局活动对象 ), 若全局也没有 则会 抛出一个错误
      xxxx is not defined

## 2. eval 与 Function

语法: 

```js
eval( '代码字符串' )

var  func = new Function( '参数1', '参数2', ..., '参数N', '函数体' );
fucn();
```

1. eval 在执行代码的时候, 代码所处的作用域为当前作用域
2. 如果 eval 被引用后执行代码, 那么代码所处的作用域为全局作用域
3. Function 都是在全局作用域下 




## 3. 函数的调用模式

函数的 5 中调用模式( ES3 时代 函数有 4 中调用模式, ES5 引入了 bind, 所以 函数有 5 中调用模式 )

- 函数模式: this 表示全局对象( 浏览器中是window, 在 node 中是 global )
```js
  // 1. 函数模式
  // 特点: 独立的运行, 调用语法格式前面没有任何引导数据
  function foo () {
    console.log( '函数模式: ', this );
  }
  foo()
  // 就是前面 不能 有 '通过 xxx 得到 函数名' 这个行为
```
- 构造器( constructor )模式: this 表示刚刚创建出来的对象
```js
   // 2. 构造器模式
    // 我们需要知道构造器的执行过程
    // 语法: new 构造函数()
    function Person() {}
    var p = new Perosn();
    // 
    // 1. 先使用 new 运算符 分配内存空间, 在 js 中就是创建对象 ( 一个空的对象, 一个具有原型结构的对象 )
    //  - 空对象表示没有 自己 的 任何 成员
    //  - 具有原型结构, 该对象的原型 ( __prop__ ) 是 Person.prototype
    // 2. 调用构造函数
    //  - 创建活动文对象
    //  - this 创建的对象的引用会作为 活动 对象 中的上下文对象被引用
    //  - 预解析
    //  - 解释执行
    //  - ...
```
- 方法 ( method ) 模式: this 表示引导方法调用的对象
```js
    // 一个函数作为对象的一个成员, 由对象引导调用, 这个调用就是方法调用
    // 表现就是 调用前有一个引导数据: 满足 通过 xxx 访问到方法名 调用
    var o1 = {
      sayHello: function () { console.log( this ) } 
    }
    o1.sayHello(); // 方法调用: sayHello 是通过 o1 引导得到的

     function foo() {
      console.log( '这是什么调用呢? ', this )
    }
    var o2 = { name: 'o2' };
    
    o2.func = foo;

    o2.func(); // 方法调用. 因为 是 通过 o2 引导得到的 函数 再调用. 

    o2.func 与 foo 是什么关系? 它们是完全相同的一个东西
    o2.func(); // 方法
    foo();     // 函数

    // 测试 1
    var arr = [];
    arr.push( foo );

    var fn = arr[ 0 ];

    fn(); // 是什么调用? 函数 
    arr[ 0 ](); // 是什么调用? 方法

    var o = {
      name: '对象',
      foo: function () {

        console.log( '什么调用呢? ', this );
      }
    };  
    var f;
    ( f = o.foo )(); // 面试题

    // o.foo();
    // var fn = o.foo;
    // fn();
    // 词法分析, 运行原理
    // 晦涩:
    // 赋值运算, 是分为两步操作
    // 1. 将 = 右边 的值取出( 求出 ), 取出东西 与 原本的 东西并不相同
    // 2. 将 取出的数据 存储到 = 左边的 变量表示容器中

    // 赋值表达式的值就是取出的那个值

    // 所以此时是在调用取出的 那个 东西

```
- 上下文 ( context ) 模式: this 可以使用参数来动态的描述 ( 动态绑定 )
  - 在调用函数的时候, 传入对象作为 this, 每次调用可以传入不同 对象作为 this
```js
    function foo() {
      console.log( '调用' );
    }
    foo();            // 函数调用, 正常调用
    foo.call();   // call 调用
    foo.apply();  // apply 调用

    // 函数在执行的时候回创建活动对象, 活动对象中会存在一个上下文 就是 函数中的 this
    // call 和 apply 方法的第一个参数就是决定上下文的东西
    // 1. 你传入什么 上下文 就是什么 ( this 就是什么 )
    // 2. 如果出入的是 引用类型, 除了 null 外就是表示 this
    // 3. 如果传入的是基本类型 ( 数字, 布尔, 字符串 ), 它们会转换成包装类型
    // 4. 如果传入的是 空 ( null, undefined ), 那么上下文就是全局对象

    function foo() {

      console.log( '调用', this );
    }
    // foo.call( { name: '对象1' } );   // call 调用
    // foo.apply( { name: '对象2' } );  // apply 调用

    // 如果不传任何参数, 其实就表示传入的 空
    // foo.call();   // call 调用
    // foo.apply();  // apply 调用

    // foo.call( 123 );   // call 调用
    // foo.apply( true ); // apply 调用

    // foo.call( '123' );

    // 如果 call 与 apply 不传入任何参数, 那么就相当于函数调用
    // 如果 call 和 apply 只传入一个参数, 那么久相当于方法调用
    // 我们的 call 与 apply 除了第一个参数以外, 其他的所有参数都是与 源函数 参数相对应的, 只是形式不同
    
    // 我们的 call 与 apply 除了第一个参数以外, 其他的所有参数都是与 源函数 参数相对应的, 只是形式不同  
    
    function foo( num1, num2 ) {
      console.log( `num1 = ${num1}, num2 = ${num2}, this: `, this )
    }
    // 下面三个调用等价
    // foo( 1, 2 );
    // foo.call( null, 1, 2 );
    // foo.apply( null, [ 1, 2 ] );
    foo.call( null, 123, 456 ); // call 从 第二个参数开始, 描述的就是正常函数调用时需要提供的参数
    foo.apply( { name: 'apply 调用' }, [ 1, 2 ] ); // apply 的第二个参数是数组的形式, 用数组的形式描述正常函数的参数
```
- bind 模式: this 与 上下文模式类似, 也是通过参数来确定 ( 静态绑定 )
  - bind 一开启就绑定, 其后在使用的时候就不需要绑定, 每次使用的时候都是一开始绑定的那一个 this
  - 语法:
    - 函数.bind( 对象 ) // 返回已绑定 this 的新函数
```js
  var $ = document.querySelectorAll.bind(document)
  $('a')
```

## 4. this丢失问题
- 其实多数情况下，是不会发生this绑定丢失的，只有一种情况下会丢失，函数没有执行，当做值传递了。不管是赋值操作，还是当做回调函数的参数传递。
```js
//demo1
function foo() {
    console.log( this.a );
}
 
var obj = {
    a: 2,
    foo: foo
};
 
var bar = obj.foo; // function reference/alias!
 
var a = "oops, global"; // `a` also property on global object
 
bar(); // "oops, global"
// 很容易看到，var bar = obj.foo; obj.foo并没有执行，而是直接赋值给了bar，所以在bar调用时，不存在任何上下文执行环境，就应用了默认绑定，非严格模式下，this绑定到window，而严格模式下，绑定到undefined。


//demo2
function foo() {
    console.log( this.a );
}
 
function doFoo(fn) {
    // `fn` is just another reference to `foo`
 
    fn(); // <-- call-site!
}
 
var obj = {
    a: 2,
    foo: foo
};
 
var a = "oops, global"; // `a` also property on global object
 
doFoo( obj.foo ); // "oops, global"
// 　　在执行doFoo()函数时，obj.foo是当做参数传递的，并没有发生函数执行的过程，向上查找，obj.foo 其实就等于 foo，所以在doFoo内部调用的时候，依然是默认绑定规则。
```
上下文语法
1. 函数名.call( 上下文, 参数1, 参数2, ... )     可以有任意个参数
2. 函数名.apply( 上下文, [ 参数, ... ] )        最多两个参数

问题:

1. 有几个参数?
2. 调用的意译: 无论是 函数的正常调用, 还是 call 调用, 还是 apply 调用其实都是在调用函数



技巧: 上下文调用的技巧主要是借用和展开

1. 上下文调用有一个特征可以随意的修改的 this 的含义
  - 数组的借用方法
  - slice( startIndex, length )
  - 可以实现将伪数组转换为真数组( ES3 时代的用法 )
    - arr.slice( 0, arr.length ) 什么意思? 拷贝数组 ( 浅拷贝 )
    - 伪数组.slice( 0, 伪数组的长度 ) 
    - 伪数组没有这个方法, 可以借用
    - Array.prototype.slice.call( 伪数组 )  
    - Array.prototype.slice.apply( 伪数组 )  

  - 在页面中将歌曲数据找出来
    - title="你要找的名字"
    - 找到该元素的 父元素 的子元素 ( class="rank-list-box" )
    - rank-list-box 里面有 a 标签, title 是歌曲名字, a 里面还有 img 是图片

    function getMusitInfo( typeName ) {
      var songsContainer = Array.prototype.slice.call( document.querySelectorAll( 'a' ) )
        .filter( a => a.title == '新歌榜' && a.className.indexOf( 'rank-title' ) > -1 )

      var list = Array.prototype.slice.call( document.querySelectorAll( 'a' ) ).filter( a => a.title == '新歌榜' && a.className.indexOf( 'rank-title' ) > -1 )[ 0 ].parentNode.parentNode.children;

      Array.prototype.call( list ).filter( div => div.className.indexOf( 'rank-list-box' ) > -1 ) 
    }


    var $ = document.querySelectorAll()


箭头函数

1. 箭头函数它不改变 this

面试题


```js
var length = 10;
function fn() {
  console.log( this.length );
}

var obj = {
  length: 5,
  method: function ( fn ) {
    fn();
    arguments[ 0 ]();
  }
};
obj.method( fn, 1, 2, 3 );
```

## 4. 箭头函数


## 5. 面试题

```js
var length = 10;
function fn() {
  console.log( this.length );
}

var obj = {
  length: 5,
  method: function ( fn ) {
    fn();
    arguments[ 0 ]();
  }
};
obj.method( fn, 1, 2, 3 );
```


   