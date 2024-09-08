# CSS3新特性
`CSS3`是最新的`CSS`标准，旨在扩展`CSS2.1`。

## 圆角
通过`border-radius`属性可以给任何元素制作圆角。
* `border-radius`: 所有四个边角`border-*-*-radius`属性的缩写。
* `border-top-left-radius`: 定义了左上角的弧度。
* `border-top-right-radius`: 定义了右上角的弧度。
* `border-bottom-right-radius`: 定义了右下角的弧度。
* `border-bottom-left-radius`: 定义了左下角的弧度。

```html
<div id="t1"></div>
<style type="text/css">
    #t1{
        height: 100px;
        width: 100px;
        background-color: blue;
        border-radius: 10px;
    }
</style>
```

## 盒阴影
`box-shadow: h-shadow v-shadow blur spread color inset`  
* `h-shadow`: 必需，水平阴影的位置，允许负值。
* `v-shadow`: 必需，垂直阴影的位置，允许负值。
* `blur`: 可选，模糊距离。
* `spread`: 可选，阴影的大小。
* `color`: 可选，阴影的颜色。在CSS颜色值寻找颜色值的完整列表。
* `inset`: 可选，从外层的阴影改变阴影内侧阴影。

```html
<div id="t2"></div>
<style type="text/css">
    #t2{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        box-shadow: 5px 5px 5px #aaa;
    }
</style>
```

## 背景
`CSS3`中包含几个新的背景属性，提供更大背景元素控制。 
* `background-image`: 规定背景图片路径。
* `background-clip`: 规定背景的绘制区域。
* `background-origin`: 规定背景图片的定位区域。
* `background-size`: 规定背景图片的尺寸。

```html
<div id="t3"></div>
<style type="text/css">
    #t3{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        background-image: url(https://blog.touchczy.top/favicon.ico);
        background-size:30px 30px;
        background-repeat:repeat;
        /* no-repeat：不重复背景图像，只显示一次。
		repeat-x：仅在水平方向重复背景图像。
		repeat-y：仅在垂直方向重复背景图像。
		space：在背景图像之间留出空白，不裁剪背景图像。
		round：缩放背景图像，使其适应容器，并在可能的情况下填满整个容器。*/
        background-origin:content-box;
        /* 
        content-box:从内容区域的左上角开始绘制，而不会延伸到内边距或边框区域
		padding-box：背景图像从内边距区域的左上角开始绘制，默认值。
		border-box：背景图像从边框区域的左上角开始绘制。*/
    }
</style>
```

## 渐变
`CSS3`渐变可以在两个或多个指定的颜色之间显示平稳的过渡。 
* `Linear Gradients`: 线性渐变，向下/向上/向左/向右/对角方向。
* `Radial Gradients`: 径向渐变，由中心定义。

```html
<div id="t4"></div>
<style type="text/css">
    #t4{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        background-image: linear-gradient(to right, red , yellow);
    }
</style>
```

## 文本效果

`CSS3`对文本进行了更多的支持。  

* `hanging-punctuation`: 规定标点字符是否位于线框之外。
* `punctuation-trim`: 规定是否对标点字符进行修剪。
* `text-align-last`: 设置如何对齐最后一行或紧挨着强制换行符之前的行。
* `text-emphasis`: 向元素的文本应用重点标记以及重点标记的前景色。
* `text-justify`: 规定当`text-align`设置为`justify`时所使用的对齐方法。
* `text-outline`: 规定文本的轮廓。
* `text-overflow`: 规定当文本溢出包含元素时发生的事情。
* `text-shadow`: 向文本添加阴影。
* `text-wrap`: 规定文本的换行规则。
* `word-break`: 规定非中日韩文本的换行规则。
* `word-wrap`: 允许对长的不可分割的单词进行分割并换行到下一行。

以下是对这些 CSS 属性的详细介绍：

### 1. **`hanging-punctuation`**
- **描述**: `hanging-punctuation` 属性用于控制标点符号是否可以在其所在的文本框之外显示。
- **可选值**:
  - `none`（默认）：不允许悬挂标点。
  - `first`：允许首行标点悬挂。
  - `last`：允许末行标点悬挂。
  - `force-end`：强制末行标点悬挂。
  - `allow-end`：允许末行标点悬挂，但不强制。

