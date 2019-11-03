# DOM

Document Object Model   -   文档对象模型


## 1、childNodes

- 获取元素节点的所有子节点（空格也算一个字符）
- IE 低版本获取到的只能获取到 元素节点，无法获取到文本节点

```html
<body>
    <div class="box">
        <p>1</p>
        <p>2</p>
        <p>3</p>
    </div>

    <script>
        let oBox = document.querySelector('.box')
        let oPs = oBox.childNodes
        
        // 空格会被解析为一个空字符串 的数组项
        console.info(oPs)   // NodeList(7) [text, p, text, p, text, p, text]
    </script>
</body>
```

###### 节点类型

![](D:\Windows 10 Documents\Desktop\节点各属性参数.png)

- 
  getAttributeNode() 方法从当前元素中通过名称获取属性节点。


提示：如果你仅想返回属性值请使用 getAttribute 方法。


```html
<body>
    <div id="box">
        <p>1</p>
        <p>2</p>
        <p>3</p>
    </div>

    <script>
        var element = document.getElementById("box");
        var attr = document.getElementById("box").getAttributeNode("id");
        var text = document.getElementById("box").firstChild;  // 即childNodes[0]

        console.log(element.nodeType);     //1
        console.log(attr.nodeType);        //2
        console.log(text.nodeType);        //3
    </script>
</body>
```

###### childNodes优化

去除childNodes获取到子节点中的空项

```js
let myChildNodes = el => {
    let child = document.querySelect(el).childNodes
    let childArr = []
    child.forEach( (item,index) => {
        if( item.nodeType === 1 ){
            childArr.push( item )
        }
    } )
    return childArr
}
```

## 2、children

- 获取 元素节点的所有 子元素节点

```html
<body>
    <div class="box">
        <p>1</p>
        <p>2</p>
        <p>3</p>
    </div>

    <script>
        let oBox = document.querySelector('.box')
        let oPs = oBox.children
        
        // 获取到的为 HTMLCollection，不能进行forEach操作
        console.info(oPs)   // HTMLCollection(3) [p, p, p]
    </script>
</body>
```

## 3、parentNode

- 获取元素节点的父元素节点


```html
<body>
    <div class="box">
        <p id="ppp">111</p>
    </div>

    <script>
        let oP = document.getElementById('ppp')
        console.info(oP.parentNode);  // <div class="box">...</div>
        
        // 获取“爷爷”节点
        console.info(oP.parentNode.parentNode)
    </script>
</body>
```

## 4、offsetParent

- 获取元素节点的最近的带有定位的父元素节点（ 若父节点都没有定位，则返回 body 节点 ）

  （包括 absolute、relative、fixed 、sticky 定位）

```html
<body>
    <div id="app" style="position: absolute;">
        <div class="box">
            <p id="ppp">111</p>
        </div>
    </div>

    <script>
        let oP = document.getElementById('ppp')
        console.info(oP.offsetParent);   // 返回 #app 节点
    </script>
</body>
```

## 5、有兼容性问题的DOM操作

### firstElementChild    

- 返回元素的第一个子元素节点

  在 IE8- 下的写法 ：  firstChild    

  **兼容写法**：

  ```js
  first = xxx.firstElementChild || xxx.firstChild
  ```

### lastElementChild    

- 返回元素的最后一个子元素节点

  在 IE8- 下的写法 ：   lastChild   

  **兼容写法**：

  ```js
  last = xxx.lastElementChild || xxx.lastChild
  ```

### previousElementSibling   

- 返回元素的上一个同级节点

  在 IE8- 下的写法 ：previousSibling    

  **兼容写法**：

  ```js
  previous = xxx.previousElementSibling || xxx.previousSibling
  ```

### nextElementSibling

- 返回元素的下一个同级节点

  在 IE8- 下的写法 ： nextSibling    

  **兼容写法**：

  ```js
  next = xxx.nextElementSibling || xxx.nextSibling    
  ```

## 6、+= 拼接会出现的问题

```html
<body>
    <div id="app">
        <p id="ppp">1111111</p>
    </div>

    <script>
        let app = document.getElementById('app')
        document.querySelector("#ppp").onclick = () => alert(1)

        // += 操作只会保留之前的html结构，不会保留之前节点上绑定的事件，导致点击p时点击弹窗事件不会触发
        app.innerHTML += `<a href="http://www.tanzhouedu.com">潭州教育</a>`
        
    </script>
</body>
```

## 7、创建和添加节点

- document.createElement( <节点> ) ：创建节点
- <父节点>.appendChild( <子节点> )   : 将子节点添加到父节点的最后
-   document.createTextNode( <文本内容> )   ：创建文本节点，等同于 innerText
-   <父节点>.insertBefore( <新创建节点> , <子节点> )   ：将新创建的节点插入指定子节点的前面
-   <父节点>.replaceChild( <新创建节点> , <子节点> )   ：使用新创建的节点 替换指定子节点
-   < 父节点>.removeChild( <子节点> )   ：删除一个节点中的指定子节点
-   <元素>.remove( )   ：删除某个节点

```html
<body>
    <div id="app">
        <p id="text">我是一段话</p>
    </div>
    
    <script>
        let app = document.getElementById('app')
        let text = document.getElementById('text')
        
        // 创建一个新的节点
        let oDiv = document.createElement('div')
        // 创建一个文本节点
        let oText = document.createTextNode('一段文本哈哈哈')
        // 将新创建的文件节点添加到新创建的div中
        oDiv.appendChild(oText)
        // 将 oDiv 添加到 #app 的最后
        app.appendChild(oDiv)
        // 删除 oDiv 节点
        oDiv.remove()
    </script>
</body>
```

