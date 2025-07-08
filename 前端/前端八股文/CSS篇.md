# 理论篇
## 什么是BFC？
### 解释
BFC是 **块级格式化上下文**，是CSS渲染的的一种机制，是一块独立渲染的区域，定义了盒子内部元素的排列方式，盒子内部元素与外部元素相互不影响
 - 内部的盒子在垂直方向上一个接一个放置
 - 同一个BFC的两个相邻的盒子的margin会发生重叠，与方向无关
 - BFC不会和float元素区域重叠
 - 计算BFC的高度时，浮动子元素也参与计算
 目的是为了形成相对于外界完全独立的空间，让内部元素不会影响外部元素

### 触发条件
 - 根元素
 - 浮动元素
 - overflow不为visible
 - display的值为inline-block，grid，flex，等
 - position的值为absolute或fixed

### 应用场景
 - 清除内部浮动
 - 包裹浮动
 - 解决高度坍塌

## 说一说盒模型？
  - CSS盒模型定义了盒的每个部分包含 margin, border, padding, content 。
  - 根据盒子大小的计算方式不同盒模型分成了两种，**标准盒模型**和**怪异盒模型**。 
	  - **标准模型**，给盒设置 `width` 和 `height`，实际设置的是 content box。`padding` 和 `border `再加上设置的宽高一起决定整个盒子的大小。 
	  - **怪异盒模型**，给盒设置 `width` 和 `height`，包含了`padding`和`border `，设置的 `width` 和 `height`就是盒子实际的大小 
  - **默认**情况下，盒模型都是**标准盒模型** 
  - 设置标准盒模型：`box-sizing:content-box` 
  - 设置怪异盒模型：`box-sizing:border-box`

## 说一下浮动？
1. 浮动的作用：常用于图片，可以实现文字环绕图片。
2. 浮动的特点：脱离文档流，容易造成盒子塌陷，影响其他元素的排列。
3. 解决塌陷问题：流行用法：父元素中添加overflow:hidden、给父元素添加高度、建立空白标签、添加clear、或者在父级添加伪元素::after{content:'',clear:both,display:table}

## 说几个未知宽高元素水平垂直居中方法
## CSS3的新特性

 
---
# 手撕篇
## CSS水平垂直居中
## CSS画三角形
## CSS实现两栏和三栏布局