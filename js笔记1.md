1. display:none  元素在页面不显示，不占有页面空间
    
    overflow:hidden
    
    visibity:hiddden|visible 元素在页面不显示，仍然占有空间。

2. js中的属性操作注意事项：
    
    js中不允许出现-，如font-size,margin-top...在设置相应属性值时，都要改为驼峰命名fontSize、marginTop...

    当修改多个属性时，我们可以写在class中，修改class实现效果。但是js中，属性class为保留字，所以在设置元素的class属性时，应写className。

    js中，不要去获取元素的相对路径地址，以及颜色值和背景，因为各浏览器的处理方式不同；此外，innerHTML有兼容性问题

    float的问题，直接写在js中，会有兼容性问题，有两种解决办法：1.写在css中，通过添加和删除对应的类修改；2.IE：oDiv.style.styleFloat = "",非IE：oDiv.style.cssFloat="";

    input的type属性修改的问题，不能兼容ie678，解决办法，直接写多个，显示隐藏

    元素.style.属性名 = 值,js中.后面的值无法动态修改，解决办法，元素.style[属性名]=值。 

    - js的属性操作二:cssText,与直接操作属性的不同在于，他是直接覆盖原有的行内属性，oDiv.style.width是在原有基础添加
```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<style>#div1{width: 100px;height: 100px;border:1px solid red;}</style>
<body>
<div id="div1" style="background: red"></div>
<button>按钮</button>
</body>
<script>
    var oDiv = document.getElementById("div1");
    var oBtn = document.getElementsByTagName("button")[0];
    /*oDiv.onclick = function(){
        oDiv.style.width = "200px";
    }
    oBtn.onclick = function(){
        oDiv.style.height = "300px";
    }*/

    oDiv.onclick = function(){
        oDiv.style.width = "200px";
    }
    oBtn.onclick = function(){
        oDiv.style.cssText ="height:300px";
    }
</script>
</html>
```

3. 获取元素的方式一：document.getElementById("id")
   
    获取元素的方式二：document.getElementsByTagName("标签名")

    获取元素的方式三：对于页面上只存在一个标签的元素可以通过document直接获取，比如：document.title = 123；但是body添加元素需要innnerHTML，如上代码。

        第二个是类数组，具有数组的方法，使用时必须加下标[]。
        第一个只可以通过 document访问，第二个可以是他的父级元素。
        第一个为静态方法；第二个为动态方法。（第一个不可以查看通过js动态添加的元素）
    
```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<script>
window.onload = function () {

    var aBtn = document.getElementById("test");
    var aBtn1 = document.getElementsByTagName("input");
    
    document.body.innerHTML = '<input id="test" type="button"  value="按钮1"/><input type="button"  value="按钮2"/><input type="button"  value="按钮3"/> '
    

    alert(aBtn);//null
    alert(aBtn1[0]);//object HTMLInputElement
}
</script>
<body>
</body>
</html>
```
4. 性能问题举例
    for循环中，如果需要数组长度，可以在循环外赋值为变量后使用，否则每次循环开始都会计算数组长度。
    在循环内，document.getElementBY**不要出现，否则，每次都会找对应元素，解决办法是定义变量。
5. 关于this   
```
<!DOCTYPE html>
<html>
<head>
    <title></title>
</head>
<body>
    <input type="button" value="btn1" id="btn1">
    <div onclick="test()">我就是想看看1</div><!--5.***window-->
    <br>
    <div onclick="test(this)">我就是想看看2</div><!--6.***传参*为object HTMLInputElement-->
    <br>
    <div onclick="alert(this)">我就是想看看3</div><!--7.***object HTMLInputElement-->
</body>
<script>
    var oBtn1 = document.getElementById("btn1");
    
    function test (a){
        alert(this);
        // alert("canshu")
        // alert(a);
    }

    //test();//1.***window

    oBtn1.onclick = function(){
        // alert(this);//2.***object HTMLInputElement
        test()//3.***window；（如果此处传参，结果为bject HTMLInputElement）
    }

    // oBtn1.onclick = test;//4.***object HTMLInputElement
</script>
</html>
```
6. 自定义属性
7. 判断一个值是小数还是整数
    ```
    var num = '200.45';
    if( parseInt(num) == parseFloat(num) ){
        alert( num + '是整数' );
    }else{
        alert( num + '是小数' );
    }
    ```
8. js的数据类型
9. js中的NaN
    NaN：not a number。不是数字的数字类型。alert(typeof(NaN))//number
    一旦程序中出现了NaN，一定是程序中进行了非法的运算操作。
    NaN的boolean是false。
    NaN和自己都不相等。
10. isNaN()--- 判断某些值是不是数字,,,不喜欢数字、讨厌数字,,,只要遇到数字，就返回false。
    isNaN本质上就是利用Number对象对参数进行转换，如果转换为数字，就返回false,反之返回true

    alert( isNaN( function(){ alert(1) } ) );//true

    alert( isNaN('250') ); //false,内部实现靠的是Number，凡是Number()可以转化的，都返回false.
    Number()  '250' => 250 => false

    alert( isNaN( [] ) );//false

    alert(isNaN(true));//false







