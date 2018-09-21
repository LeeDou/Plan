# Grid 布局的基本概念

## 什么是网格布局
网格是一组相交的水平和垂直线，他定义了网格的行和列，我们可以将网格元素放置在与这些行和列相关的位置上。CSS网格布局有以下特点：
- 固定的位置和弹性的大小（fr创建弹性尺寸）
- 元素位置（行号行名或标定的网格区域精确定位元素）
- 创建额外的轨道来包含元素
- 对齐控制
- 控制重叠内容（z-index控制优先级）

## 网格容器
通过声明`display: grid`或`display: inline-grid` 来创建一个容器。所有的直接子元素都是网格项

## 网格轨道
grid-template-columns 和 grid-template-columns-row 属性来定义网格中的行和列。这些属性定义网格轨道，一个网格轨道就是王铮中任意两条线之间的距离。
```
  <div class="wrapper">
    <div>One</div>
    <div>Two</div>
    <div>Three</div>
    <div>Four</div>
    <div>Five</div>
  </div>

.wrapper {
  display: grid;
  grid-template-columns: 200px 200px 200px; // 定义每列及每列的宽度
}
```

- fr 单位： 表示容器中可用空间的一等份
- 在轨道清单中使用reapeat() : 有着多轨道的大型网格可使用reapeat() 标记来重复部分或整个轨道列表列表
```
.wapper {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}
// 也可写成：
.wapper {
  display: grid;
  grid-templeate-columns: repeat(3, 1fr)
}
```
> reapeat 语句可以用于重复轨道的一部分
```
wapper {
  display: grid;
  grid-template-columns: 20px repeat(6, 1fr) 20px;
}
```

- 隐式和显示网格
当我们创建上文中的网格例子的时候，我们使用grid-template-colunmns 属性定义了自己的列轨道，但是却没让网格安所需的内容创建行，这些行会被创建在隐式网格中。显示网格包含了在grid-template-columns 和grid-template-rows 属性中定义的行和列。如果你在网格之外又定义了另外的东西，或者因内容数量而需要更多的网格轨道的时候网格将会在隐式网格中创建行和列。
grid-auto-rows 属性来却报在隐式网格中创建的轨道像素高。

- 轨道大小 minmax()
给网格一个最小的尺寸，确保他们能扩大到容纳他里面添加的内容。
```
.wapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: minmax(100px, auto); //自动创建行高将会是最100px，auto意味着行的尺寸会根据内容的大小来自动变换：根据最高的单元，扩展到足够容纳改单元。
}
```

## 网格线
网格线的编号顺序取决于文章书写模式，在从左至右的书写的语言中，编号为1的在最左边，在从右至左的书写语言中，编号为1的网格线在最右边

## 跨轨道放置网格元素
在放置元素时使用网格线定位，而非网格轨道
grid-column-start,grid-column-end,grid-row-start,grid-row-end
```
<div class="wrapper">
   <div class="box1">One</div>
   <div class="box2">Two</div>
   <div class="box3">Three</div>
   <div class="box4">Four</div>
   <div class="box5">Five</div>
</div>
// CSS 
.wrapper { 
    display: grid; 
    grid-template-columns: repeat(3, 1fr); 
    grid-auto-rows: 100px; 
} 
.box1 { 
    grid-column-start: 1; 
    grid-column-end: 4; 
    grid-row-start: 1; 
    grid-row-end: 3; 
} 
.box2 { 
    grid-column-start: 1; 
    grid-row-start: 3; 
    grid-row-end: 5; 
}
```

## 网格单元
一个网格单元是在一个网格元素中的最小单位

## 网格区域
网格元素可以向行或列的方向扩展一个或对个单元，并创建一个网格区域，网格区域应该是一个矩形

## 网格间距
在两个网格单元间的网格横向距离或纵向距离 可使用`grid-column-gap`和`grid-row-gap`属性来创建，或者直接使用两个合并的缩写形式`grid-gap`。
```
<div class="wrapper">
   <div>One</div>
   <div>Two</div>
   <div>Three</div>
   <div>Four</div>
   <div>Five</div>
</div>
// CSS 
.wapper {
  diaplay: grid;
  grid-template-columns: reapeat(3, 1fr);
  grid-column-gap: 10px;
  grid-row-gap: 1em; 
}
```

## 嵌套网格
一个网格元素也可以成为一个网格容器，网格元素`display：grid` 

## 子网格
`display: subgrid`

## 使用z-index控制层级
多个网格项目可以占用同一个网格单位，如果我们回到之前根据网格线编号放置网格项目的话，我们可以更改此项来使两个网格项目重叠。
  - 控制顺序
    在网格项目发生重叠时，使用z-index属性控制重叠的顺序

## [更多](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout)
