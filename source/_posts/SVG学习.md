---
title: SVG学习
date: 2019-10-07 16:07:40
tags: SVG
---
# 简介
SVG官网学习笔记，主要参考W3C中的SVG教程。
SVG意为可缩放矢量图形，使用XML格式定义图像。
## 什么是SVG
* SVG指可伸缩矢量图形
* SVG用来定义用于网络的基于矢量的图形。
* SVG使用XML格式定义图形。
* SVG图像在放大或改变尺寸的情况下，其图形质量不会有所损失。
* SVG是万维网联盟的标准
* SVG与诸如DOM和XSL之类的W3C标准是一个整体。
## SVG优势
* SVG可被非常多的工具读取和修改。
* SVG与JPEG和GIF图像比起来，尺寸更小，且压缩性更强。
* SVG是可伸缩的。
* SVG图像可在任何的分辨率下被高质量的打印。
* SVG图像的文本是可选的，同时也是可搜索的（很适合制作地图）。
* SVG可以与java技术一起运行。
* SVG是开放的标准。
* SVG文件是纯粹的XML。
# SVG形状
![uR8Ac8.png](https://s2.ax1x.com/2019/10/07/uR8Ac8.png)

示例：
```javascript
<svg width="1000" height="1000">
    <rect x="20" y="20" width="300" height="100" style="fill:rgb(0,0,255);stroke-width:10;
    stroke:pink;fill-opacity: 0.1;stroke-opacity: 0.9" />
        <circle cx="200" cy="100" r="40" stroke="black" stroke-width="2" fill="red" />
        <ellipse cx="240" cy="100" rx="220" ry="30" fill="purple" stroke="rgb(0,0,100)" stroke-width="2" />
        <ellipse cx="220" cy="70" rx="190" ry="20" style="fill:lime" />
        <ellipse cx="210" cy="45" rx="170" ry="15" style="fill:yellow" />


        <ellipse cx="240" cy="100" rx="220" ry="30" style="fill:yellow" />
        <ellipse cx="220" cy="100" rx="190" ry="20" style="fill:white" />


        <line x1="0" y1="0" x2="300" y2="300" style="stroke:red;stroke-width:2" />
        <polygon points="220,100 300,210 170,250" fill="#ccc" stroke="#000" stroke-width="1" /> 

        <polyline points="0,0 0,20 20,20 20,40 40,40 40,60" style="fill:white;stroke:red;stroke-width:2" />
        <path d="M250 150 L150 350 L350 350 Z" />
</svg>

```

# SVG滤镜
SVG 滤镜用来向形状和文本添加特殊的效果。
在 SVG 中，可用的滤镜有：

* feBlend
* feColorMatrix
* feComponentTransfer
* feComposite
* feConvolveMatrix
* feDiffuseLighting
* feDisplacementMap
* feFlood
* feGaussianBlur
* feImage
* feMerge
* feMorphology
* feOffset
* feSpecularLighting
* feTile
* feTurbulence
* feDistantLight
* fePointLight
* feSpotLight
注释：您可以在每个 SVG 元素上使用多个滤镜！

```javascript
<defs>
    <filter id="Gaussian_Blur">
         <feGaussianBlur in="SourceGraphic" stdDeviation="20"/>
     </filter>
</defs>
<ellipse cx="200" cy="150" rx="70" ry="40" style="fill:#ff0000;stroke:#000000;stroke-width:2;filter:url(#Gaussian_Blur)"/>
```

# SVG渐变
SVG 渐变必须在 <defs> 标签中进行定义。
在 SVG 中，有两种主要的渐变类型：
线性渐变
放射性渐变
## 线性渐变
```<linearGradient>``` 可用来定义 SVG 的线性渐变。

```<linearGradient>``` 标签必须嵌套在 ```<defs>``` 的内部。
线性渐变可被定义为水平、垂直或角形的渐变：

* 当 y1 和 y2 相等，而 x1 和 x2 不同时，可创建水平渐变
* 当 x1 和 x2 相等，而 y1 和 y2 不同时，可创建垂直渐变
* 当 x1 和 x2 不同，且 y1 和 y2 不同时，可创建角形渐变

```javascript
<defs>
    <linearGradient id="orange_red" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" style="stop-color:rgb(255,255,0);stop-opacity:1"/>
        <stop offset="100%" style="stop-color:rgb(255,0,0);stop-opacity:1"/>
    </linearGradient>
</defs>
<ellipse cx="200" cy="190" rx="85" ry="55" style="fill:url(#orange_red)"/>
```

* ```<linearGradient>``` 标签的 id 属性可为渐变定义一个唯一的名称
*  ```fill:url(#orange_red) ```属性把 ellipse 元素链接到此渐变
* ```<linearGradient>``` 标签的 x1、x2、y1、y2 属性可定义渐变的开始和结束位置
渐变的颜色范围可由两种或多种颜色组成。每种颜色通过一个 ```<stop>``` 标签来规定。offset 属性用来定义渐变的开始和结束位置。

## 放射性渐变
```<radialGradient>``` 用来定义放射性渐变。
```<radialGradient>``` 标签必须嵌套在 ```<defs> ```中。
```javascript
<defs>
    <radialGradient id="grey_blue" cx="50%" cy="50%" r="50%" fx="50%" fy="50%">
        <stop offset="0%" style="stop-color:rgb(200,200,200);stop-opacity:0"/>
        <stop offset="100%" style="stop-color:rgb(0,0,255);stop-opacity:1"/>
    </radialGradient>
</defs>

<ellipse cx="230" cy="200" rx="110" ry="100"
style="fill:url(#grey_blue)"/>
````

* ```<radialGradient>``` 标签的 id 属性可为渐变定义一个唯一的名称
* ```fill:url(#grey_blue) ```属性把 ellipse 元素链接到此渐变
* ```cx、cy ```和 ```r ```属性定义外圈，而 fx 和 fy 定义内圈 渐变的颜色范围可由两种或多种颜色组成
* 每种颜色通过一个 ```<stop>``` 标签来规定。offset 属性用来定义渐变的开始和结束位置。