示例

```css
p {
  hanging-punctuation: first last;
}
```
在这个示例中，首行和末行的标点符号都可以悬挂在文本框之外。

### 2. **`punctuation-trim`**

- **描述**: `punctuation-trim` 属性用于控制是否修剪文本中的标点符号。
- **可选值**:
  - `none`（默认）：不修剪标点符号。
  - `start`：修剪行首的标点符号。
  - `end`：修剪行末的标点符号。
  - `adjacent`：修剪行首和行末的标点符号。

示例

```css
p {
  punctuation-trim: end;
}
```
在这个示例中，行末的标点符号将被修剪。

### 3. **`text-align-last`**

- **描述**: `text-align-last` 属性用于设置如何对齐最后一行或紧挨着强制换行符之前的行。
- **可选值**:
  - `auto`（默认）：继承 `text-align` 的值。
  - `left`：左对齐最后一行。
  - `right`：右对齐最后一行。
  - `center`：最后一行居中对齐。
  - `justify`：最后一行两端对齐。

示例

```css
p {
  text-align: justify;
  text-align-last: left;
}
```
在这个示例中，段落的文本是两端对齐的，但最后一行是左对齐的。

### 4. **`text-emphasis`**
- **描述**: `text-emphasis` 属性用于向文本应用重点标记（如点或圈）及其前景色。
- **可选值**:
  - `none`（默认）：没有重点标记。
  - `circle`：应用圆圈标记。
  - `dot`：应用点标记。
  - `sesame`：应用芝麻形状标记。
  - `filled`：应用填充标记。
  - `open`：应用空心标记。
  - `<color>`：设置标记的颜色。

示例

```css
p {
  text-emphasis: circle red;
}
```
在这个示例中，文本将带有红色的圆圈标记。

### 5. **`text-justify`**

- **描述**: `text-justify` 属性规定当 `text-align` 设置为 `justify` 时，使用的对齐方法。
- **可选值**:
  - `auto`（默认）：浏览器自动选择对齐方法。
  - `none`：禁用对齐调整。
  - `inter-word`：通过调整单词之间的空格来对齐。
  - `inter-character`：通过调整字符之间的空格来对齐。

示例

```css
p {
  text-align: justify;
  text-justify: inter-word;
}
```
在这个示例中，段落的文本将通过调整单词之间的空格来实现两端对齐。

### 6. **`text-outline`**
- **描述**: `text-outline` 属性用于为文本设置轮廓效果。这个属性在现代浏览器中并不广泛支持，一般使用 `text-shadow` 实现类似效果。
- **可选值**:
  - `none`（默认）：没有文本轮廓。
  - `<length> <color>`：设置文本轮廓的宽度和颜色。

示例

```css
p {
  text-outline: 1px red;
}
```
在这个示例中，文本将具有 1px 宽的红色轮廓（如果浏览器支持）。

### 7. **`text-overflow`**
- **描述**: `text-overflow` 属性控制当文本溢出包含元素时应采取的措施。
- **可选值**:
  - `clip`（默认）：剪切溢出的文本，不显示省略号。
  - `ellipsis`：用省略号表示溢出的文本部分。
  - `string`：使用指定的字符串表示溢出的文本部分。

示例

```css
p {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
```
在这个示例中，溢出的文本将以省略号显示。

### 8. **`text-shadow`**
- **描述**: `text-shadow` 属性用于向文本添加阴影效果。
- **可选值**:
  - `none`（默认）：无阴影。
  - `<x-offset> <y-offset> <blur-radius> <color>`：分别设置阴影的水平偏移、垂直偏移、模糊半径和颜色。

示例

```css
p {
  text-shadow: 2px 2px 5px gray;
}
```
在这个示例中，文本将有 2px 的水平和垂直阴影，阴影模糊半径为 5px，颜色为灰色。

### 9. **`text-wrap`**
- **描述**: `text-wrap` 属性用于控制文本的换行规则。这个属性在现代 CSS 中已被 `word-wrap` 替代。
- **可选值**:
  - `normal`：文本会在合适的地方自动换行。
  - `none`：文本不会换行。
  - `unrestricted`：没有明确的限制，但不常用。
  - `suppress`：强制抑制换行。

