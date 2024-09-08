在 CSS 中，属性可以分为**继承属性**和**非继承属性**。它们之间的区别在于子元素是否会默认从父元素继承这些属性的值。

### 1. 继承属性 (Inherited Properties)
继承属性是指子元素会自动从父元素继承其值的属性。换句话说，如果父元素应用了某个继承属性，子元素会默认使用相同的值，除非在子元素上显式地设置了该属性的值。

#### 常见的继承属性包括：
- **文本相关属性**:
  - `color` (文本颜色)
  - `font-family` (字体)
  - `font-size` (字体大小)
  - `font-style` (字体样式)
  - `font-weight` (字体粗细)
  - `line-height` (行高)
  - `text-align` (文本对齐)
  - `visibility` (可见性)
  - `letter-spacing` (字间距)
  - `word-spacing` (词间距)
  - `text-indent` (文本缩进)
  - `text-transform` (文本转换)
- **列表相关属性**:
  - `list-style` (列表样式)
- **表单相关属性**:
  - `cursor` (光标样式)

#### 示例:
```css
.parent {
  color: red;
}

.child {
  /* color 将继承自 .parent, 因为 color 是一个继承属性 */
}
```
在上面的例子中，`.child` 元素的文本颜色会自动继承 `.parent` 的红色。

### 2. 非继承属性 (Non-Inherited Properties)
非继承属性是指子元素不会自动从父元素继承其值的属性。如果需要对子元素应用特定的值，必须显式地在子元素上定义这些属性。

#### 常见的非继承属性包括：
- **盒子模型相关属性**:
  - `margin` (外边距)
  - `padding` (内边距)
  - `border` (边框)
  - `width` (宽度)
  - `height` (高度)
- **布局相关属性**:
  - `display` (显示方式)
  - `position` (定位方式)
  - `top` / `bottom` / `left` / `right` (定位偏移)
  - `overflow` (溢出处理)
  - `float` (浮动)
  - `clear` (清除浮动)
- **背景相关属性**:
  - `background-color` (背景颜色)
  - `background-image` (背景图像)
  - `background-size` (背景大小)
- **其他**:
  - `opacity` (透明度)
  - `z-index` (层级)
  - `box-shadow` (盒子阴影)
  - `text-decoration` (文本装饰)
  - `transform` (变换)

#### 示例:
```css
.parent {
  margin: 20px;
}

.child {
  /* margin 不会继承自 .parent, 因为 margin 是一个非继承属性 */
}
```
在上面的例子中，`.child` 元素不会继承 `.parent` 的 `margin` 值。它的 `margin` 会默认设置为 0，除非显式地在 `.child` 上设置了 `margin`。

### 总结
- **继承属性**: 子元素会自动从父元素继承这些属性的值。
- **非继承属性**: 子元素不会自动继承这些属性的值，除非显式设置。

`initial` 和 `unset` 都是 CSS 中的全局属性值，它们在处理样式时有不同的作用和用法。以下是它们的区别：

### 1. `initial`
- **作用**: 将 CSS 属性重置为其初始值（浏览器默认值）。
- **继承**: 对于大多数非继承属性，`initial` 会将属性设置为它的默认初始值，而对于继承属性，它通常表现为设置为浏览器的默认初始值。
- **适用性**: 适用于所有 CSS 属性。

**示例**:
```css
div {
  color: red; /* 父元素的颜色 */
}

.child {
  color: initial; /* 重置为浏览器的默认颜色 */
}
```
在上面的例子中，如果 `color` 属性使用了 `initial`，它会被设置为浏览器的默认文本颜色（通常是黑色）。

### 2. `unset`
- **作用**: 结合了 `inherit` 和 `initial` 的效果。
  - 对于**继承属性**（例如 `color`、`font-family`），`unset` 的行为等同于 `inherit`，即继承父元素的属性值。
  - 对于**非继承属性**（例如 `display`、`margin`），`unset` 的行为等同于 `initial`，即将属性设置为其初始值。
- **继承**: 针对继承属性，`unset` 会继承父元素的值；对于非继承属性，它将属性设置为初始值。

**示例**:
```css
div {
  color: red; /* 父元素的颜色 */
  margin: 20px; /* 父元素的外边距 */
}

.child {
  color: unset;  /* 对继承属性, 等同于 inherit, 子元素将继承 red */
  margin: unset; /* 对非继承属性, 等同于 initial, 子元素将使用初始值 (0) */
}
```
在上面的例子中：
- `color: unset` 将继承父元素的 `color` 值（红色）。
- `margin: unset` 将重置 `margin` 到其初始值（通常为 `0`）。

### 总结
- **`initial`**: 始终重置为属性的初始值，无论该属性是否继承。
- **`unset`**: 对继承属性表现为 `inherit`，对非继承属性表现为 `initial`。

根据需要决定是将属性重置为初始值（`initial`），还是根据属性类型决定是继承还是重置为初始值（`unset`）。