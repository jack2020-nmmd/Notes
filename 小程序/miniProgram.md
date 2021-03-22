# 小程序注意事情
## 最少拥有四个重要文件且没有dom对象,推荐使用flex
### *.js 
### *.wxml view结构对应html 
### *.wxss view样式对应css
### *.json view数据,json文件           
<br/>

# 小程序适配方案
## rpx:                   
                        通常移动端让我们的视觉视口等于布局视口<meta name="viewport" content="width=device-width, initial-scale=1.0"/> 设备像素比假设为2,则一个方向上一个物理像素对应0.5个css像素使用rem,但移动端有一个新单位rpx,它底层已经做了viewport封装,即实现了理想视口,1rpx = 1 物理像素 = 0.5css, 移动端还有一个较常用的单位view,设备像素比=设备像素(物理像素)/设备独立像素(css像素)
## 小程序包含一个描述整体程序的 app 和多个描述各自页面的 page,小程序主体要有三个文件:                    
###  app.js 
                        小程序的的逻辑
### app.json
                        小程序的公共配置
###   app.wxss(非必须)
                        小程序的公共样式

## 每个页面由以下四个文件组成(为了减少配置,每个页面的代码必须在同一个路径和文件)
### js
                        页面的逻辑代码文件
### wxml
                        页面结构,相当于html
### json
                        页面配置(不是必须)
### wxss
                        页面样式(非必须)

# 配置小程序
## 配置
                        小程序根目录下的 app.json 文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等,但如果页面也配置了会覆盖全局配置
