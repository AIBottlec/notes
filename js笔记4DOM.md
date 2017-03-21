<h3 style='text-align:center'>DOM</h3>

### DOM
Document Object Model
文档：html页面
文档对象：页面中的元素
文档对象模型：让js操作页面元素的定义

DOM会把文档看作是一棵树，同时定义了很多方法来操作这棵数中的每一个元素（节点）DOM节点

DOM节点的类型有很多种    12种
元素.nodeType : 只读 属性 当前元素的节点类型       
元素节点 : 1
属性节点 : 2
文本节点 : 3

#### DOM元素属性
1. 元素.childNodes 只读 属性 子节点列表集合
    标准下：包含了 *文本和元素类型* 的节点，也会包含非法嵌套的子节点
    非标准下：只包含 *元素类型* 的节点，ie7以下不会包含非法嵌套子节点
    childNodes只包含一级子节点，不包含后辈孙级以下的节点

2. 元素.children  只读 属性 子节点列表集合
    标准下：只包含 *元素类型* 的节点
    非标准下：只包含 *元素类型* 的节点

3. 元素.attributes : 只读 属性 属性列表集合

4. 元素.firstChild 只读 属性 第一个子节点
    标准下，firstChild会包含文本类型的节点
    非标准下，firstChild只包含元素节点
    元素.firstElementChild : 只读 属性 标准下，获取第一个元素类型的子节点
    
    获取元素第一个元素节点的方法如下：
```
/*if ( oUl.firstElementChild ) {
    oUl.firstElementChild.style.background = 'red';
} else {
    oUl.firstChild.style.background = 'red';
}*/
/*或者*/
var oFirst = oUl.firstElementChild || oUl.firstChild;
oFirst.style.background = 'red';
```
    但是上述方法，有时会出现问题，比如，父节点的子节点为空时：oFirst找到的是文本节点，设置style属性会报错。
```
var oUl = document.getElementById('ul1');
//alert(oUl.firstElementChild)//null
var oFirst = oUl.firstElementChild || oUl.firstChild;
//alert(oFirst)//文本节点
/*if ( oFirst ) {
    oFirst.style.background = 'red';
} else {
    alert( '没有子节点可以设置' );
}*/
//解决办法如下：
if ( oUl.children[0] ) {
    oUl.children[0].style.background = 'red';
} else {
    alert( '没有子节点可以设置' );
}
```
    类似用法的DOM属性：
    元素.lastElementChild || 元素.lastChild 最后一个子节点
    元素.nextElementSibling || 元素.nextSibling 下一个兄弟节点
    元素.previousElementSibling || 元素.previousSibling 上一个兄弟节点

5. 元素.parentNode 只读 属性 当前节点的父级节点

6. 元素.offsetParent 只读 属性 离当前元素最近的一个有定位属性的父节点
    如果没有定位父级，默认是body
    ie7以下，如果当前元素没有定位默认是body，如果当前元素有定位则是html
        如果父级有定位： 当前元素有定位，则是定位的父级元素；当前元素没有定位是body ？
    ie7以下，如果当前元素的某个父级触发了layout，那么offsetParent就会被指向到这个触发了layout特性的父节点上
7. offsetLeft/offsetTop
    封装获取当前元素到页面的left、top值
    。。。。。。

#### DOM元素的创建、删除、替换
1. 父级.appendChild(要添加的元素) 方法 追加子元素
2. 父级.insertBefore(新的元素，被插入的元素) 方法 在指定元素前面插入一个新元素
    在ie下如果第二个参数的节点不存在，会报错
    在其他标准浏览器下如果第二个参数的节点不存在，则会以appendChild的形式进行添加
    ```
    if(oUl.childNodes[0]){
        oUl.insertBefore(oLi,oUl.childNodes[0]);
    }else{
        oUl.appendChild(oLi);
    }
    ```
    
3. 父级.removeChild(要删除的元素); 删除元素
4. 父级.replaceChild(新节点，被替换节点) 替换子节点
appendChild,insertBefore,replaceChild都可以操作动态创建出来的节点，也可以操作已有节点,且都会剪切被操作元素

