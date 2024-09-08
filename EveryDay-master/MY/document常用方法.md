`document` 对象是 JavaScript 中表示整个 HTML 文档的根节点，它包含许多用于操作和查询 DOM 的方法。以下是常见的 `document` 上的一些方法：

### **获取元素相关的方法**
1. **`getElementById(id)`**  
   根据元素的 `id` 返回单个元素。
   ```js
   const element = document.getElementById('myId');
   ```

2. **`getElementsByClassName(className)`**  
   返回具有指定 `class` 的所有元素的集合 (HTMLCollection)。
   ```js
   const elements = document.getElementsByClassName('myClass');
   ```

3. **`getElementsByTagName(tagName)`**  
   返回具有指定标签名的所有元素的集合。
   ```js
   const elements = document.getElementsByTagName('div');
   ```

4. **`getElementsByName(name)`**  
   返回具有指定 `name` 属性的所有元素 (通常用于表单元素)。
   ```js
   const elements = document.getElementsByName('myName');
   ```

5. **`querySelector(selector)`**  
   返回与指定的 CSS 选择器匹配的**第一个元素**。
   ```js
   const element = document.querySelector('.myClass');
   ```

6. **`querySelectorAll(selector)`**  
   返回与指定的 CSS 选择器匹配的**所有元素** (NodeList)。
   ```js
   const elements = document.querySelectorAll('.myClass');
   ```

### **创建与插入元素的方法**
7. **`createElement(tagName)`**  
   创建一个指定标签的元素。
   ```js
   const newDiv = document.createElement('div');
   ```

8. **`createTextNode(text)`**  
   创建包含指定文本的文本节点。
   ```js
   const textNode = document.createTextNode('Hello, world!');
   ```

9. **`createDocumentFragment()`**  
   创建一个文档片段，可以用于将多个节点插入 DOM，减少页面的重排。
   ```js
   const fragment = document.createDocumentFragment();
   ```

10. **`appendChild(child)`**  
    将子节点添加到父节点的末尾。
    ```js
    document.body.appendChild(newDiv);
    ```

11. **`insertBefore(newNode, referenceNode)`**  
    将新节点插入到参考节点之前。
    ```js
    const parent = document.getElementById('parent');
    const newNode = document.createElement('p');
    const referenceNode = document.getElementById('child');
    parent.insertBefore(newNode, referenceNode);
    ```

12. **`removeChild(child)`**  
    从 DOM 中移除子节点。
    ```js
    parent.removeChild(referenceNode);
    ```

13. **`replaceChild(newNode, oldNode)`**  
    用新节点替换现有节点。
    ```js
    parent.replaceChild(newNode, oldNode);
    ```

### **获取文档属性和内容的方法**
14. **`documentElement`**  
    返回文档的根元素，通常是 `<html>`。
    ```js
    const rootElement = document.documentElement;
    ```

15. **`body`**  
    返回文档的 `<body>` 元素。
    ```js
    const bodyElement = document.body;
    ```

16. **`head`**  
    返回文档的 `<head>` 元素。
    ```js
    const headElement = document.head;
    ```

17. **`title`**  
    获取或设置文档的标题。
    ```js
    document.title = 'New Title';
    ```

18. **`cookie`**  
    获取或设置文档的 cookie 信息。
    ```js
    document.cookie = "username=John Doe; expires=Thu, 18 Dec 2024 12:00:00 UTC";
    ```

19. **`URL`**  
    返回当前文档的 URL。
    ```js
    const url = document.URL;
    ```

20. **`referrer`**  
    返回引导用户到当前页面的前一个页面的 URL。
    ```js
    const referrer = document.referrer;
    ```

21. **`lastModified`**  
    返回文档的最后修改日期和时间。
    ```js
    const lastMod = document.lastModified;
    ```

22. **`location`**  
    返回或设置文档的 URL，`location` 对象提供了对 URL 相关信息和控制导航的接口。
    ```js
    console.log(document.location.href);
    ```

### **事件处理相关的方法**
23. **`addEventListener(type, listener, options)`**  
    向指定的元素添加事件监听器。
    ```js
    document.addEventListener('click', () => {
      console.log('Document was clicked!');
    });
    ```

24. **`removeEventListener(type, listener, options)`**  
    移除事件监听器。
    ```js
    const handleClick = () => { console.log('clicked'); };
    document.addEventListener('click', handleClick);
    document.removeEventListener('click', handleClick);
    ```

### **查询与遍历方法**
25. **`hasFocus()`**  
    检查当前文档是否获得焦点。
    ```js
    const focused = document.hasFocus();
    ```

26. **`getSelection()`**  
    返回用户当前选中的文本。
    ```js
    const selectedText = document.getSelection().toString();
    ```

27. **`elementFromPoint(x, y)`**  
    返回文档中位于指定坐标处的元素。
    ```js
    const element = document.elementFromPoint(100, 200);
    ```

### **样式和样式表**
28. **`styleSheets`**  
    返回文档中所有样式表的列表。
    ```js
    const stylesheets = document.styleSheets;
    ```

29. **`createStyleSheet()`**  
    创建并添加新的样式表。
    ```js
    const newStyleSheet = document.createStyleSheet();
    ```

### **滚动与位置**
30. **`scrollTo(x, y)`**  
    滚动文档到指定位置。
    ```js
    document.scrollTo(0, 500);
    ```

31. **`scrollingElement`**  
    返回当前文档中可滚动的元素。
    ```js
    const scrollElem = document.scrollingElement;
    ```

这些方法提供了广泛的功能来操作和查询HTML文档，允许开发人员动态修改页面内容、样式、布局、事件监听等。****