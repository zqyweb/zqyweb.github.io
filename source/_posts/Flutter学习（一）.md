---
title: Flutter学习（一）
date: 2020-04-01 15:57:24
tags:
---

# Flutter基础知识学习（一）


# 基础组件
## Text

textScaleFactor：代表文本相对于当前字体大小的缩放因子，相对于去设置文本的样式style属性的fontSize，它是调整字体大小的一个快捷方式。该属性的默认值可以通过MediaQueryData.textScaleFactor获得，如果没有MediaQuery，那么会默认值将为1.0。

fontSize：该属性和Text的textScaleFactor都用于控制字体大小。但是有两个主要区别：

fontSize可以精确指定字体大小，而textScaleFactor只能通过缩放比例来控制。
textScaleFactor主要是用于系统字体大小设置改变时对Flutter应用字体进行全局调整，而fontSize通常用于单个文本，字体大小不会跟随系统字体大小变化。

# 布局类组件
## 线性布局(Row,Column)
Row和Column都继承自Flex。对于线性布局，有主轴和纵轴之分，如果布局是沿水平方向，那么主轴就是指水平方向，而纵轴即垂直方向；如果布局沿垂直方向，那么主轴就是指垂直方向，而纵轴就是水平方向。主轴对齐和纵轴对齐分别是MainAxisAlignment和CrossAxisAlignment。
实际上，Row和Column都只会在主轴方向占用尽可能大的空间，而纵轴的长度则取决于他们最大子元素的长度。
将Column的宽度指定为屏幕宽度；这很简单，我们可以通过ConstrainedBox或SizedBox（我们将在后面章节中专门介绍这两个Widget）来强制更改宽度限制，
```dart
ConstrainedBox(
  constraints: BoxConstraints(minWidth: double.infinity), 
  child: Column(
    crossAxisAlignment: CrossAxisAlignment.center,
    children: <Widget>[
      Text("hi"),
      Text("world"),
    ],
  ),
);

```

如果Row里面嵌套Row，或者Column里面再嵌套Column，那么只有对最外面的Row或Column会占用尽可能大的空间，里面Row或Column所占用的空间为实际大小，
```dart
Container(
  color: Colors.green,
  child: Padding(
    padding: const EdgeInsets.all(16.0),
    child: Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      mainAxisSize: MainAxisSize.max, //有效，外层Colum高度为整个屏幕
      children: <Widget>[
        Container(
          color: Colors.red,
          child: Column(
            mainAxisSize: MainAxisSize.max,//无效，内层Colum高度为实际高度  
            children: <Widget>[
              Text("hello world "),
              Text("I am Jack "),
            ],
          ),
        )
      ],
    ),
  ),
);

```
如果要让里面的Column占满外部Column，可以使用Expanded 组件：