示例

```css
p {
  text-wrap: normal;
}
```
在这个示例中，文本会根据需要自动换行。

### 10. **`word-break`**
- **描述**: `word-break` 属性用于指定如何在单词内断行，尤其是针对非中日韩（CJK）文本。
- **可选值**:
  - `normal`（默认）：按照常规的单词断行规则。
  - `break-all`：可以在单词的任意字符间断行。
  - `keep-all`：只能在单词之间断行（针对 CJK 文本）。

示例

```css
p {
  word-break: break-all;
}
```
在这个示例中，文本可以在单词的任何地方断行。

### 11. **`word-wrap`**
- **描述**: `word-wrap`（现在的标准属性名为 `overflow-wrap`）允许对长的不可分割的单词进行分割并换行到下一行。
- **可选值**:
  - `normal`（默认）：只在允许的断行点换行。
  - `break-word`：必要时在单词内断行以避免溢出。

示例

```css
p {
  word-wrap: break-word;
}
```
在这个示例中，长单词会在必要时被分割并换行到下一行。

这些属性主要用于控制文本的排版和布局，提升网页设计的美观性和可读性。

```html
<div id="t5">Text</div>
<style type="text/css">
    #t5{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        color: #fff;
        text-shadow: -1px 3px 5px #333;
    }
</style>
```

## 字体

`CSS3`可以使用`@font-face`规则加载所需字体。  
* `font-family`: 必需，规定字体的名称。
* `src`: 必需，定义字体文件的`URL`。
* `font-stretch`: 可选，定义如何拉伸字体，默认是`normal`。
* `font-style`: 可选，定义字体的样式，默认是`normal`。
* `font-weight`: 可选，定义字体的粗细，默认是`normal`。
* `unicode-range`: 可选，定义字体支持的`UNICODE`字符范围，默认是`U+0-10FFFF`。

```html
<div id="t6">Text</div>
<style type="text/css">
    @font-face{
        font-family: ff;
        src: url(https://cdn.jsdelivr.net/gh/WindrunnerMax/Yolov3-Train@2d965d2/keras-yolo3/font/FiraMono-Medium.otf);
    }
    #t6{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        font-family:ff;
    }
</style>
```

## 2D转换
`CSS3`转换可以对元素进行移动、缩放、转动、拉长或拉伸。
* `transform`: 适用于`2D`或`3D`转换的元素。
* `transform-origin`: 允许更改转化元素位置。
* **`transform`** 是一个强大的 CSS 属性，可以对元素进行各种 2D 或 3D 变换，如旋转、缩放、平移和倾斜，常用于动画效果和复杂的布局调整。
* **`transform-origin`** 则是定义这些转换效果的基准点的位置，控制转换效果的中心点，允许更精确地控制转换的行为。

以下是对 CSS 属性 `transform` 和 `transform-origin` 的详细介绍：

### 1. **`transform`**
- **描述**: `transform` 属性用于应用 2D 或 3D 转换（如旋转、缩放、平移、倾斜）到元素。可以对元素进行各种变换效果，而不影响周围其他元素的位置。

- **常见值**:
  - `none`：无转换，元素保持原状。
  - `translate(x, y)`：平移元素，`x` 和 `y` 分别表示在 X 和 Y 轴方向上的移动距离。可以使用像素（px）或百分比（%）等单位。
  - `scale(x, y)`：缩放元素，`x` 和 `y` 分别表示在 X 和 Y 轴方向上的缩放比例。`scale(1)` 表示原始大小，`scale(2)` 表示放大两倍。
  - `rotate(angle)`：旋转元素，`angle` 是旋转的角度。正值表示顺时针旋转，负值表示逆时针旋转。
  - `skew(x-angle, y-angle)`：倾斜元素，`x-angle` 和 `y-angle` 分别表示在 X 和 Y 轴方向上的倾斜角度。
  - `matrix(a, b, c, d, e, f)`：结合所有 2D 转换的一个通用函数，使用 6 个参数来描述 2D 变换（平移、旋转、缩放、倾斜的组合）。
  - `translate3d(x, y, z)`：在 3D 空间中平移元素。
  - `scale3d(x, y, z)`：在 3D 空间中缩放元素。
  - `rotate3d(x, y, z, angle)`：在 3D 空间中围绕给定的轴旋转元素。
  - `perspective(n)`：设置 3D 变换的视距。

