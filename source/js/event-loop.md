---
title: 事件循环
---  
### 1. 规则
  1. 线性执行
  2. 函数调用栈
  3. 事件队列

事件队列就是一个函数队列 ( 函数数组 )
每次执行 **耗时的操作** ( 其他进程处理 ) 都会提供一个回调函数, 交给其他进程
当其他进程实现完耗时操作以后 回调函数 连同处理的结果 作为参数会投递到 函数队列 中

js 引擎, 从队列中取一个函数执行,
执行完成以后 再 从队列中取一个函数执行,
直到队列为空, 那么就停止
然后如果又有人将事件投递到队列中, js 引擎又会运行起来.

通俗的比喻 js 引擎就好比一个公司的老板
老板要做事情不是自己做, 将任务分配给人员, 将这个任务会记录到工作日志中
如果有一个人把事情做完了, 走到老板身边, 如果老板现在正在处理一些事情, 怎么办?
等待, 事件的结果交给老板的秘书
如果又有一个人完成, 交给秘书( 排序, 汇报 )

在 js 中它的事件队列非常复杂 ( V8 )
 - timer 队列
 - pending poll 队列( 不是 **一个** 队列, 这专门处理 IO, 很多队列, 只是我们不需要考虑 )
 - check 队列
 - close 队列

内部的逻辑 ( 伪代码 )
```js
var queue = [
    [],           // timer 队列, 专门用于存放 timeout 回调 ( web, fullstack, gost )
    [],           // 综合队列, 可以**简单**的理解为 IO 队列 ( gost )
    [],           // check 队列, 专门有从来存放 setImmediate 回调 ( web, fullstack, gost )
    []            // close 队列, 专门用来存放 close 行为 ( fullstack, gost )
]

// js 引擎 处理队列
  var i = 0;
  var len = queue.length;

// 引擎内部的处理流程

  while ( true ) {      // 除非所有的队列清空, 否则一直执行下去
                        // 并且如果处于停止状态, 一旦有事件入队, 立即激活该循环
    while( i < len ) {
      var subqueue = queue[ i ];

      // 处理 子队列, 处理完该子队列

      i++
    }  
    i = 0;
  }
```

### 3.案例
```js
// 1
console.log( 'main process 1' );
console.log( 'main process 2' );
console.log( 'main process 3' );
console.log( 'main process 4' );
console.log( 'main process 5' );
console.log( 'main process 6' );
// 代码不存在任何异步行为, 代码会从上往下依次执行

// 1
setTimeout( function () {
  console.log( 'main process timeout 1' );
}, 1000 );

console.log( 'main process 1' );
console.log( 'main process 2' );
console.log( 'main process 3' );
console.log( 'main process 4' );
console.log( 'main process 5' );
console.log( 'main process 6' );
// 分析
// 1. 执行 `setTimeout`, 这个执行是同步的, 会将 回调函数, 放入事件队列的 Timer 队列中, 现在不会执行, 并记录执行时间
// 2. 进入 console.log 系列, 此时就是先行执行, 这里执行完成以后, 启用事件队列中的代码
// 3. 进入事件队列后, 优先开始运行 timer 队列, 发现只有里面存在一个函数, 将其取出执行
// 4. 此时需要注意 timer 事件队列中的函数存在一个 时间的范围, 加入现在已过去 900ms, 不好意思, 
//    此时不会执行, 还会继续遍历 pending, roll, check, close ... 队列,
//    等到这些队列遍历完, 又会回调 timer 对象, 如果此时发现时间已过去 990ms, 不好意思, 
//    此时还不会执行, 还会继续遍历 pending, roll, check, close ... 队列, 
//    等到这些队列遍历完, 又会回调 timer 对象, 如果此时发现时间已过去 1000ms, 才会执行. 


// 3
setTimeout( function () {
  console.log( 'main process timeout 1' );
}, 0 );

console.log( 'main process 1' );
console.log( 'main process 2' );
console.log( 'main process 3' );
console.log( 'main process 4' );

setTimeout( function () {
  console.log( 'main process timeout 2' );
}, 0 );

console.log( 'main process 5' );
console.log( 'main process 6' );

setTimeout( function () {
  console.log( 'main process timeout 3' );
}, 0 ); // 此时 对中有 3 个函数




// 4

setTimeout( function () {
  console.log( 'main process timeout 1' );
}, 500 );
// 给一个 500ms 的延时
console.log( '延时 500 ms' );
let curr = +new Date();
while( (+new Date) - curr < 500 );
console.log( 'main process 1' );
console.log( 'main process 2' );

setTimeout( function () {
  console.log( 'main process timeout 2' );
}, 0 );

console.log( 'main process 3' );
console.log( 'main process 4' );

setTimeout( function () {
  console.log( 'main process timeout 3' );
}, 0 ); // 此时 对中有 3 个函数
// 时间很微妙

// 5

setTimeout( function () {
  console.log( 'main process timeout 1' );
}, 0 );

console.log( 'main process 1' );
console.log( 'main process 2' );

console.log( 'main process 3' );
console.log( 'main process 4' );

setImmediate( function () {
  console.log( 'main process immediate 1' );
} )


// 6

setImmediate( function () {
  console.log( 'main process immediate 1' );
} )

console.log( 'main process 1' );
console.log( 'main process 2' );

console.log( 'main process 3' );
console.log( 'main process 4' );


setTimeout( function () {
  console.log( 'main process timeout 1' );
}, 3 );


// 启用队列, 优先执行 timer

// 如果进入 timer 队列的时候 还不足 5 毫秒, 那么 就会进入下一个循环队列

// 需求, 因为不知道谁会先执行, 如果我就希望先执行 timeout 的事件处理, 后处理 immediate 
// 要实现, 只需要将代码 放到 immediate 逻辑中即可

setImmediate( () => {
  // 若要执行这段代码, 必然是在 check 事件队列中, 此时, 接着进入 timer 对象
  setImmediate( function () {
    console.log( 'main process immediate 1' );
  } )

  setTimeout( function () {
    console.log( 'main process timeout 1' );
  }, 0 );

} )
// 假如我就要先执行 immediate, 那就只需将代码放在 timeout 中

setTimeout( () => {
  // 若要执行这段代码, 必然是在 timer 事件队列中, 此时, 接着进入 其他的队列中
  setImmediate( function () {
    console.log( 'main process immediate 1' );
  } )

  setTimeout( function () {
    console.log( 'main process timeout 1' );
  }, 0 );

}, 0 );
```


  