```dart
Expanded( 
  child: Container(
    color: Colors.red,
    child: Column(
      mainAxisAlignment: MainAxisAlignment.center, //垂直方向居中对齐
      children: <Widget>[
        Text("hello world "),
        Text("I am Jack "),
      ],
    ),
  ),
)

```
## 弹性布局(Flex)
弹性布局允许子组件按照一定比例来分配父容器空间。能使用Flex的地方基本上都可以使用Row或Column。Flex本身功能是很强大的，它也可以和Expanded组件配合实现弹性布局。
**Expanded**
可以按比例“扩伸” Row、Column和Flex子组件所占用的空间。
```dart
class FlexLayoutTestRoute extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        //Flex的两个子widget按1：2来占据水平空间  
        Flex(
          direction: Axis.horizontal,
          children: <Widget>[
            Expanded(
              flex: 1,
              child: Container(
                height: 30.0,
                color: Colors.red,
              ),
            ),
            Expanded(
              flex: 2,
              child: Container(
                height: 30.0,
                color: Colors.green,
              ),
            ),
          ],
        ),
        Padding(
          padding: const EdgeInsets.only(top: 20.0),
          child: SizedBox(
            height: 100.0,
            //Flex的三个子widget，在垂直方向按2：1：1来占用100像素的空间  
            child: Flex(
              direction: Axis.vertical,
              children: <Widget>[
                Expanded(
                  flex: 2,
                  child: Container(
                    height: 30.0,
                    color: Colors.red,
                  ),
                ),
                Spacer(
                  flex: 1,
                ),
                Expanded(
                  flex: 1,
                  child: Container(
                    height: 30.0,
                    color: Colors.green,
                  ),
                ),
              ],
            ),
          ),
        ),
      ],
    );
  }
}

```
## 流式布局(Wrap,Flow)
Row默认只有一行，如果超出屏幕不会折行。我们把超出屏幕显示范围会自动折行的布局称为流式布局。Flutter中通过Wrap和Flow来支持流式布局.
```dart
Wrap(
  spacing: 8.0, // 主轴(水平)方向间距
  runSpacing: 4.0, // 纵轴（垂直）方向间距
  alignment: WrapAlignment.center, //沿主轴方向居中
  children: <Widget>[
    new Chip(
      avatar: new CircleAvatar(backgroundColor: Colors.blue, child: Text('A')),
      label: new Text('Hamilton'),
    ),
    new Chip(
      avatar: new CircleAvatar(backgroundColor: Colors.blue, child: Text('M')),
      label: new Text('Lafayette'),
    ),
    new Chip(
      avatar: new CircleAvatar(backgroundColor: Colors.blue, child: Text('H')),
      label: new Text('Mulligan'),
    ),
    new Chip(
      avatar: new CircleAvatar(backgroundColor: Colors.blue, child: Text('J')),
      label: new Text('Laurens'),
    ),
  ],
)

```


## 层叠布局(Stack,Position)

## 对齐与相对定位(Align)


# 可滚动组件
## SingleChildScrollView

在Flutter中，如果内容过多无法展示完全，屏幕的边界会给我们一个OVERFLOWED的警告提示，在Flutter中我们通常使用SingleChildScrollView处理滑动，这里需要注意的是，通常SingleChildScrollView只应在期望的内容不会超过屏幕太多时使用，这是因为SingleChildScrollView不支持基于Sliver的延迟实例化模式，所以如果预计视口可能包含超出屏幕尺寸太多的内容时使用SingleChildScrollView将会导致性能差的问题，此时应该使用一些支持Sliver延迟加载的可滚动组件，如ListView。

**基于Sliver的延迟构建模式：**
通常可滚动组件的子组件可能会非常多，占用的总高度也会非常大，如果要一次性将子组件全部构建出将会导致性能差的问题出现，为此，Flutter中提出一个Sliver（中文为"薄片"的意思）概念，如果一个可滚动组件支持Sliver模型，那么该滚动组件可以将子组件分成好多个薄片（Sliver），只有当Sliver出现在视口时才会去构建它，这种模型也成为"基于Sliver的延迟构建模型"。可滚动组件中有很多都支持基于Sliver的延迟构建模型，如ListView、GridView，但是也有不支持该模型的，如SingleChildScrollView。

```dart
import 'package:flutter/material.dart';

class TextRoute extends StatefulWidget {
  @override
  _TextRouteState createState() => _TextRouteState();
}

class _TextRouteState extends State<TextRoute> {
  @override
  Widget build(BuildContext context) {
    String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    return Scrollbar(
      //显示进度条
      child: SingleChildScrollView(
        ///滑动组件，默认垂直滚动
        padding: EdgeInsets.all(16.0),
        child: Center(
          child: Column(
            //textScaleFactor字体为原来的两倍
            children: str
                .split("")
                .map((c) => Text(
                      c,
                      textScaleFactor: 2.0,
                    ))
                .toList(),
          ),
        ),
      ),
    );
  }
}

```
效果截图：
![GBvsKI.png](https://s1.ax1x.com/2020/04/05/GBvsKI.png)

## ListView
## GridView
## CustomScrollView
## 滚动监听即控制（）
