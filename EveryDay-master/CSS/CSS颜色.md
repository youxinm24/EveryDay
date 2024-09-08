CSS 中的颜色可以通过多种方式表示，主要包括以下几类：

### 1. **命名颜色**
CSS 提供了一些常见颜色的名称，你可以直接使用它们。比如：

- `red`
- `blue`
- `green`
- `black`
- `white`
- `gray`
- `yellow`
- `purple`
- `orange`

例如：

```css
color: red;
background-color: blue;
```

### 2. **十六进制颜色（Hex）**
使用 `#` 后跟 3 或 6 位的数字和字母来表示颜色。例如：

- `#FF0000` （红色）
- `#00FF00` （绿色）
- `#0000FF` （蓝色）
- `#000000` （黑色）
- `#FFFFFF` （白色）

例如：

```css
color: #FF0000;
background-color: #00FF00;
```

### 3. **RGB 颜色**
使用 `rgb()` 函数，指定红、绿、蓝三种颜色的值，范围是 0 到 255。例如：

- `rgb(255, 0, 0)` （红色）
- `rgb(0, 255, 0)` （绿色）
- `rgb(0, 0, 255)` （蓝色）

例如：

```css
color: rgb(255, 0, 0);
background-color: rgb(0, 255, 0);
```

### 4. **RGBA 颜色**
与 `rgb()` 类似，但增加了一个透明度参数，范围是 0 到 1。例如：

- `rgba(255, 0, 0, 0.5)` （半透明红色）
- `rgba(0, 255, 0, 0.7)` （半透明绿色）

例如：

```css
color: rgba(255, 0, 0, 0.5);
background-color: rgba(0, 255, 0, 0.7);
```

### 5. **HSL 颜色**
使用 `hsl()` 函数，指定色相、饱和度和亮度。例如：

- `hsl(0, 100%, 50%)` （红色）
- `hsl(120, 100%, 50%)` （绿色）
- `hsl(240, 100%, 50%)` （蓝色）

例如：

```css
color: hsl(0, 100%, 50%);
background-color: hsl(120, 100%, 50%);
```

### 6. **HSLA 颜色**
与 `hsl()` 类似，但增加了透明度参数。例如：

- `hsla(0, 100%, 50%, 0.5)` （半透明红色）
- `hsla(120, 100%, 50%, 0.7)` （半透明绿色）

例如：

```css
color: hsla(0, 100%, 50%, 0.5);
background-color: hsla(120, 100%, 50%, 0.7);
```

### 7. **CSS 自定义属性（变量）**
你可以使用 CSS 变量来定义颜色，然后在需要的地方使用。例如：

```css
:root {
  --main-color: #3498db;
  --secondary-color: rgb(255, 99, 71);
}

element {
  color: var(--main-color);
  background-color: var(--secondary-color);
}
```

