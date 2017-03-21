<h3 style="text-align: center;">js基础视11-12</h3>

#### 字符串方法

1. str.charAt(下标) 查看对应下标的字符
2. str.charCodeAt(下标) 查看对应下标字符的编码
3. String.fromCharCodeAt(编码值) 查看编码对应的字符

>练习：判断输入的是否全是数字、加密

1. str.indexOf('m') 判断str中第一次出现'm'的位置，找到后对出，返回对应下标
    str.indexOf('m',下标) 下标可以限定从第几个字符往后找
    该方法如果超出对应下标或未找到，返回-1 
    从左往右找
2. str.lastIndexOf('m') 用法和indexOf相同
    共同点：如果第二个值为负数，默认从0开始找

>  练习：查看字符串中关键字出现的位置，和次数

1. str.substring(arg1[,arg2])
    一个参数：从该参数开始截取到字符串末尾；
    两个参数：从第一个参数开始，截取到第二个参数；
    可以检测两个数，大的往后扔，小的往前扔；
    str.substring(0);相当于未截取
    str.substring(2,0);相当于str.substring(0,2)
    str.substring(-3,2);-3当成0来截取
    str.substring(2,-3);相当于str.substring(-3,2)即str.substring(0,3)
2. str.slice(arg1[,arg2])
    两个截取方法的区别：
    slice()当第二个参数值小于第一个时，不会交换位置，直接返回空
    slice(-4,-2)从倒数第四个截取到倒数第二个

>练习：给定一段文字的展开和收缩

1. str.toUpperCase()
2. str.toLowerCase()

>常用于字符串做判断的兼容性问题解决

1. str.split(‘arg’[,arg2]);字符串分割为数组
    以arg为分隔符，分割后得到的是数组（arg也被去掉了）
    str.split()：相当于不分割，直接转为数组
    str.split(‘’)：把str分割成字符数组
    str.split(‘ ’)：把str分割成数组, 以空格为分隔符
    两个参数代表以arg分割后，取arg2个

> 练习：通过文本框输入内容，通过按钮把他写在div中，并且每个字加不同的背景颜色 

#### json和数组

json对象没有length方法，遍历时只能使用for...in...

数组通过new Array(arg)定义时，如果arg为数字，直接被解析为数组长度 

var arr = [],快速清空数组的方法是，arr.length = 0或则arr = []；
但是注意，字符串中的length是不可写的

#### 数组方法

1. arr.join() 把数组转化为字符串，默认以逗号分割
    arr.join(' ') 把数组转化为字符串，以空字符分割，即去掉数组元素间原有的逗号
    arr.join('-') 把数组转化为字符串，以-分割

>  练习：浏览器窗口中的查找功能
>  

1. 添加数组元素--返回值均为数组长度
    arr.push()
    arr.unshift()//返回值不支持ie6和ie7
2. 删除方法--返回值为被删除的元素
    arr.pop()
    arr.shift()

> 应用:排队走

1. 删除、替换、添加--splice,返回被删除的元素

    arr.splice(0,2);从数组零下标开始，*删除*两个值
    arr.splice(0,1,"new");从数组零下标开始，找到两个元素，整体 *替换*为"new"
    arr.splice(1,0,"new");从数组一下标开始，添加元素"new"

> 练习：数组去重

```
var arr = ["张三","李四","李四","李四","张三","王五","张三"];

for (var i = 0; i < arr.length; i++) {
    for(var j = i+1; j < arr.length; j++){
        if(arr[i] == arr[j]){
            arr.splice(j,1);
            j--;
        }
    }
}

alert(arr);
```

1. 数组排序sort(),默认全部按字符串排序

```
// 随机排序
var arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(function(a,b) {
    return Math.random() - 0.5;
});
alert(arr);
```

1. concat()--数组连接
    arr1.concat(arr2,arr3)
2. reverse()--颠倒数组元素的位置
#### 随机函数及随机函数的推理过程


wenti:12....html
随机产生100个0-1000之间不重复的数字