示例

```css
div {
  transform: rotate(45deg);
}
```
在这个示例中，`div` 元素将顺时针旋转 45 度。

### 2. **`transform-origin`**
- **描述**: `transform-origin` 属性用于定义一个元素进行 2D 或 3D 转换时的基准点，即转换的原点。默认情况下，转换是围绕元素的中心进行的，但可以通过 `transform-origin` 改变这个基准点的位置。

- **常见值**:
  - `x-axis y-axis`：分别表示转换的基准点在 X 和 Y 轴方向上的位置。可以使用长度值（如 `px`、`%`）或关键字（如 `left`、`center`、`right`、`top`、`bottom`）。
  - `z-axis`（可选）：在 3D 转换中，还可以指定 Z 轴上的位置。

- **默认值**: `50% 50%`，即元素的中心点。

示例

```css
div {
  transform: rotate(45deg);
  transform-origin: top left;
}
```
在这个示例中，`div` 元素将围绕左上角（`top left`）旋转 45 度，而不是默认的中心点。

```html
<div id="t7"></div>
<style type="text/css">
    #t7{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        transform:rotate(10deg);
    }
</style>
```

## 3D转换
`CSS3`可以使用`3D`转换来对元素进行格式化。
* `transform`: 向元素应用`2D`或`3D`转换。

* `transform-origin`: 允许你改变被转换元素的位置。

  

* `transform-style`: 规定被嵌套元素如何在`3D`空间中显示。

* `perspective`: 规定`3D`元素的透视效果。

* `perspective-origin`: 规定`3D`元素的底部位置。

  

* `backface-visibility`: 定义元素在不面对屏幕时是否可见。

以下是对这些 CSS 属性的详细介绍：

### 1. **`transform`**
- **描述**: `transform` 属性用于向元素应用 2D 或 3D 转换效果，可以实现元素的旋转、缩放、平移、倾斜等效果。该属性允许对元素进行视觉上的变换，而不影响周围其他元素的位置。

- **常见值**:
  - `none`：无转换效果，元素保持原状。
  - `translate(x, y)`：在 X 和 Y 轴方向上平移元素。
  - `scale(x, y)`：在 X 和 Y 轴方向上缩放元素。
  - `rotate(angle)`：绕 Z 轴旋转元素（2D 转换）。
  - `skew(x-angle, y-angle)`：倾斜元素。
  - `translate3d(x, y, z)`：在 X、Y 和 Z 轴方向上平移元素（3D 转换）。
  - `scale3d(x, y, z)`：在 X、Y 和 Z 轴方向上缩放元素（3D 转换）。
  - `rotate3d(x, y, z, angle)`：在 3D 空间中围绕指定的轴旋转元素。
  - `matrix(a, b, c, d, e, f)`：结合所有 2D 转换的通用函数。

### 2. **`transform-origin`**

- **描述**: `transform-origin` 属性用于定义元素进行转换时的基准点。可以调整元素的转换效果的中心点，从而控制旋转、缩放等转换操作的起始位置。

- **常见值**:
  - `x-axis y-axis`：分别指定基准点在 X 和 Y 轴上的位置。可以使用长度单位（如 `px`）或百分比。
  - `z-axis`（可选）：用于 3D 转换中指定 Z 轴上的位置。
  
### 3. **`transform-style`**
- **描述**: `transform-style` 属性用于指定嵌套元素在 3D 空间中的显示方式。它定义了子元素是否保留在 3D 空间中，还是在 2D 平面上呈现。

- **可选值**:
  - `flat`（默认）：子元素在 2D 平面上显示，所有子元素的 3D 转换效果将被平展。
  - `preserve-3d`：子元素在 3D 空间中显示，保留 3D 转换效果。

### 4. **`perspective`**
- **描述**: `perspective` 属性用于为 3D 转换效果提供透视视图。它定义了观察者与 Z=0 平面的距离，从而影响 3D 元素的深度和视角。

