<h3 style='text-align:center'>事件深入应用、鼠标滚轮和COOKIE</h3>

####  拖拽
1. 拖拽的时候，如果有文字被选中，会产生问题
    原因：当鼠标按下的时候，如果页面中有文字被选中，那么会触发浏览器默认拖拽文字的效果
    解决：
        标准：阻止默认行为
        非标准ie：全局捕获
    拖拽图片会有问题，原因，解决的办法同上

2. 全局捕获
    设置全局捕获 ，当我们给一个元素设置全局捕获以后，那么这个元素就会监听后续发生的所有事件，当有事件发生的时候，就会被当前设置了全局捕获的元素所触发

    ie : 有，并且有效果
    ff : 有，但是没效果
    chrome : 没有
    
3. 应用
    限定范围、磁性吸附、碰撞检测、拖拽改变元素大小、滚动条

#### 鼠标滚轮事件
1. 兼容性、鼠标滚轮滚动方向
    ie/chrome : onmousewheel
        event.wheelDelta
            上：120
            下：-120
        
    firefox : DOMMouseScroll 必须用addEventListener
        event.detail
            上：-3
            下：3
            
    return false阻止的是  obj.on事件名称=fn 所触发的默认行为
    addEventListener绑定的事件需要通过event下面的preventDefault();
    ```
    var oDiv = document.getElementById('div1');

    oDiv.onmousewheel = fn;//非firefox
    if (oDiv.addEventListener) {//firefox
        oDiv.addEventListener('DOMMouseScroll', fn, false);
    }

    function fn(ev) {
        var ev = ev || event;
        var b = true;  
        if (ev.wheelDelta) {
            b = ev.wheelDelta > 0 ? true : false;
        } else {
            b = ev.detail < 0 ? true : false;
        }

        if ( b ) {this.style.height = this.offsetHeight - 10 + 'px';
        } else {this.style.height = this.offsetHeight + 10 + 'px';
        }
        
        //取消浏览器的默认行为
        if (ev.preventDefault) {
            ev.preventDefault();
        }
        return false;   
    }
    ```

#### cookie
1. cookie : 存储数据，当用户访问了某个网站（网页）的时候，我们就可以通过cookie来像访问者电脑上存储数据
    + 不同的浏览器存放的cookie位置不一样，也是不能通用的
    + cookie的存储是以域名形式进行区分的
    + ookie的数据可以设置名字的
    + 一个域名下存放的cookie的个数是有限制的，不同的浏览器存放的个数不一样
    + 每个cookie存放的内容大小也是有限制的，不同的浏览器存放大小不一样

2. 我们通过document.cookie来获取当前网站下的cookie的时候，得到的字符串形式的值，他包含了当前网站下所有的cookie。他会把所有的cookie通过一个分号+空格的形式串联起来

3. 如果我们想长时间存放一个cookie。需要在设置这个cookie的时候同时给他设置一个过期的时间
cookie默认是临时存储的，当浏览器关闭进程的时候自动销毁

```
window.onload = function() {
    
    var oUsername = document.getElementById('username');
    var oLogin = document.getElementById('login');
    var oDel = document.getElementById('del');
    
    if ( getCookie('username') ) {
        oUsername.value = getCookie('username');
    }
    
    oLogin.onclick = function() {
        
        alert('登陆成功');
        setCookie('username', oUsername.value, 5);
        
    }
    
    oDel.onclick = function() {
        removeCookie('username');
        oUsername.value = '';
    }
    
}

function setCookie(key, value, t) {
    var oDate = new Date();
    oDate.setDate( oDate.getDate() + t );
    document.cookie = key + '=' + value + ';expires=' + oDate.toGMTString();
}

function getCookie(key) {
    var arr1 = document.cookie.split('; ');
    for (var i=0; i<arr1.length; i++) {
        var arr2 = arr1[i].split('=');
        if ( arr2[0] == key ) {
            return decodeURI(arr2[1]);
        }
    }
}

function removeCookie(key) {
    setCookie(key, '', -1);
}
```



