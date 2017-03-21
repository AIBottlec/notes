<h3 style="text-align: center;">js基础视频6-10</h3>

#### JavaScript中的作用域
> 域：空间、范围、区域
> 作用：读、写
> 作用域的标志是，先解析后执行

1. 全局作用域(script)：
自上而下
1. 函数作用域(函数)：
由里向外

>浏览器
>"js解析器"

1. 找一些东西 var/function/参数
    a = ...
    所有的变量，在正式运行代码之前，都提前赋了一个值：未定义
    
    fn1 = function fn1(){ alert(2); }
    所有的函数，在正式运行代码之前，都是整个函数块

    遇到重名的：只留一个
    变量和函数重名了，就只留下函数

2. 逐行解析代码
    表达式：= + - * / % ++ -- ! 参数……
    表达式可以修改预解析的值！

*练习1*：
<pre style="height: 500px;"><code>alert(a);//function a(){alert(4)；}
var a = 1;
alert(a);//1
function a(){
    alert(2);
}
alert(a);//1
var a = 3;
alert(a);//3
function a(){
    alert(4);
}
alert(a);//3
/*解析过程：
    1. 预解析
        --------------1.预解析开始--------------
        第一步：var a = undefined;
        第二步：a = function(){alert(2);}
        第三步：变量和函数重名了，就只留下函数
        a = function a(){alert(2);}
        第四步：var a = undefined,留下第三步的函数
        第五步：a = function a(){alert(4);}
        第六步：顺序执行，下面的函数覆盖上面的函数，此时，预解析只留下function a(){alert(4)；}
        --------------1.预解析结束--------------
        第八步：a=1
        十一步：a=3覆盖原有值
    2. 逐行解析代码
        表达式：= + - * / % ++ -- ! 参数……
        表达式可以修改预解析的值！
        --------------2.执行代码开始--------------
        第一行：alert(a)
        第七步：读到第二行：a=1，修改预解析器中的a
        第九步：遇到函数的声明，不是表达式，函数继续向下执行
        第十步：遇到a=3，表达式覆盖
        --------------2.执行代码结束--------------
*/</code></pre>

*练习二*
函数执行过程中，优先访问和设置局部的变量，即使是undefined；如果函数内部没有对应变量，函数会随着作用域链寻找外层对应变量。
<pre style="height: 240px"><code>var n = 10;
function fn1() {
    alert(n);
    var n = 20;
}
fn1();//undefined
alert(n);//10
/*解析过程：
    1.预解析
        --------------1.预解析开始--------------
        第一步：var n=...
        第二步：function fn1(){alert(n);var n = 20;}
        
        --------------1.预解析结束--------------
        第四步：给n赋值为n=4；
    2.逐行解析代码
        表达式：= + - * / % ++ -- ! 参数……
        表达式可以修改预解析的值！
        --------------2.执行代码开始--------------
        第三步：n=10
        第五步：继续读代码，遇到函数声明，继续到函数调用，重新开解析器，解析函数内容；
            一、预解析：
              --------------1.预解析开始--------------
                1、n = ...
              --------------1.预解析结束--------------
                4、n = 20；
            二、逐行解析代码
                2、alert(n);//undefined
                3、n=20;
        --------------2.执行代码结束--------------
*/</code></pre>

<pre><code>var n = 10;
function fn1() {
    alert(n);
    n = 20;
}
fn1();//10
alert(n);//20</code></pre>

<pre><code>var n = 10;
function fn1(n) {
    alert(n);
    n = 20;
}
fn1();//undefined
alert(n);//10</code></pre>

<pre><code>var n = 10;
function fn1(n) {
    alert(n);
    n = 20;
}
fn1(n);//10
alert(n);//10</code></pre>

>不要在if、for中定义变量，FF浏览器存在兼容性问题
>如何访问函数的局部变量（目前有两种方法）

<code><pre>var temp = 0;
function test() {
    var a ="蓝宝石";
    temp = a;
}
test();
alert(temp)</code></pre>

<pre><code>function test() {
    var a ="蓝宝石";
    fn(a);
}
test();

function fn(temp){
    alert(temp);
}</code></pre>

#### 真假的问题：
> 数据类型-数字（NaN）、字符串、布尔、函数、对象（elem、[]、{}、null）、未定义
> 真：非0的数字、非空字符串、true、函数、能找到的元素、[]、{}
> 假：0、NaN、空字符串''、false、不能找到的元素、null、未定义

#### return:返回值(数字、字符串、布尔、函数、对象、undefined)
> 1. 函数名+括号： fn()=>return后面的值；
> 2. 一个函数如果不写return或者return后面为空，默认返回undefined；
> 3. return 后面的代码不再执行；

#### arguments:
> 1. 实参的个数
> 2. 当函数参数个数无法确定时，用arguments
> 3. arguments的值，既可以读也可以写

#### 获取元素样式
> var a = 元素.style.属性名;只能获取到行内样式。

问题解决：使用浏览器计算后的样式：getComputedStyle(元素).属性名-不兼容IE6,7,8；元素.getCurrentStyle.属性-不兼容标准浏览器。 获取到的是计算机（浏览器）计算后的样式

注意：
1. background: url() red ……        复合样式（不要获取）
   backgroundColor                                 单一样式（不要用来做判断）
2. 不要有空格
3. 不要获取未设置后的样式：不兼容

#### 定时器
1. setInterval
放在事件中执行时，首先要清空定时器，再开启，否则多次点击会创建多个定时器。
2. setTimeOut

> 存在的问题：10定时器应用4.html
> 6函数传参4.html









