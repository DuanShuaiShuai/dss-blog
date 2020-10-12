---
title: new 操作符做了什么
---
## new操作符做了什么
```js
var Func=function(){};
var func=new Func ();
```
- 四件事
    - 创建一个对象
    ```js
    var obj = new Object();
    ```
    - 链接到原型
    ```js
    obj.__proto__= Func.prototype;
    ```
    - 绑定this指向，执行构造函数
    ```js
    var result =Func.call(obj);
    ```
    - 确保返回的是对象

- white-space是什么?
在做许多文章的静态页面,或者说依靠textarea 文本域实现文章编辑上传  最后显示在页面html 标签中渲染显示,这时候要实现一些缩进, 换行 空格等等效果  都需要依赖到  white-space 这个css属性
- white-space的用法与效果
  - normal：忽略掉文本中多余的空格和回车符。默认属性
  - nowrap：忽略掉文本中多余的空格和回车符，并且文本在一行中显示。
  - pre：不忽略文本中多余的空格和回车符。但是文本只在回车符处进行换行，如果没有回车符，文本将会在一行中显示。
  - pre-wrap：不忽略空格和回车符，同时文本自动允许换行。
  - pre-line：忽略多余的空格，但不忽略回车符。
  - initial：继承父级属性。