- **常见值**:
  - `<length>`：指定透视距离，距离越小，3D 效果越明显，通常使用 `px` 为单位。

- **注意**: 该属性通常应用于包含 3D 转换元素的父级元素。

### 5. **`perspective-origin`**
- **描述**: `perspective-origin` 属性用于定义透视效果的基准点，即从哪个位置观察 3D 场景。它决定了 3D 元素相对于视口的呈现方式。

- **常见值**:
  - `x-axis y-axis`：分别指定基准点在 X 和 Y 轴上的位置，通常使用百分比或长度单位。
  
### 6. **`backface-visibility`**
- **描述**: `backface-visibility` 属性用于控制当元素背面朝向屏幕时，是否可见。通常用于 3D 旋转效果中，决定元素的背面是否在旋转过程中显示。

- **可选值**:
  - `visible`（默认）：背面可见。
  - `hidden`：背面不可见，即当元素背面朝向屏幕时，元素将隐藏。

```html
<div id="t8"></div>
<style type="text/css">
    #t8{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        transform:rotateX(10deg);
    }
</style>
```

## 动画

`CSS3`可以创建动画，它可以取代许多网页动画图像、`Flash`动画和`JavaScript`实现的效果。
* `@keyframes`: 规定动画。
* `animation`: 所有动画属性的简写属性，除了`animation-play-state`属性。  
* `animation-name`: 规定`@keyframes`动画的名称。
* `animation-duration`: 规定动画完成一个周期所花费的秒或毫秒，默认是`0`。
* `animation-timing-function`: 规定动画的速度曲线，默认是`ease`。
* `animation-fill-mode`: 规定当动画不播放时，例如当动画完成时，或当动画有一个延迟未开始播放时，要应用到元素的样式。
* `animation-delay`: 规定动画何时开始，默认是`0`。
* `animation-iteration-count`: 规定动画被播放的次数，默认是`1`,`infinite`：无限循环播放。
* `animation-direction`: 规定动画是否在下一周期逆向地播放，默认是`normal`。
* `animation-play-state`: 规定动画是否正在运行或暂停，默认是`running`,`paused`/动画暂停。

以下是对 CSS 动画相关属性的详细介绍：

### 1. **`@keyframes`**
- **描述**: `@keyframes` 规则用于定义动画的关键帧，即动画过程中在特定时间点上元素的样式变化。通过 `@keyframes`，可以详细描述动画从开始到结束的过渡效果。

- **语法**:
  
  ```css
  @keyframes animation-name {
    from { /* 起始状态 */ }
    to { /* 结束状态 */ }
    /* 或 */
    0% { /* 开始时的状态 */ }
    100% { /* 结束时的状态 */ }
  }
  ```
  
- **示例**:
  ```css
  @keyframes slide {
    from { transform: translateX(0); }
    to { transform: translateX(100px); }
  }
  ```

### 2. **`animation`**
- **描述**: `animation` 是所有动画属性的简写形式（除了 `animation-play-state`），用于同时设置多个动画属性，如名称、时长、延迟等。

- **语法**:
  ```css
  animation: animation-name animation-duration animation-timing-function animation-delay animation-iteration-count animation-direction animation-fill-mode;
  ```

- **示例**:
  ```css
  div {
    animation: slide 2s ease-in-out 1s infinite alternate;
  }
  ```
  在这个例子中，`div` 元素应用名为 `slide` 的动画，持续 2 秒，延迟 1 秒后开始，以 `ease-in-out` 的速度曲线播放，无限循环，并且每次循环方向相反。

### 3. **`animation-timing-function`**

- **描述**: `animation-timing-function` 属性定义动画的速度曲线，控制动画的加速、减速或线性变化。常见的值包括 `linear`（线性），`ease`（缓动），`ease-in`，`ease-out`，`ease-in-out`，以及 `cubic-bezier()` 自定义函数。

- **语法**:
  ```css
  animation-timing-function: ease-in-out;
  ```

### 4. **`animation-fill-mode`**

- **描述**: `animation-fill-mode` 属性规定动画在播放之前和之后的状态。它决定了在动画未开始或已结束时，元素应采取什么样的样式。

