## less 定义复用样式

### 定义不带参数的属性集合

1，可用于隐藏这些属性集合，不让它暴露到 CSS 中。

```less
// 是网页整体变成灰色
.htmlToGray() {
  filter: grayscale(100%);
  -webkit-filter: grayscale(100%);
  -moz-filter: grayscale(100%);
  -ms-filter: grayscale(100%);
  -o-filter: grayscale(100%);
  filter: url("data:image/svg+xml;utf8,#grayscale");
  filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);
  -webkit-filter: grayscale(1);
}

// 双击不选中文字
.clickNoSelectText {
  -moz-user-select: none;
  /*火狐*/
  -webkit-user-select: none;
  /*webkit浏览器*/
  -ms-user-select: none;
  /*IE10*/
  -khtml-user-select: none;
  /*早期浏览器*/
  user-select: none;
}
```

### 定义带参数的属性集合

1，不给参数设置默认值。

```less
.border-radius (@radius) {
  border-radius: @radius;
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
}

#header {
  .border-radius(4px);
}

.button {
  .border-radius(6px);  
}
```

2，给参数设置默认值。

```less
.border-radius (@radius : 5px) {
  border-radius: @radius;
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
}

#header {
  .border-radius(4px);
}
```

3，arguments 包含了所有的传递进来的参数，不用单独处理每一个参数。

```less
.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);
```

> 以上样式编译结果如下：

```css
.box-shadow{
  box-shadow: 2px 5px 1px #000;
  -moz-box-shadow: 2px 5px 1px #000;
  -webkit-box-shadow: 2px 5px 1px #000;
}
```

### 匹配模式

1，可以通过参数与实参的名称进行匹配，也可以通过参数的个数进行匹配。

```less
//让.mixin根据不同的@switch值而表现各异
.mixin (dark, @color) {
  color: darken(@color, 10%);
}
.mixin (light, @color) {
  color: lighten(@color, 10%);
}
.mixin (@_, @color) {
  display: block;
}

//运行
@switch: light;

.class {
  .mixin(@switch, #888);
}
```

> 编译结果如下：

```css
.class {
  color: #a2a2a2;
  display: block;
}
/*
  mixin就会得到传入颜色的浅色。如果@switch设为dark，就会得到深色。

  具体实现如下：
  第一个混合定义并未被匹配，因为它只接受dark做为首参。
  第二个混合定义被成功匹配，因为它只接受light。
  第三个混合定义被成功匹配，因为它接受任意值。

  只有被匹配的混合才会被使用。变量可以匹配任意的传入值，而变量以外的固定值就仅仅匹配与其相等的。
*/
```