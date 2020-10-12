---
title: Promise 做了什么
---
## 1. Promise 与回调地狱

参考:

- https://promisesaplus.com/
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects


在小程序中要获得用户的信息

wx.login( {

  success ( code ) {
    
    fetch( '我们的服务器', code, function ( session_key ) {

      fetch( '我们的服务器', {
        加密数据,
        session_key
      }, function ( 用户信息 ) {

        更新页面

      } );

    } );

  }
} );



例如 login

function myLogin ( resolve ) {
  return new Promise( resolve => {
    wx.login( {

      success( res ) {

        resolve( res )

      }

    } )
  } )
}


myLogin( code => 发起获得 session_key 的请求 )
  .then( session_key => 发起解析用户信息的请求 )
  .then( userinfo => 更新页面中信息 )



async clickHandler() {
  let code = await myLogin(  );
  let session_key = await 发起 session ...
  let userinfo = await ...

  更新
}
## 2. 手写 Promise
```js
// 封装  MyPromise

// 到这里其实离真正的 Promise 还有很长很长的距离
console.log( '改良版的 MyPromise 2' )

function MyPromise( resolver ) {

  this.__internal_array__ = [];

  
  runResolver( this, resolver );

}
function runResolver( _this, resolver ) {
  resolver( function resolve( value ) {
   
    doResolver( _this, value );

  } )
}
function doResolver( _this, value ) {

  if ( value instanceof MyPromise ) {
    // 递归执行
    runResolver( _this, value.then.bind( value ) );
  } else {
    _this.__internal_array__.forEach( item => {
      item.resolveHandler( value );
    } );
  }
}
MyPromise.prototype.then = function ( fulfilled ) {
  let newpromise = new MyPromise( () => {} );
  this.__internal_array__.push( { 
    promise: newpromise,
    resolveHandler: function ( value ) {
      let _promise = fulfilled( value );
      // 用户可能返回 promise
      doResolver( newpromise, _promise instanceof MyPromise ? _promise : value );
    },
  } );
  return newpromise;
}
// 1. then 应该返回新的 Promise
// 2. then 中的函数应是支持 返回 Promise



//模拟调用
function get( url ) {
    return new MyPromise( resolve => {
    let xhr = new XMLHttpRequest();
    xhr.open( 'GET', url );
    xhr.onreadystatechange = function () {
        if ( this.readyState === 4 && this.status === 200 ) {
        resolve( JSON.parse( this.responseText ) );
        }
    };
    xhr.send();
    } )
}
document.querySelector( 'button' ).onclick = function () {
    get( 'http://127.0.0.1:3000/api_1' )
    .then( json => {
            console.log( json ); 

            return get( 'http://127.0.0.1:3000/api_2' );
    } )
    .then( json => console.log( '可以了吗? ', json ) )
    .then( res => console.log( 'res: ', res ) )
    .then( () => console.log( 'finished' ) );
}
```

