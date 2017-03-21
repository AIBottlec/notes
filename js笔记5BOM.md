<h3 style='text-align:center'>BOM和事件</h3>

### BOM
Browser Object Model
浏览器对象模型：让js操作页面元素的定义

1. open(arg1,arg2) 地址arg1默认是空白页面，打开方式arg2默认新窗口；打开一个新窗口
2. close();
    ff : 无法关闭
    chrome : 直接关闭
    ie : 询问用户
    //可以通过关闭用window.open方法打开的窗口
3. window.navigator.userAgent : 浏览器信息
4. window.location : 地址
    window.location.href = window.location内容
    window.location.search = url?后面的内容
    window.location.has h = url#后面的内容 
5. 窗口的尺寸和大小
    可视区尺寸
        document.documentElement.clientHeight
        document.documentElement.clientWidth
    滚动距离
        document.documentElement.scrollTop //非chrome
        document.body.scrollTop //chrome
        兼容性写法：
        var scrollTop = document.documentElement .scrollTop || document.body.scrollTop;
    内容高度
        scrollHeight : 内容实际宽高
    文档高度、宽度
        offsetHeight、offsetWidth
        //alert( document.body.offsetHeight );
        //ie : 如果内容没有可视区高，那么文档高就是可视区
        //alert( document.documentElement.offsetHeight );

6. 两个事件
    onscroll : 当滚动条滚动的时候触发

    var i = 0;
    window.onscroll = function() {
        document.title = i++;
    }

    //onresize : 当窗口大小发生变化的时候触发
    window.onresize = function() {
        document.title = i++;
    }

#### Event
1. 焦点事件
    焦点 :使浏览器能够区分用户输入的对象，当一个元素有焦点的时候，那么他就可以接收用户的输入。
    我们可以通过一些方式给元素设置焦点
    1.点击
    2.tab
    3.js
    不是所有元素都能够接收焦点的.能够响应用户操作的元素才有焦点
    
    onfocus : 当元素获取到焦点的时候触发
    onblur : 当元素失去焦点的时候触发
    obj.focus() 给指定的元素设置焦点
    obj.blur() 取消指定元素的焦点
    obj.select() 选择指定元素里面的文本内容

2. Event对象
    event : 事件对象 , 当一个事件发生的时候，和当前这个对象发生的这个事件有关的一些详细的信息都会被临时保存到一个指定地方-event对象，供我们在需要的调用。飞机-黑匣子

    事件对象必须在一个事件调用的函数里面使用才有内容
    事件函数：事件调用的函数，一个函数是不是事件函数，不在定义的决定，而是取决于这个调用的时候

    兼容
    ie/chrome : event是一个内置全局对象
    标准下 : 事件对象是通过事件函数的第一个参数传入
    如果一个函数是被事件调用的,那么这个函数定义的第一个参数就是事件对象
    ```
    //onmousemove : 当鼠标在一个元素上面移动的触发
    //触发频率不是像素，而是间隔时间，在一个指定时间内（很短），如果鼠标的位置和上一次的位置发生了变化，那么就会触发一次
    document.onmousemove = function(ev) {
        var ev = ev || event;

        var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;

        oDiv.style.left = ev.clientX + 'px';
        oDiv.style.top = ev.clientY + scrollTop + 'px';
        
    }
    ```

    clientX[Y] : 当一个事件发生的时候，鼠标到页面可视区的距离

3. 事件流
    +  事件冒泡：由内向外
    当一个元素接收到事件的时候，会把他接收到的所有传播给他的父级，一直到顶层window.事件冒泡机制。
    阻止冒泡 : 当前要阻止冒泡的事件函数中调用 event.cancelBubble = true;
    ```
    <style>
        div{padding: 40px;width:600px;}
        #div1{background: red}
        #div2{background: pink}
        #div3{background: skyblue;position: absolute;top:200px;}
    </style>
    <div id="div1">
        <div id="div2">
            <div id="div3"></div>
        </div>
    </div>

    <script>
        window.onload = function() {
            var oDiv1 = document.getElementById("div1");
            var oDiv2 = document.getElementById("div2");
            var oDiv3 = document.getElementById("div3");

            function fn(){
                alert(this.id)
            }
            oDiv1.onclick = fn;
            //oDiv2.onclick = fn;
            oDiv3.onclick = fn;
        }
    </script>
    ```
    
    +  事件捕获：由外向内
    ```
    var oDiv1 = document.getElementById('div1');
    var oDiv2 = document.getElementById('div2');
    var oDiv3 = document.getElementById('div3');
    function fn1() {
        alert( this.id );
    }
    /*oDiv1.onclick = fn1;
    oDiv2.onclick = fn1;
    oDiv3.onclick = fn1;*/
    
    //false = 冒泡
    //告诉div1，如果有一个出去的事件触发了你，你就去执行fn1这个函数
    /*oDiv1.addEventListener('click', fn1, false);
    oDiv2.addEventListener('click', fn1, false);
    oDiv3.addEventListener('click', fn1, false);*/
    
    //告诉div1，如果有一个进来的事件触发了你，你就去执行fn1这个函数
    /*oDiv1.addEventListener('click', fn1, true);
    oDiv2.addEventListener('click', fn1, true);
    oDiv3.addEventListener('click', fn1, true);*/
    
    oDiv1.addEventListener('click', function() {
        alert(1);
    }, false);
    oDiv1.addEventListener('click', function() {
        alert(3);
    }, true);
    oDiv3.addEventListener('click', function() {
        alert(2);
    }, false);
    //3 2 1
    ```