### 全局配置属性
    entryPagePath:           小程序启动时显示的路径文件,默认page第一个  
    pages:                  页面的路径列表(必须),每一个代表一个页面的路径信息,每增加一个页面都需要对其进行配置,string或者[]  
    window:                  全局的默认窗口表现,用来设置小程序的状态栏,导航条,背景色标题等,用到差文档 ,Object  
    tabBar:                  底部table栏的表现,如果他是一个多标签应用,可以使用点击标签切换到响应的页面,它的颜色,文字颜色,选中文字的颜色等    
    networkTimeout:          网络超时时间Object,可以配置request,downloadFile等的超时时间等  
    workers:                 worker代码放置的目录,使用多线程时才使用,string  
    plugins:                 用到的插件Object  
    permission:              小程序接口相关配置  
    singlePage:              单页面模式相关配置  
    requireBackgroundModes:  申明需要后台运行的能力,类型为数组,支持以下的项目,audio和location定位,"requiredBackgroundModes": ["audio", "location"],开发板和体验版有用,正式版需要审核  
    usingComponents:         在此声明自定义组件变为全局组件,使用时不需再次引入  
    permissionObject:        小程序获取权限展示的用途信息,``{"pages": ["pages/index/index"],"permission": {"scope.userLocation": {"desc": "你的位置信息将用于小程序位置接口的效果展示" // 高速公路行驶持续后台定位}}}``  
    style:                  "style": "v2"可表明启用新版的组件样式  
    lazyCodeLoading:        表示按需引入,当前页面不使用的组件就不加载,性能优化  
## 页面配置  
    navigationBackgroundColor:          导航栏背景颜色  
    navigationBarTextStyle:             导航栏字体颜色  
    navigationBarTitleText:             导航标题内容  
    navigationStyle:                    导航栏样式,需要自己定制导航栏的时候使用custom值  
    backgroundColor:                    页面背景颜色  
    backgroundTextStyle:                下拉loading的样式,仅支持dark和light两个样式  
    enablePullDownRefresh:              是否开启下拉刷新  
    onReachBottomDistance:              页面上拉触底事件触发时距离底部页面距离  
    disableScroll:                      页面整体是否能上下滚动,只能在页面设置中使用,无法再app.js中设置  
# WXML语法
语法与Vue很相似,把v-if变为wx-,由于每一个页面都是一个文件夹,则一个文件夹里面的数据不需引入即可直接使用,也有插值这些,模板语法定义与插槽一样,他是使用template标签,要复用的代码写在他的里面,name属性定义引用,当使用设置好的模板时也是用template标签,但是<span style="color:red">is</span>属性对应name的值,接着模板要使用的数据用<span style="color:red">data</span>属性引用.相当于重复使用一个组件,代码复用.如果想用<span style="color:red">is</span>进行动态模板渲染,外层可以使用<span style="color:red">block</span>标签进行包囊(不会渲染任何内容),它只接受控制属性,如wx-for,wx-if这些,求出的值可以给template模板的<span style="color:red">is</span>属性确定是否渲染.模板拥有自己的作用域，只能使用 data 传入的数据以及模板定义文件中定义的 <wxs /> 模块  
<br/>
WXML提供两种方式引入,import和include  
``<import src="item.wxml"/>
<template is="item" data="{{text: 'forbar'}}"/>
``
text是item里面的数据,变为forbar数据,引入了item的模板,页面使用的模板复用  ,include标签是把除template和wxs外的文件全部引入  

## WXML的数据绑定,它的数据来自page里的data
### 可以用在
```html
    1:内容中:和Vue一样  
    2:组件属性中,需要用大括号括起来,并且外面有双引号
    <view id="item-{{id}}"> </view>
    3:控制属性,即属性值:``
<view wx:if="{{condition}}"> </view>``  
    4 :关键字:<checkbox checked="{{false}}"> </checkbox>,如果直接写check="false",他只是一个字符串  
```
5:运算:可以是三目运算符,算数运算,逻辑判断,字符串运算,数据路径运算等,<span style="color:yellow">花括号和引号之间如果有空格，将最终被解析成为字符串 </span>
### <span style="color:red">双花括号中的运算除了在内容中是不需要引号的,其余全部都是要用引号括起来.</span>
## 列表渲染
## <span style="color:red">wx:for</span>
```html
    它和Vue是一样的,也是可以嵌套,但是它的默认索引是index,默认数据名是item,所以可以直接写<view wx:for="{{array}}">{{index}}: {{item.message}}</view>,想更改可以命名可以使用``<view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">来改默认名字  
<block>标签相当于react的Fragment标签,没有实际意义,就是一个包囊块
```  
## <span style="color:red">wx:key</span>
```html
    如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 input 中的输入内容，switch 的选中状态），需要使用 wx:key 来指定列表中项目的<span style="color:red">唯一的标识符。</span>,它的值只能是字符串(字符串的话必须是item里面的属性)或者保留关键字 *this(代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字),有key可以使他们数据改变时,框架会让他们重新排序而不是重新渲染,在获取键的值时,可以直接双引号括起来,相当于结构一样,可以直接获取item里面的属性值``<switch wx:for="{{objectArray}}" wx:key="unique"> {{item.id}} </switch>相当于{{item.unique}}
```  
## switch标签,注意它和switch case语法的区别,不是一个东西 
    是一个toggle按钮  
## bindtap是一个点击button按钮实现跳转的实现函数  
## wx-if与hidden就是v-if与v-show的区别  

# WXS语法参考  
```html
    WXS（WeiXin Script）是小程序的一套脚本语言，结合 WXML，可以构建出页面的结构.WXS 与 JavaScript 是不同的语言，有自己的语法，并不和 JavaScript 一致,WXS 代码可以编写在 wxml 文件中的 <wxs> 标签内，或以 .wxs 为后缀名的文件内
```
## WXS语法
```html  
    如果是引入的.WXS文件,则在文件中要暴露属性,如module.exports={要暴露的东西,键值对模式},然后引入的时候要<wxs src="相对路径" module="要给这些数据重命名"/>  ~~~如果不是引入外部的.wxs文件,只是在wxml中书写wxs标签,则这样写<wxs module="给他命名">书写的内容,也要module.expoets = {暴露的属性,键值对模式}</wxs>.如果想在.wxs文件中引入wxs文件,使用require引入,const name = require('相对路径'),注意::,<wxs> 模块只能在定义模块的 WXML 文件中被访问到。使用 <include> 或 <import> 时，<wxs> 模块不会被引入到对应的 WXML 文件中。<template> 标签中，只能使用定义该 <template> 的 WXML 文件中定义的 <wxs> 模块,wxs里面的数据类型比js少了null,undefined和symbol,wxs里面的for循环语法与js的是一样的,但是标签里面的for循环不一样,注意  
```