> appendChild()和innerHTML比较：
> 1. 在执行速度的比较上，使用appendChild比innerHTML要快，特别是内容包括html标记时，appendChild明显要快于innerHTML，因为innerHTML在铺到页面之前还要对父元素的全部内容进行解析，当包含html标记过多时，innerHTML速度会明显变慢。
> 2. 如果appendChild的参数是页面存在的一个元素，则执行后原来的元素会被移除，如document.getElement("a").appendChild(document.getElementByIdx("b"))，执行后，b元素会先被移除，然后再添加到a中。
> 3. 通过appendChild添加到页面的是dom对象，返回的也是dom对象，可以通过dom对象访问获取元素的各种属性，而innerHTML则是纯字符串，不能获取内部元素的属性。
> 4. 在使用上innerHTML比appendChild要方便，特别是创建的节点属性多，同时还包含文本的时候。
但是如果数据量较大且对性能有所要求时，还是应该使用appendChild。

#### getElementsByClassName()
```
window.onload = function(){
    // var oUl = document.getElementsByTagName("ul");//error
    /*var oBox = document.getElementsByClassName("box");
    oBox[0].style.backgroundColor = "pink"*/


    /***************************getElementsByClassName()实现*************/
    var oUl = document.getElementsByTagName("ul")[0];
    var oBox = getElementsByClassName(oUl,"box");

    console.log(oBox);

    for (var i = oBox.length - 1; i >= 0; i--) {
        oBox[i].style.background = "pink";
    }

    function getElementsByClassName(parent,claName){
        var aEles = parent.getElementsByTagName("*");
        var arr = [];
        for (var i = aEles.length - 1; i >= 0; i--) {
            var aClassName = aEles[i].className.split(' ');
            for (var j = aClassName.length - 1; j >= 0; j--) {
                if(aClassName[j] == claName){
                    arr.push(aEles[i]);
                    break;
                }
            }
        }
        return arr;
    }
}
```
#### 给元素添加或删除class
```
window.onload = function(){
    /*存在的问题：当原有class和新加的相同时，有两个相等的class*/
    /*var oDiv = document.getElementById("div1");
    oDiv.className +=' '+ "box2";*/


    //问题解决，判断是否存在，存在，返回-1
    var oDiv = document.getElementById("div1");
    addClass(oDiv,"box2")
    function addClass(obj,className){
        if(obj.className == ' '){//为空
            obj.className = className;
        }else{
            var arrClassName =  obj.className.split(' ');
            var aIndex = classIndexOf(arrClassName,className);
            if(aIndex == -1){
                oDiv.className +=' '+ className;
            }
        }
    }
    function classIndexOf(arr,v){
        for (var i = arr.length - 1; i >= 0; i--) {
            if(arr[i] == v){
                return i;
            }
        }
        return -1;
    }
}
```

```
window.onload = function(){
    var oDiv = document.getElementById("div1");
    removeClass(oDiv,"box2")
    function removeClass(obj,className){
        if(obj.className == ' '){//为空
            return;
        }else{
            var arrClassName =  obj.className.split(' ');
            var aIndex = classIndexOf(arrClassName,className);
            if(aIndex != -1){
                arrClassName.splice(aIndex ,1);
                obj.className = arrClassName.join(' ');
            }
        }
    }
    function classIndexOf(arr,v){
        for (var i = arr.length - 1; i >= 0; i--) {
            if(arr[i] == v){
                return i;
            }
        }
        return -1;
    }
}
```
#### DOM中的表格操作
注意：在table中，浏览器会自动给tr外包一对tbody标签，所以直接操控子元素时，注意考虑这层标签。
在层级关系较深时，直接操作子元素或父元素不容易分析，所以DOM中提供了单独操作table元素的属性：
tHead、tBodies、tFoot、rows、cells
```
<body>
    <table id="tab" width="100%" border="1" cellspacing="0" cellpadding="5">
        <tr>
            <td>alyss</td>
            <td>18</td>
            <td>女</td>
            <td>本科</td>
        </tr>
        <tr>
            <td>jous</td>
            <td>28</td>
            <td>男</td>
            <td>本科</td>
        </tr>
    </table>
</body>
</html>
<script>
    window.onload = function(){
        var oTab = document.getElementById("tab");
        // console.log(oTab.children[0].children[1].children[0].innerHTML)

        var aTabArr = oTab.tBodies[0].rows[1].cells[0].innerHTML;
        console.log(aTabArr);
    }
</script>
```
#### DOM中的表单操作
1. name属性
    var form = document.forms[name];
    var ele = form.eleName;
2. onchange()
3. 表单事件
    onsubmit()
    onreset()