- **常见值**:
  - `none`：默认值，动画未应用任何样式。
  - `forwards`：动画结束后，保持最后一帧的状态。
  - `backwards`：在动画延迟期间，应用动画的第一帧。
  - `both`：结合 `forwards` 和 `backwards` 的效果。

- **语法**:
  ```css
  animation-fill-mode: forwards;
  ```

### 5. **`animation-direction`**

- **描述**: `animation-direction` 属性规定动画在每次完成循环时，是否反向播放。它决定了动画的播放方向和循环模式。

- **常见值**:
  - `normal`：默认值，每次循环都从头到尾播放。
  - `reverse`：每次循环都反向播放。
  - `alternate`：每个奇数循环从头到尾播放，每个偶数循环反向播放。
  - `alternate-reverse`：每个奇数循环反向播放，每个偶数循环从头到尾播放。

- **语法**:
  
  ```css
  animation-direction: alternate;
  ```

```html
<div id="t9"></div>
<style type="text/css">
    @keyframes animation{
        from {background:red;}
        to {background:yellow;}
    }
    #t9{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        animation:animation 5s ease infinite alternate;
    }
</style>
```

## 过渡
`CSS3`中可以使元素从一种样式转变到另一个的时候，无需使用`Flash`动画或`JavaScript`。
* `transition`: 简写属性，用于在一个属性中设置四个过渡属性。
* `transition-property`: 规定应用过渡的`CSS`属性的名称。
* `transition-duration`: 定义过渡效果花费的时间，默认是 0。
* `transition-timing-function`: 规定过渡效果的时间曲线，默认是`ease`。
* `transition-delay`: 规定过渡效果何时开始，默认是 0。

以下是关于 CSS 过渡相关属性的详细介绍：

### 1. **`transition`**
- **描述**: `transition` 是一个简写属性，用于在一个声明中设置元素的过渡效果。通过它可以同时定义过渡的属性、持续时间、时间曲线和延迟时间。

- **语法**:
  ```css
  transition: [transition-property] [transition-duration] [transition-timing-function] [transition-delay];
  ```

- **示例**:
  ```css
  div {
    transition: all 0.5s ease-in-out 1s;
  }
  ```
  在这个例子中，`div` 元素的所有可动画的 CSS 属性（`transition-property: all`）将在 0.5 秒内以 `ease-in-out` 的速度曲线完成过渡，并且在 1 秒后开始。

### 2. **`transition-property`**
- **描述**: `transition-property` 属性用于指定哪些 CSS 属性将在应用过渡效果时发生变化。可以选择单个属性，也可以选择多个属性，甚至可以指定 `all` 来应用于所有支持过渡的属性。

- **常见值**:
  - `all`：对所有可动画属性应用过渡效果。
  - 具体属性名：如 `width`、`height`、`background-color` 等。

- **语法**:
  
  ```css
  transition-property: width, height;
  ```

### 3. **`transition-timing-function`**

- **描述**: `transition-timing-function` 属性控制过渡效果的速度曲线，决定了动画的开始和结束方式（例如是否匀速、加速或减速）。

- **常见值**:
  - `ease`：默认值，动画从慢到快再到慢。
  - `linear`：匀速动画。
  - `ease-in`：动画以慢速开始，然后加速。
  - `ease-out`：动画以快速开始，然后减速。
  - `ease-in-out`：动画以慢速开始，中间加速，最后再减速。
  - `cubic-bezier(n,n,n,n)`：自定义贝塞尔曲线，允许精确控制动画的时间曲线。

- **语法**:
  
  ```css
  transition-timing-function: ease-in-out;
  ```

```html
<div id="t10"></div>
<style type="text/css">
    #t10{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        background: red;
        transition: all .5s;
    }
    #t10:hover{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        background: yellow;
        transition: all .5s;
    }
</style>
```

