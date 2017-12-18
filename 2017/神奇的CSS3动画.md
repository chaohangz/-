# 神奇的CSS3动画
在写微信上的活动页面时，因为只需要适配微信内置的浏览器，所以可以很自由的使用各种HTML5、CSS3的新特性，
要是微信支持的就行。在这期间我最喜欢的就是CSS3的animation，与用js写的动画相比，animation更加简单直观，
而且也不失灵活。

## animation and @keyframes
animation要与@keyframes搭配使用。在animation中我们引用@keyframes动画的名称，并规定动画的速度，循环等
属性，在@keyframes中，我们为一个动画命名，并定义它每个关键帧的状态。来看下面的例子：

```css
div {
	/*把 myAnimation 动画捆绑到div元素，时长2s*/
	animation: myAnimation 2s;
}

/*定义一个简单的动画*/
/*该动画包含两个关键帧，起始和结束*/
/*动画会在规定的时间内(2s)逐渐的改变*/
@keyframes myAnimation
{
from {background: red}
to {background: green}
}
```

该动画只是简单的实例，要想匹配更多的浏览器，需要加上其他前缀，可以点击下面的链接观看实例。

[第一个简单的动画](https://chaohangz.github.io)

[更多属性请查看w3shool](http://www.w3school.com.cn/css3/css3_animation.asp)

单单改变背景颜色肯定不能满足我们的需求，我们要的是“动画”，当然是要动起来才行。为此CSS3为我们提供了
 2D 转换和 3D 转换。

### 2D transform

`translate()` 从当前位置，移动到指定坐标(x, y)

```css
div {
	transform: translate(50px, 80px);
}
```

`rotate()` 顺时针旋转指定角度。如果是负值，就逆时针旋转

```css
div1 {
	transform: rotate(30deg);
}

div2 {
	transform: rotate(-30deg);
}
```

`scale()` 元素的尺寸会增加或减少一定的倍数，根据 (x, y) 的参数

```css
div {
	transform: scale(2, 0.8);
	/*把宽度变为原始尺寸的2倍，高度变为原始尺寸的0.8倍*/
}
```

`skew()` 围绕X轴和Y轴翻转给定的角度

```css
div {
	transform: skew(30deg, 20deg)
	/*围绕X轴翻转30度，围绕Y轴翻转20度*/
}
```

`matrix()` 把所有2D转换方法组合在一起，需要六个参数。

以上就是所有的2D转换方法，我们做一个小的总结：

`translate(x,y)`  从当前位置移动到(x, y)位置

`rotate(xdeg)`  循转给定角度

`scale(x, y)`  尺寸的宽(x)和高(y)增加或减少给定的倍数

`skew(xdeg, ydeg)`  在X轴和Y轴翻转给定的角度

`matrix(n,n,n,n,n,n)`  把所有的方法组合在一起

[更详细实例请访问w3shool](http://www.w3school.com.cn/css3/css3_2dtransform.asp)

### 3D transform
3D 转换这里只介绍两个方法，`rotateX()` `rotateY()` 围绕X轴Y轴旋转给定的角度。

与2D转换中的rotate不同的是，2D的rotate在平面中旋转，3D的rotate在空间中旋转。

[w3shool里有一个很直观的比较](http://www.w3school.com.cn/css3/css3_3dtransform.asp)

### 组合
如果我们把这些属性组合在一起，将会组成各种好玩的动画。以下是两个简单的示例，希望能给大家一点启发。

这是一个移动端摇一摇礼盒落下的例子，在PC端看起来可能会有点夸张，建议用控制台调到移动端观看。

[礼盒掉落]()




