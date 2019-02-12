# 微信小程序第三周总结  
1.事件  
```
<!-- 事件 -->
<view id="outter" style="border:1px solid red;" bindtap='outtertap'>
  outter view
  <view id="middle" style="border:1px solid green;"  bindtap='middletap'>
    middle view
    <view id="inner"style="border:1px solid yellow;" bindtap="innertap">
      inner view
    </view>
  </view>
</view>  

outtertap:function(event){
    console.log(event)
    console.log("触发了outter的事件")
  },
  middletap:function(){
    console.log("触发了middle的事件")
  },
  innertap: function () {
    console.log("触发了inner的事件")
  },
```  
2.事件分类  
**bind事件绑定不会阻止冒泡事件向上冒泡，catch事件绑定可以阻止冒泡事件向上冒泡。**  
冒泡事件：当一个组件上的事件被触发后，该事件会向父节点传递。  
非冒泡事件：当一个组件上的事件被触发后，该事件不会向父节点传递。  
```
bindtap == bind:tap
cathchtap
catchtouchstart == catch:touchstart
bindtouchmove
```  
3.**除上表之外的其他组件自定义事件如无特殊声明都是非冒泡事件，如<form/>的submit事件，<input/>的input事件，<scroll-view/>的scroll事件，(详见各个组件)**  
4.wmxl:  
(数据绑定)  
运算符绑定  
```
<!-- index.wxml -->
<view hidden='{{flag?ture:false}}'>
    hidden
</view>
<!-- index.js -->
page({
    data:{
    flage:false
    }
})
```  
属性绑定  
```
<!--index.wxml-->
<view>
    <text data-name="{{theName}}">
    </text>
</view>
<!--index.js-->
page({
    data:{
        theName:"jack"
    }
})
<!--文本绑定-->

<view>
    <text>{{message}}</text>
</view> 

page({
    data:{
    message:"hello world"
    }
})
```  
5.列表渲染  
wx:for  
item:变量名  
index:下表变量名 **从0开始** 
使用 wx:for-item 可以指定数组当前**元素**的变量名  
使用 wx:for-index 可以指定数组当前**下标**的变量名
九九乘法表  
```
<view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="i">
  <view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="j">
    <view wx:if="{{i <= j}}">
      {{i}} * {{j}} = {{i * j}}
    </view>
  </view>
</view>
```  
6.条件渲染  
```
<view>今天吃什么？</view>
<view wx:if="{{condition==1}}">🥟</view>
<view wx:elif="{{condition==2}}">🍚</view>
<view wx:else>🍜</view>

page({
  data:{
    condition:Math.floor(Math.random()*3+1)
  }
})
```  
7.**一般来说，wx:if 有更高的切换消耗而 hidden 有更高的初始渲染消耗。因此，如果需要频繁切换的情景下，用 hidden 更好，如果在运行时条件不大可能改变则 wx:if 较好。**  
8.  is 属性可以使用 Mustache 语法，来动态决定具体需要渲染哪个模板//is="tempItem"  
    data:然后将模板所需要的 data 传入  
```
<template name="tempItem">
    <view>
        <view>收件人:{{name}}</view>
        <view>联系方式:{{phone}}</view>
        <view>地址:{{address}}</view>
    </view>
</template>
<template is="tempItem" data="{{...itemx}}"></template>  

page({
  itemx:{
            name:"XXX",
            phone:"XXXXXXXXXX",
            address:"四川成都"
        }
})
```  
9.wxss  
样式导入  
使用@import语句可以导入外联样式表，@import后跟需要导入的外联样式表的相对路径，用;表示语句结束  
```  
<!--itemcommon.wxss-->
@import "item.wxss";

<!--item.wxss-->
.rpx{
    width: 400rpx;
    height: 200rpx;
    background-color: greenyellow
}
```  
内联样式  
静态的样式统一写到 class 中,style 接收动态的样式  
class：用于指定样式规则，其属性值是样式规则中类选择器名(样式类名)的集合，样式类名不需要带上,样式类名之间用空格分隔.  
全局样式与局部样式  
定义在 app.wxss 中的样式为全局样式，作用于每一个页面。在 page 的 wxss 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 app.wxss 中相同的选择器。  
10.wxs模块  
每一个 .wxs 文件和 <wxs> 标签都是一个单独的模块.  
每个模块都有自己独立的作用域。即在一个模块里面定义的变量与函数，默认为私有的，对其他模块不可见。  
一个模块要想对外暴露其内部的私有变量与函数，只能通过 module.exports 实现。  
```
<wxs src="item.wxs" module="item" />
<view>{{item.msg}}</view>
<view>{{item.bar(item.FOO)}}</view>

var foo = "'hello world' from item.wxs";
var bar = function (d) {
    return d;
}
module.exports = {
    FOO: foo,
    bar: bar,
};
module.exports.msg = "some msg";
```
10.wxs.require函数
在.wxs模块中引用其他 wxs 文件模块，可以使用 require 函数  
```
<!--pages/tools.wxs-->

var foo = "'hello world' from tools.wxs";
var bar = function (d) {
  return d;
}
module.exports = {
  FOO: foo,
  bar: bar,
};
module.exports.msg = "some msg";  


<!--pages/logic.wxs-->

var tools = require("./tools.wxs");

console.log(tools.FOO);
console.log(tools.bar("logic.wxs"));
console.log(tools.msg);   

<!-- /page/index/index.wxml -->

<wxs src="./../logic.wxs" module="logic" />  //src的使用
```  
11.
<wxs>的标签  
module:模块名  
```
<!--wxml-->

<wxs module="foo">
  var some_msg = "hello world"; module.exports = { msg : some_msg, }
</wxs>
<view>{{foo.msg}}</view>
```  
12.
wsx的变量  
变量名的注意事项
标识符  
13.
wxs的注释  
 方法一：单行注释 //  
 方法二：多行注释 /*  */  
 方法三：结尾注释。即从 /* 