## Flex布局
通过指定`display: flex`来标识一个弹性布局盒子，称为`FLEX`容器，容器内部的盒子就成为`FLEX`容器的成员，容器默认两根轴线，水平的主轴与垂直的交叉轴，主轴的开始位置叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`，容器成员默认按照主轴排列。  

```
https://github.com/WindrunnerMax/EveryDay/blob/master/CSS/Flex布局.md
```

## Grid布局
通过指定`display: grid;`指定容器使用`Grid`布局，`Grid`布局中采用网格布局的区域，称为容器，容器内部采用网格定位的子元素，称为成员。容器中水平区域称为行，垂直区域称为列，可以将其看作二位数组。划分网格的线就称为网格线，正常情况下`n`行有`n + 1`根水平网格线，`m`列有`m + 1`根垂直网格线。注意当容器设置为`Grid`布局以后，容器子元素的`float`、`display: inline-block`、`display: table-cell`、`vertical-align`和`column-*`等设置都将失效。

```
https://github.com/WindrunnerMax/EveryDay/blob/master/CSS/Grid布局.md
```

## 多列布局
`CSS3`可以将文本内容设计成像报纸一样的多列布局。
* `column-count`: 指定元素应该被分割的列数。
* `column-fill`: 指定如何填充列。
* `column-gap`: 指定列与列之间的间隙。
* `column-rule`: 所有`column-rule-*`属性的简写。
* `column-rule-color`: 指定两列间边框的颜色。
* `column-rule-style`: 指定两列间边框的样式。
* `column-rule-width`: 指定两列间边框的厚度。
* `column-span`: 指定元素要跨越多少列。
* `column-width`: 指定列的宽度。
* `columns`: 设置`column-width`和`column-count`的简写。

```html
<div id="t11">多列布局示例</div>
<style type="text/css">
    #t11{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        column-count: 3;
        column-gap: 20px;
    }
</style>
```

## 用户界面
`CSS3`中增加了一些新的用户界面特性来调整元素尺寸，框尺寸和外边框。
* `appearance`: 允许使一个元素的外观像一个标准的用户界面元素。
* `box-sizing`: 允许以适应区域而用某种方式定义某些元素。
* `icon`: 为创作者提供了将元素设置为图标等价物的能力。
* `nav-down`: 指定在何处使用箭头向下导航键时进行导航。
* `nav-index`: 指定一个元素的`Tab`的顺序。
* `nav-left`: 指定在何处使用左侧的箭头导航键进行导航。
* `nav-right`: 指定在何处使用右侧的箭头导航键进行导航。
* `nav-up`: 指定在何处使用箭头向上导航键时进行导航。
* `outline-offset`: 外轮廓修饰并绘制超出边框的边缘。
* `resize`: 指定一个元素是否是由用户调整大小。

```html
<div id="t12"></div>
<style type="text/css">
    #t12{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        resize:both;
        overflow:auto;
    }
</style>
```

## 滤镜
`CSS3`的`filter`属性可支持对于网页进行各种滤镜效果。  
`filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() | hue-rotate() | invert() | opacity() | saturate() | sepia() | url();`  

```html
<div id="t13"></div>
<style type="text/css">
    #t13{
        height: 100px;
        width: 100px;
        border: 1px solid #eee;
        filter: blur(3px);
        background-color: blue;
    }