4. call()
    是函数下的一个方法，call方法第一个参数可以改变函数执行过程中的内部this的指向，call方法第二个参数开始就是原来函数的参数列表
    ```
    function fn1(a, b) {
        alert(this);
        alert(a + b);
    }

    //fn1();    //window
    fn1.call(null, 10, 20); //调用函数  fn1() == fn1.call()
    ```
5. 事件绑定
    + 给一个对象绑定一个事件处理函数的第一种形式
        obj.onclick = fn;
        document.onclick = fn1;
        document.onclick = fn2;   //会覆盖前面绑定fn1
    + 给一个对象的同一个事件绑定多个不同的函数
        给一个元素绑定事件函数的第二种形式
        ie：obj.attachEvent(事件名称，事件函数);
            1.没有捕获
            2.事件名称有on
            3.事件函数执行的顺序：标准ie->正序   非标准ie->倒序
            4.this指向window
        标准：obj.addEventListener(事件名称，事件函数，是否捕获);非标准的IE不支持该事件。
            1.有捕获
            2.事件名称没有on
            3.事件执行的顺序是正序
            4.this触发该事件的对象
            5.是否捕获 : 默认是false    false:冒泡 true：捕获

    兼容性写法：
    ```
    function fn1() {
        alert(this);
    }
    function fn2() {
        alert(2);
    }
    function bind(obj, evname, fn) {
        if (obj.addEventListener) {
            obj.addEventListener(evname, fn, false);
        } else {
            obj.attachEvent('on' + evname, function() {
                fn.call(obj);
            });
        }
    }

    bind(document, 'click', fn1);
    bind(document, 'click', fn2);
    ```

6. 事件取消
    + 第一种事件绑定形式的取消：eg.document.onclick = null;
    + ie : obj.detachEvent(事件名称，事件函数);
    标准 : obj.removeEventListener(事件名称，事件函数，是否捕获);
7. 键盘事件
    onkeydown(): 当键盘按键按下的时候触发
    onkeyup(): 当键盘按键抬起的时候触发
    event.keyCode : 数字类型 键盘按键的值 键值
    event.ctrlKey,shiftKey,altKey 布尔值
    当一个事件发生的时候，如果ctrl || shift || alt 是按下的状态，返回true，否则返回false
    
    小案例1：留言板,按Ctrl和回车留言

    ```
    <input type="text" id="txt">
    <ul id="ul1"></ul>
    <script>
        window.onload = function(){
            var oTxt = document.getElementById("txt");
            var oUl = document.getElementById("ul1");

            oTxt.onkeyup = function(eve){
                var eve = eve||event;
                if(this.value){
                    if(eve.keyCode == 13&&eve.ctrlKey){
                        var oLi = document.createElement("li");
                        oLi.innerHTML = this.value;
                        if(oUl.children[0]){
                            oUl.insertBefore(oLi,oUl.childNodes[0])
                        }else{
                            oUl.appendChild(oLi);
                        }
                        this.value=""
                    }
                }

            }
        }
    </script>
    ```

    小案例2：键盘控制div移动
    注意：不是所有元素都能够接收键盘事件，能够响应用户输入的元素，能够接收焦点的元素就能够接收键盘事件

    在案例2中，存在问题当按下键盘的某个键时，div会有一个停顿时间；怎么解决（可以用定时器）

8. 事件默认行为当一个事件发生的时候浏览器自己会默认做的事情
    怎么阻止？
    当前这个行为是什么事件触发的，然后在这个事件的处理函数中使用return false;

    oncontextmenu : 右键菜单事件，当右键菜单（环境菜单）显示出来的时候触发

    小案例：自定义鼠标右键菜单
    ```
    <style>
        #div1 {width:100px; height: 200px; border: 1px solid red; position: absolute; display: none;}
    </style>
    <body style="height: 2000px">
        <div id="div1"></div>
    </body>
    <script>
        window.onload = function(){
            var oDiv = document.getElementById("div1");
            document.oncontextmenu = function(eve) {
                var eve = eve||event;
                oDiv.style.display = "block";

                var scrollTop = document.documentElement.scrollTop||document.body.scrollTop;
                oDiv.style.left = eve.clientX + "px"
                oDiv.style.top = eve.clientY + scrollTop + "px"

                return false;
            }
            document.onclick = function() {
                oDiv.style.display = "none";
            }
        }
    </script>
    ```



