## 025 Combining Selectors

### list selector 列表选择器

```css
h1,
h2,
h3 {
    font-family: sans-serif;
}
```

### descendant selector 后代选择器

```css
footer p {
    font-size: 16px; 
}
```

## 026 Class and ID Selectors

### id selector

```css
#author {
    font-style: italic; 
}
```

### class selector

```css
.related-author {
    font-size: 18px;       
}
```

## 027 Working With Colors

* 如何用代码表示颜色

  RGB 模型，即任意一种颜色可以用红绿蓝的组合得到。

  速记：#01a 表示 #0011aa

* shades of grey (灰色)

  当所有的3个通道的值一样的时候，我们可以得到灰色。一共有 256 种纯灰色可选。

  灰色：#444、#b7b7b7、#f7f7f7

  蓝色：#1098ad 

* border

  设置边框为 5px，实心，颜色为 #1098ad。

  ```css
  border: 5px solid #1098ad
  ```

  设置上边框为 5px，实心，颜色为 #1098ad。

  ```css
  border-top: 5px solid #1098ad
  ```

  ## 028 Pseudo-classes 

  选择第一个 li

  ```css
  li:first-child 
  ```

  选择最后一个 li

  ```css
  li:last-child
  ```

  选择第 2 个 li

  ```css
  li:nth-child(2)
  /*
  上面的意思是指选择li元素且是其父元素的第二个子元素的元素
  */
  ```
  
  选择奇数位置的 li
  
  ```css
  li:nth-child(odd)
  ```
  
  选择偶数位置的 li
  
  ```css
  li:nth-child(even )
  ```
  
  
  
  ## 029 Styling Hyperlinks
  
  ```css
  a:link {
      color: #1098ad;
      text-decoration: none;
  }
  
  a:visited {
      color: #1098ad;
  }
  
  a: hover {
      color: orangered;
      font-weight: bold;
      text-decoration: underline orangered;
  }
  
  a:active {
      background-color: black;
      font-style: italic;
  }
  /* LVHA */
  ```
  
  ## 031 CSS Theory #1_ Conflicts Between Selectors（选择器之间的冲突）
  
  ![image-20240228084204989](.\assets\image-20240228084204989.png)
  
  
  
  ## 032 CSS Theory #2_ Inheritance and the Universal Selector
  
  并不是所有的属性都会被继承，能被继承的基本上与文字相关，比如说 color、font-size、font-family。
  
  ## 034 CSS Theory #2_ The CSS Box Model
  
  盒子模型定义了元素如何显示在网页以及它们的大小
  
  <img src="./assets/截屏2024-02-28 10.21.47.png" alt="截屏2024-02-28 10.21.47" style="zoom: 25%;" />
  
  * content
  
    文本、图像、等等
  
  * Boarder
  
    元素周围的线，依然包含在元素之内
  
  * Padding
  
    Content 周围不可见的区域，依然包含在元素之内
  
  * Margin
  
    元素之外的空白，位于元素之间
  
  最终元素的宽度 = 左边框+左填充+width+右填充+右边框
  
  最终元素的高度 = 上边框+上填充+width+下填充+下边框
  
  ## 035 Using Margins and Paddings
  
  指定边距
  
  ```css
  /* 同时设置上下左右边距均为 20px */
  .main-header {
    padding: 20px;
  }
  /* 分别指定边距 */
  .main-header {
    padding-top: 20px;
    padding-bottom: 20px;
    padding-left: 40px;
    padding-right: 40px;
  }
  /*
  当只指定一个值时，该值会统一应用到全部四个边的外边距上。
  指定两个值时，第一个值会应用于上边和下边的外边距，第二个值应用于左边和右边。
  指定三个值时，第一个值应用于上边，第二个值应用于右边和左边，第三个则应用于下边的外边距。
  指定四个值时，依次（顺时针方向）作为上边，右边，下边，和左边的外边距。
  */
  
  ```
  
  Collapsing margins（折叠边距）
  
  折叠边距（Collapsing margins）是指在CSS中，相邻的两个元素之间的垂直外边距（margin）可能会合并成一个单独的外边距。这种合并通常发生在垂直方向上，只有上下边距会发生合并，左右边距不会。
  
  折叠边距的规则如下：
  
  1. **相邻兄弟元素的上外边距和下外边距会发生合并。** 如果两个相邻的兄弟元素分别具有上外边距和下外边距，且它们之间没有边框、内边距或内容，那么这两个外边距会合并成一个外边距，取其中较大的值。
  2. **父元素与第一个/最后一个子元素的外边距会发生合并。** 如果一个元素具有上外边距，而其第一个子元素也具有上外边距，那么这两个外边距会合并成一个外边距，取其中较大的值。同样的规则也适用于下外边距。
  
  ## 036 Adding Dimensions（添加尺寸）
  
  将一个元素的宽度设为 100%，那么该元素的宽度为其父元素的宽度。
  
  ## 037 Centering our Page
  
  将所有内容包含在一个 div 内。设置div的属性如下
  
  ```css
  widht: 700px;
  margin-left: auto;
  margin-right: auto;
  ```
  
  ## 039 CSS Theory #4_ Types of Boxes
  
  ### Black-level Element
  
  * 元素以视觉上的块状格式进行排列
  * 元素占据父元素宽度的100%，无论内容多少。
  * 元素默认垂直堆叠，依次排列在一起。
  * 盒子模型按照之前展示的方式应用。
  
  通过设置属性 display: block 将元素更改为块级元素。
  
  ### Inline Element
  
  * 仅占据其内容所需的空间。
  * 在元素之后或之前不引起换行。
  * 盒子模型以一种不同的方式显示，高度和宽度不适用。
  * 内边距和外边距仅在水平方向应用。
  
   通过设置属性 display: inline 可将元素更改为 inline 元素。
  
  ### Inline-Block Box
  
  * Looks like inline from the outside, behaves like block-level on the inside.
  * Occupies only content's space 
  * Causes no line-breaks
  * Box-model applies as showed
  
  通过设置属性 display: inline-block 可将元素改为 inline-block 元素

