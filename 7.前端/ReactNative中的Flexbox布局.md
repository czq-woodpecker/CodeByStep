# React Native中的Flexbox布局

## 一、什么是Flexbox布局

Flexbox布局，简称Flex布局，意为“弹性布局”，用来为盒状模型提供极大的灵活性。在React Native中，一般使用Flex布局规则来指定某个组件的子元素布局，一般而言，使用`flexDirection`、`alignItems`、`justifyContent`这三个属性就能够满足大部分布局需求。

> 注意：React Native中的Flexbox的工作原理和web上的css基本一致，但也存在少许差异。如默认值不同，flexDirection的默认值是column而不是row，而flex也只能指定一个数字值。

## 二、基本概念

`Flex Container`：采用Flex布局的元素，称为Flex容器。

`Flex Item`：Flex容器的所有子元素称为容器成员，简称项目。

//TODO 差个图（整体布局结构）

## 三、容器的属性

### flexDirection

flexDirection属性决定`主轴的方向`。由于react native的flex布局flexDirection属性默认值是**column**（竖直轴），所以子元素默认是沿着竖直轴的方向排列。

```css
.box{
  flexDirection: 'column-reverse' | 'column' | 'row' | 'row-reverse';
}
```

![flexDirection属性图](https://www.runoob.com/wp-content/uploads/2015/07/0cbe5f8268121114e87d0546e53cda6e.png)

* column-reverse: 从下到上排列
* column：从上到下排列
* row：从左到you排列
* row-reverse：从右到左排列

> 其实都是沿着主轴排列，如column-reverse表示主轴方向是列的反方向，列默认是从上到下，所以column-reverse就是将主轴方向设置为从下到上，所以子元素的排列方式是从下到上排列。

### justifyContent

justifyContent可以决定子元素`沿着主轴的排列方式`。

```css
.box{
  justifyContent: 'flex-start' | 'center' | 'flex-end' | 'space-around' | 'space-between' | 'space-evenly'
}
```

//TODO 差个图

* flex-start: 子元素（容器成员、项目）靠近主轴的起始端分布。
* center：项目沿着主轴居中分布。
* flex-end：项目靠近主轴的末尾端分布。
* space-around：每个项目两侧的间隔相等，所以项目之间的间隔比第一个项目与主轴起始端边缘、最后一个项目与主轴末尾端边缘的间隔大一倍。
* space-between：两端对齐，项目之间的间隔都相等。第一个项目与主轴起始端边缘无间距，最后一个项目与主轴末尾端边缘也没有间距。
* space-evenly：子元素沿着主轴均匀地分布在容器内，每对相邻的项目之间的间距、主轴起始端边缘和第一个项目以及最后一个项目与主轴末尾端边缘的间距完成相等。

### alignItems

alignItems可以决定其`子元素沿着次轴（与主轴垂直的轴，若主轴方向为column，则次轴方向为row）的排列方式`。

```css
.box{
  alignItems: 'flex-start' | 'center' | 'flex-end' | 'stretch'
}
```

//TODO 差个图

* flex-start：子元素沿着次轴起始端排列
* center: 子元素沿着次轴居中分布
* flex-end：子元素沿着次轴末尾端排列
* stretch：元素在次轴方向上被拉伸以适应容器（在次轴方向尽量撑满容器）。

> 注意：要使stretch选项生效的话，子元素在次轴方向上不能有固定的尺寸。

> 参考资料
>
> * https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content
> * https://www.runoob.com/w3cnote/flex-grammar.html