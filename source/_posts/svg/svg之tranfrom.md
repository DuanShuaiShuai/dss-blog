---
title: flex布局巩固
---

# scg与htmo元素的一些区别

- transform等转换的时候的参照点不一样
    - HTML元素的坐标原点在自身50% 50%处
    - SVG元素的坐标原点在SVG画布0 0
```html
<!DOCTYPE html>
<html lang="en">
<body>
<svg width="40" height="50" style="background-color:#bff;">
  <rect x="0" y="0" width="10" height="10" transform="rotate(45)" />
</svg>
<div class="box">
  <div class="box-son"></div>
</div>
  <style type="text/css">
  .box{
    border: 1px solid #ccc;
    width: 40px;
    height: 40px;
  }
  .box-son {
    width: 10px;
    height: 10px;
      background-color: red;
      transform: rotate(45deg);
    }
  </style>
</body>
</html>
```
- svg与html的样式类属性是一样的,但是行内属性有所区别
    - 在IE浏览器下包含transform的类样式对SVG元素没有作用,建议书写行内属性
    - 行内属性不需要写单位
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
<svg width="400" height="400" style="background-color:#bff;">
  <rect width='150' height='80' transform='translate(295 115)' />
</svg>
<svg width="400" height="400" style="background-color:#bff;">
  <rect width='150' height='80' class="rec" />
</svg>
<div class="box">
  <div class="box-son"></div>
</div>
  <style type="text/css">
    .rec { /* doesn't work in IE */ transform: translate(295px, 115px); }
    .box{
      background-color: #bff;
      width: 400px;
      height: 400px;
    }
    .box-son {
      width: 150px;
      height: 80px;
        background-color: red;
        transform: translate(295px, 115px);
      }
  </style>
</body>
</html>
```

- transform的rotate
    - 以元素为中心旋转45°C  
        - CSS transform中的2D旋转函数就是rotate(angle)。angle可以是多种单位的值： degrees，radians，turn,grad,我们也可以使用calc()(比如calc(.25turn - 30deg) )
        - 在SVG的transform属性中的旋转函数是这样的：rotate(angle[ x y])，angle的值与CSS transform属性中的一样(必须是无单位的degree值，正值表示顺时针旋转，负值反之，可选的x y表示旋转时固定点的位置，其值默认是该元素坐标系的原点)，如果只有x，则变换无效。
        - 我们可以在CSS transform中指定transform-origin属性来模拟SVG中的x y参数，长度单位是相对于元素坐标系而言的，百分比单位则是以元素自身为基准著作权归作者所有。
        - 以下两种是等价的
        ```html
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
        <svg width='300' height='300'>
            <rect x='100' y='100' width='100' height='100' transform='rotate(45 150 150)' />
        </svg>
        <svg width='300' height='300'>
            <rect class="origin" x='100' y='100' width='100' height='100' transform='rotate(45)' />
        </svg>
        <style type="text/css">
            .origin{
            /* transform: rotate(45deg);  IE不支持 */
            transform-origin:50% 50%;
            }
        </style>
        </body>
        </html>
        ```
    - 为了加深理解，我们再应用一个相同角度45°，相反方向的roatate()

        ```html
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Document</title>
        </head>
        <body>
        <svg width='300' height='300'>
            <rect x='100' y='100' width='100' height='100' transform='rotate(45 150 150) rotate(-45)' />
        </svg>
        <svg width='300' height='300'>
            <rect class="origin" x='100' y='100' width='100' height='100' transform='rotate(45) rotate(-45)' />
        </svg>
        <style type="text/css">
            .origin{
            /* transform: rotate(45deg);  IE不支持 */
            transform-origin:50% 50%;
            }
        </style>
        </body>
        </html>
        ```
- transform的scale
  - 在CSS transform中有3种2D的缩放函数： scale(sx[, sy])、 scaleX(sx)、scaleY(sy)。第一个函数，会同时在x和y方向上应用sx和sy缩放因子，sy是可选的，如果该函数只有一个参数，会默认是sx的值。其它两个函数是分别在两个方向进行缩放，scaleX(sx)、 scaleY(sy)分别等价于scale(sx,1) 、scale(1,sy)，如果在此之前存在其它的变换，那么相应的x和y方向将不再是水平和垂直的。
  - 在SVG transform中，只有scale(sx[ sy])。同样，可以使用空格或者逗号分隔参数
  ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
      <svg width="400" height="400" style="background-color:#bff;">
        <rect width='150' height='80'/>
      </svg>
    <svg width="400" height="400" style="background-color:#bff;">
      <rect width='150' height='80'  transform='scale(2 1.5)'/>
    </svg>
    <svg width="400" height="400" style="background-color:#bff;">
      <rect width='150' height='80' class="rec" />
    </svg>
    <div class="box">
      <div class="box-son"></div>
    </div>
      <style type="text/css">
        .rec {  transform: scale(2, 1.5); }
        .box{
          background-color: #bff;
          width: 400px;
          height: 400px;
        }
        .box-son {
          width: 150px;
          height: 80px;
            background-color: red;
            transform: scale(2, 1.5);
          }
      </style>
    </body>
    </html>
  ```
  - transform的skewX 
    - HTML元素在其50% 50%处，SVG元素在SVG画布的0 0处。著作权归作者所有。
    ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Document</title>
      </head>
      <body>
      <svg width="400" height="400"  style="background-color:#bff;">
        <rect width='150' height='80' transform='skewX(45)'  />
      </svg>
      <svg width="400" height="400" style="background-color:#bff;">
        <rect width='150' height='80' class="rec" />
      </svg>
      <div class="box">
        <div class="box-son"></div>
      </div>
        <style type="text/css">
          .rec { /* doesn't work in IE */   transform: skewX(45deg); }
          .box{
            background-color: #bff;
            width: 400px;
            height: 400px;
          }
          .box-son {
            width: 150px;
            height: 80px;
              background-color: red;
              transform: skewX(45deg);
            }
        </style>
      </body>
      </html>
    ```
- viewBox 改变画布的显示区域
  - 见附件
  ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
      <svg style="width:150px; height:300px">
        <circle cx="200" cy="200" r="200" fill="#fdd" stroke="none"></circle>
    </svg>
    <svg style="width:150px; height:300px" viewBox="0 0 400 400">
      <circle cx="200" cy="200" r="200" fill="#fdd" stroke="none"></circle>
    </svg>
      <style type="text/css">
      </style>
    </body>
    </html>
  ```

- 附件
  - [SVG之ViewBox](https://segmentfault.com/a/1190000009226427?utm_source=tag-newest)
  - [SVG元素上的transform](https://www.w3cplus.com/svg/transforms-on-svg-elements.html)