## 040 CSS Theory #5_ Absoulte Positioning

### Normal Flow

* 默认定位方式
* 元素 "in flow"
* 元素按其在 HTML 代码中的位置进行布局

通过设置属性 position: relative 将定位方式改为 Normal Flow。

### AbsolutePositioning

* 元素从 normal flow 中移除，"out of flow"
*  不影响周围元素，可能会出现重叠。
* 我们使用top、bottom、left或right来将元素从其相对定位的容器中偏移。

视频中修改 button 的位置，首先将 button 的父元素属性 position 修改为 relative，然后 button 的属性设置如下：

```css
button {
    font-size: 22px;
    cursor: pointer;
    position: absolute;
    
    bottom: 50px;
    right: 50px;
}
```

## 041 Pseudo-element

```css
h1::first-letter
h2::first-line
/* 兄弟选择器, 下面的选择器选择 h3 元素的第一个兄弟，且该兄弟为 p 元素*/
h3 + p 
/* 
CSS伪元素::after用来创建一个伪元素，作为已选中元素的最后一个子元素。通常会配合content属性来为该元素添加装饰内容。这个虚拟元素默认是行内元素。 
::before 创建一个伪元素，其将成为匹配选中的元素的第一个子元素。常通过 content 属性来为一个元素添加修饰性的内容。此元素默认为行内元素。
*/
h1::after {
  content: "TOP";
}
```

## 047 Using Floats

`float` CSS 属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动（文档流）中移除，但是仍然保持部分的流动性（与[绝对定位](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position#absolute_positioning)相反）。

包含浮动元素的容器不会为浮动元素调整它的高度

## 048 Clearing Floats

**`clear`** [CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) 属性指定一个元素是否必须移动 (清除浮动后) 到在它之前的浮动元素下面。`clear` 属性适用于浮动和非浮动元素。

### 方法1 

创建一个 div，设置属性 clear: both, 清除左右浮动后，移动到浮动元素的下面。

### 方法2

利用伪元素

```css
.clearfix::after {
  clear: both;
  content: "";
  display: block;
}
```

## 049 Building a Simple Float Layout

将容器的两个子元素分别设为左右浮动，可实现左右两列的布局。

## 050 box-sizing_ border-box

将元素的属性 box-sizing 设为 border-box，可改为按照 box 的长宽设定值。

box-sizing 的默认值为 content-box。

## 053 A Flexbox Overview

[CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) **`display`** 属性设置元素是否被视为[块或者内联元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_flow_layout)以及用于子元素的布局，例如[流式布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_flow_layout)、[网格布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_grid_layout)或[弹性布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_flexible_box_layout)。

```css
/* FLEXBOX */
display: flex;
align-items: center;
justify-content: space-between;
```

![image-20240229120625329](assets\image-20240229120625329.png)

![image-20240229121010750](assets\image-20240229121010750.png)

## 054 Spaceing and Aligning Flex Items

## 055 The flex Property

## 056 Adding Flexbox to Our Porject