</style>
```

## 选择器

* `element1~element2`: 选择同级前面有`element1`元素的全部`element2`元素
* `[attribute^=value]`: 选择`attribute`属性中以`value`开头的元素
* `[attribute$=value]`: 选择`attribute`属性中以`value`结尾的元素
* `[attribute*=value]`: 选择`attribute`属性中包含`value`字符串的元素
* `div:first-child`: 选择属于其父元素的第一个子元素的每个`div`元素
* `div:last-child`: 选择属于其父元素最后一个子元素的每个`div`元素
* `div:nth-child(n)`: 选择属于其父元素的第n个子元素的每个`div`元素
* `div:nth-last-child(n)`: 同上，从这个元素的最后一个子元素开始算
* `div:nth-of-type(n)`: 选择属于其父元素第n个`div`元素的每个`div`元素
* `div:nth-last-of-type(n)`: 同上，但是从最后一个子元素开始计数
* `div:first-of-type`: 选择属于其父元素的首个`div`元素的每个`div`元素
* `div:last-of-type`: 选择属于其父元素的最后`div`元素的每个`div`元素
* `div:only-child`: 选择属于其父元素的唯一子元素的每个`div`元素
* `div:only-of-type`: 选择属于其父元素唯一的`div`元素的每个`div`元素
* `:root`: 选择文档的根元素
* `:empty`: 选择的元素里面没有任何内容
* `:checked`: 匹配被选中的input元素，这个input元素包括radio和checkbox
* `:default`: 匹配默认选中的元素，例如：提交按钮总是表单的默认按钮
* `:disabled`: 匹配禁用的表单元素
* `:enabled`: 匹配没有设置disabled属性的表单元素
* `:valid`: 匹配条件验证正确的表单元素

## 媒体查询
可以针对不同的媒体类型设置不同的样式规则，可以根据视窗、设备高度与宽度、设备方向、分辨率等进行不同`CSS`适配。

```html
<div id="t14"></div>
<style type="text/css">
    @media screen and (min-width:600px){ 
        #t14{
            height: 100px;
            width: 100px;
            border: 1px solid #eee;
            background: red;
            transition: all .5s;
        }
    }
    @media screen and (max-width:600px) { 
        #t14{
            height: 100px;
            width: 100px;
            border: 1px solid #eee;
            background: yellow;
            transition: all .5s;
        }
    }
</style>
```
## 代码示例

```html
<!DOCTYPE html>
<html>
<head>
    <title>CSS3新特性</title>
    <style type="text/css">
        div{
            margin: 10px 0;
            height: 100px;
            width: 100px;
            border: 1px solid #eee;
        }
        #t1{
            border-radius: 10px;
            background-color: blue;
        }
        #t2{
            box-shadow: 5px 5px 5px #aaa;
        }
        #t3{
            border: 1px solid #eee;
            background-image: url(https://blog.touchczy.top/favicon.ico);
            background-size:30px 30px;
            background-repeat:repeat;
            background-origin:content-box;
        }
        #t4{
            background-image: linear-gradient(to right, red , yellow);
        }
        #t5{
            color: #fff;
            text-shadow: -1px 3px 5px #333;
        }
        @font-face{
            font-family: ff;
            src: url(https://cdn.jsdelivr.net/gh/WindrunnerMax/Yolov3-Train@2d965d2/keras-yolo3/font/FiraMono-Medium.otf);
        }
        #t6{
            font-family:ff;
        }
        #t7{
            transform:rotate(10deg);
        }
        #t8{
            transform:rotateX(10deg);
        }
        @keyframes animation{
            from {background:red;}
            to {background:yellow;}
        }
        #t9{
            animation:animation 5s ease infinite alternate;
        }
        #t10{
            background: red;
            transition: all .5s;
        }
        #t10:hover{
            background: yellow;
            transition: all .5s;
        }
        #t11{
            column-count: 3;
            column-gap: 20px;
        }
        #t12{
            resize:both;
            overflow:auto;
        }
        #t13{
            filter: blur(3px);
            background-color: blue;
        }
        @media screen and (min-width:600px){ 
            #t14{
                height: 100px;
                width: 100px;
                border: 1px solid #eee;
                background: red;
                transition: all .5s;
            }
        }
        @media screen and (max-width:600px) { 
            #t14{
                height: 100px;
                width: 100px;
                border: 1px solid #eee;
                background: yellow;
                transition: all .5s;
            }
        }
    </style>
</head>
<body>
    <div id="t1">圆角</div>
    <div id="t2">盒阴影</div>
    <div id="t3">背景</div>
    <div id="t4">渐变</div>
    <div id="t5">文本效果</div>
    <div id="t6">FONT</div>
    <div id="t7">2D转换</div>
    <div id="t8">3D转换</div>
    <div id="t9">动画</div>
    <div id="t10">过渡</div>
    <div id="t11">多列布局示例</div>
    <div id="t12">用户界面</div>
    <div id="t13">滤镜</div>
    <div id="t14">媒体查询</div>

</body>
</html>
```

## 每日一题

```
https://github.com/WindrunnerMax/EveryDay
```

## 参考

```
https://www.jianshu.com/p/f988d438ee17
https://www.runoob.com/css3/css3-tutorial.html
https://developer.mozilla.org/zh-CN/docs/Archive/CSS3
```
