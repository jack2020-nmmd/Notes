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
    window:                  全局的默认窗口表现,用来设置小程序的状态栏,导航条,背景色标题等,用到查文档 ,Object  
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
    如果是引入的.WXS文件,则在文件中要暴露属性,如module.exports={要暴露的东西,键值对模式},然后引入的时候要<wxs src="相对路径" module="要给这些数据重命名"/>  ~~~如果不是引入外部的.wxs文件,只是在wxml中书写wxs标签,则这样写<wxs module="给他命名">书写的内容,也要module.expoets = {暴露的属性,键值对模式}</wxs>.如果想在.wxs文件中引入wxs文件,使用require引入,const name = require('相对路径'),注意::,<wxs> 模块只能在定义模块的 WXML 文件中被访问到。使用 <include> 或 <import> 时，<wxs> 模块不会被引入到对应的 WXML 文件中。<template> 标签中，只能使用定义该 <template> 的 WXML 文件中定义的 <wxs> 模块(相当于template标签不能使用外部引入的),wxs里面的数据类型比js少了null,undefined和symbol,wxs里面的for循环语法与js的是一样的,但是标签里面的for循环不一样,注意  
```
## 注册小程序
    每个小程序都需要在app.js中调用APP方法注册小程序实例,绑定生命周期回调函数,错误监听和页面不存在监听函数等,整个小程序只有一个APP实例,获取APP上的数据或者调用开发者注册在APP中,获取实例const APPInstance= getApp()  
    注册小程序:APP(Object Object)
## APP{Object, Object}
```css
    onLaunch:       类型function,声明周期回调,监听小程序初始化,只触发一次
    onShow:         类型function,生命周期回调,监听小程序启动或者切前台,小程序启动或者后台进来时启动
    onHide:         类型function,生命周期回调,监听小程序切后台
    onErr:          类型function,监听错误函数
    onPageNotFound  类型function,页面不存在监听函数
    onHandleRejection   类型function,未处理的promise拒绝事件监听函数
    onThemeChange   类型function,监听系统主题变化
    其他             类型any,开发者可以添加任何函数或者变量数据到Object参数中,用this可以访问
    除了第一个以外,其余都可以使用wx.onApp+on后面的函数进行绑定监听
    获取全局App属性:var appInstance = App.getApp(),注意不要在调用前使用,获取后不要私自调用生命周期函数,其实实例使用this就可以使用
```
## 注册小程序中的一个页面 Page(Object, Object),其指定页面的初始化数据,生命周期回调,事件处理函数等
```css
    data:       类型Object,页面的初始化数据
    options:    类型Object,页面的组件选项,同components构造器中的options
    onLoading:  类型function,生命周期回调,监听页面加载
    onShow:     类型function,生命周期回调,监听页面显示
    onReady     类型function,生命周期回调,监听页面初次渲染完成
    onHide      类型function,生命周期回调,监听页面隐藏
    onUnload    类型function,生命周期回调,监听页面卸载
    onPullDownRefresh       类型function,监听下拉更新
    onReachBottom           类型function,页面上拉触底事件的处理函数
    onShareAppMessage       类型function,用户点击右上角转发
    onShareTimeLine         类型function,用户转发到朋友圈
    onAddToFavorites        类型function,用户点击右上角收藏
    onPageScroll            类型function,页面滚动触发的处理函数
    onResize                类型function,页面尺寸改变时触发
    onTabItemTap            类型function,当前是 tab 页时，点击 tab 时触发
    其他                    类型any,开发者可以添加任何参数到Object参数中,在页面函数中用this可以访问
```

## 属性具体分类
### data
    页面第一次渲染的数据是由data提供的,页面加载时,data将会以JSON字符串的形式由逻辑层传至渲染层,所以data中的数据必须是可以转成JSON的类型,如数组,字符串,对象布尔值,数字.渲染层接着通过WXML对数据进行绑定,书写与VUE的是一样的,只不过它是对象模式
### 页面事件处理函数
    onPullDownRefresh()监听用户下拉刷新,需要在App.js中配置开启enablePullDownRefresh,接着可以通过wx.startPullDownRefresh触发下拉刷新,调用后触发下拉刷新动画,与用户手拉刷新一样,当处理完数据刷新后,wx.stopPullDownRefresh可以停止当前页面的下拉刷新
### 监听用户滑动页面的刷新
    onPageScroll(Object, Object),属性scrollTop,是number类型表示页面在垂直方向已经滚动的距离(px),此方法在需要的时候在定义,以减少不必要的事件派发对渲染层-逻辑层通信的影响,且不要 在这方法大量触发setdata等引起逻辑层-渲染层通信的操作,尤其每次传输大量数据会影响通信效率
### 进行收藏onAddToFavorites(Object object)
    此事件处理函数需要 return 一个 Object，用于自定义收藏内容,返回的对象参数如:title(默认页面标题),imageURL(自定义图片,默认页面截图),query(自定义query字段,默认当前页面的query)
```css
    Page({
        onAddToFavorites(res) {
            // webview 页面返回 webViewUrl
            console.log('webViewUrl: ', res.webViewUrl)
            return {
            title: '自定义标题',
            imageUrl: 'http://demo.png',
            query: 'name=xxx&age=xxx',
            }
        }
    })
```
### onShareAppMessage(Object object)
    注意：只有定义了此事件处理函数，右上角菜单才会显示“转发”按钮
```css
    Page({
        onShareAppMessage() {
            const promise = new Promise(resolve => {
            setTimeout(() => {
                resolve({
                title: '自定义转发标题'
                })
            }, 2000)
            })
            return {
            title: '自定义转发标题',
            path: '/page/user?id=123',
            promise 
            }
        }
    })
```
### onShareTimeline()
    注意：只有定义了此事件处理函数，右上角菜单才会显示“分享到朋友圈”按钮,事件处理函数返回一个 Object，用于自定义分享内容，不支持自定义页面路径,返回的Object与收藏的一样
### 组件事件处理函数
    Page 中还可以定义组件事件处理函数。在渲染层的组件中加入事件绑定，当事件被触发时，就会执行 Page 中定义的事件处理函数
```css
    <view bindtap="viewTap"> click me </view>
    Page({
        viewTap: function() {
            console.log('view tap')
        }
    })
```
## 属性
    Page.route是显示当前路径(this.route)
```css
    Page({
        onShow: function() {
            console.log(this.route)
        }
    })
```
## Page.prototype.setData(Object data, Function callback)
    setData 函数用于将数据从逻辑层发送到视图层（异步），同时改变对应的 this.data 的值（同步）
    参数data是本次要改变的数据,callback是setData引起的界面更新渲染完毕后的回调函数
    Object 以 key: value 的形式表示，将 this.data 中的 key 对应的值改变成 value。
    其中 key 可以以数据路径的形式给出，支持改变数组中的某一项或对象的某个属性，如 array[2].message，a.b.c.d，并且不需要在 this.data 中预先定义。
    注意：
    直接修改 this.data 而不调用 this.setData 是无法改变页面的状态的，还会造成数据不一致(与react相似)。
    仅支持设置可 JSON 化的数据。
    单次设置的数据不能超过1024kB，请尽量避免一次设置过多的数据。
    请不要把 data 中任何一项的 value 设为 undefined ，否则这一项将不被设置并可能遗留一些潜在问题。
## 页面间通信
    如果一个页面由另一个页面通过 wx.navigateTo 打开，这两个页面间将建立一条数据通道：
    被打开的页面可以通过 this.getOpenerEventChannel() 方法来获得一个 EventChannel 对象；
    wx.navigateTo 的 success 回调中也包含一个 EventChannel 对象。
    这两个 EventChannel 对象间可以使用 emit 和 on 方法相互发送、监听事件(事件总线)。
## getCurrentPages()获取当前页面栈。数组中第一个元素为首页，最后一个元素为当前页面。onLaunch的时候还不能调用,因为当时还没生成

## 自定义组件
    https://developers.weixin.qq.com/miniprogram/dev/reference/api/Component.html
## Behavior(Object object)
    注册一个 behavior，接受一个 Object 类型的参数。
```css
    module.exports = Behavior({
        behaviors: [],
        properties: {
            myBehaviorProperty: {
            type: String
            }
        },
        data: {
            myBehaviorData: {}
        },
        attached: function(){},
        methods: {
            myBehaviorMethod: function(){}
        }
    })
```

## 模块化
```css
    可以将一些公共的代码抽离成为一个单独的 js 文件，作为一个模块。模块只有通过 module.exports 或者 exports 才能对外暴露接口。
```

    require(string path)
    引入模块。返回模块通过 module.exports 或 exports 暴露的接口。
```css
    // common.js
    function sayHello(name) {
        console.log(`Hello ${name} !`)
    }
    function sayGoodbye(name) {
        console.log(`Goodbye ${name} !`)
    }
    module.exports.sayHello = sayHello//使用module.exports来暴露对象属性,让require可以获取(推荐使用这个)
    exports.sayGoodbye = sayGoodbye //module.exports的引入,和上面的一样
    //引入
    var common = require('common.js')
    Page({
        helloMINA: function() {
            common.sayHello('MINA')
        },
        goodbyeMINA: function() {
            common.sayGoodbye('MINA')
        }
    })
    /* 引入插件 */
    requirePlugin(string path)
    引入插件。返回插件通过 main 暴露的接口。参考 使用插件 - js 接口
```
## 基础功能
    wx  小程序 API 全局对象，用于承载小程序能力相关 API。具体请参考小程序 API 参考文档。
    wx.env  小程序环境变量对象wx.env.USER_DATA_PATH    文件系统中的用户目录路径
    输出函数console
```css
    console.debug()
    向调试面板中打印 debug 日志
    console.log()
    向调试面板中打印 log 日志
    console.info()
    向调试面板中打印 info 日志
    console.warn()
    向调试面板中打印 warn 日志
    console.error()
    向调试面板中打印 error 日志
    console.group(string label)
    在调试面板中创建一个新的分组。随后输出的内容都会被添加一个缩进，表示该内容属于当前分组。调用 console.groupEnd之后分组结束。
    console.groupEnd()
    结束由 console.group 创建的分组
```
    定时器setTimeout(function callback, number delay, any rest)设定一个定时器。在定时到期以后执行注册的回调函数,这个number是定时器定时器的编号,可以传递给clearTimeOut来取消定时器,其余和js的一样
    setInterval也是一样
## behavior的使用就和Vue的混入一样
```css
    module.exports = Behavior({
        data: {
            sharedText: 'This is a piece of data shared between pages.'
        },
        methods: {
            sharedMethod: function() {
            this.data.sharedText === 'This is a piece of data shared between pages.'
            }
        }
    })
// page-a.js
    var myBehavior = require('./my-behavior.js')
    Page({
        behaviors: [myBehavior],//注册behaviors
        onLoad: function() {
            this.data.sharedText === 'This is a piece of data shared between pages.'
        }
    })
```
## 使用 Component 构造器用于定义组件，调用 Component 构造器时可以指定组件的属性、数据、方法等
## 路由navigateTo, redirectTo 只能打开非 tabBar 页面。
    <navigator open-type="navigateTo"/>
注意:
    switchTab 只能打开 tabBar 页面。
    reLaunch 
    页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
    调用页面路由带的参数可以在目标页面的onLoad中获取。




## 小程序API
小程序 API 有以下几种类型：
### 事件监听 API
```css
    以 on 开头的 API 用来监听某个事件是否触发，如：wx.onSocketOpen，wx.onCompassChange 等。
    这类 API 接受一个回调函数作为参数，当事件触发时会调用这个回调函数，并将相关数据以参数形式传入。
    wx.onCompassChange(function (res) {
        console.log(res.direction)
    })
```  
### 同步 API
```css
    以 Sync 结尾的 API 都是同步 API， 如 wx.setStorageSync，wx.getSystemInfoSync 等。此外，也有一些其他的同步 API，如 wx.createWorker，wx.getBackgroundAudioManager 等，详情参见 API 文档中的说明。
同步 API 的执行结果可以通过函数返回值直接获取，如果执行出错会抛出异常。
    try {
        wx.setStorageSync('key', 'value')
    } catch (e) {
        console.error(e)
    }
```
### 异步 API
```css
    大多数 API 都是异步 API，如 wx.request，wx.login 等。这类 API 接口通常都接受一个 Object 类型的参数，这个参数都支持按需指定以下字段来接收接口调用结果：
    Object 参数说明:
    参数名	    类型	必填	说明
    success	    function	否	接口调用成功的回调函数
    fail	    function	否	接口调用失败的回调函数
    complete	function	否	接口调用结束的回调函数（调用成功、失败都会执行）
    其他	    Any	-	接口定义的其他参数

    回调函数的参数
    success，fail，complete 函数调用时会传入一个 Object 类型参数，包含以下字段：
    属性	    类型	说明
    errMsg	    string	错误信息，如果调用成功返回 ${apiName}:ok
    errCode	    number	错误码，仅部分 API 支持，具体含义请参考对应 API 文档，成功时为 0。
    其他	    Any	    接口返回的其他数据
    wx.login({
        success(res) {
            console.log(res.code)
        }
    })
```
## 异步API返回promise
    基础库 2.10.2 版本起，异步 API 支持 callback & promise 两种调用方式。当接口参数 Object 对象中不包含 success/fail/complete 时将默认返回 promise，否则仍按回调方式执行，无返回值。
注意事项
    部分接口如 downloadFile, request, uploadFile, connectSocket, createCamera（小游戏）本身就有返回值， 它们的 promisify 需要开发者自行封装。
    当没有回调参数时，异步接口返回 promise。此时若函数调用失败进入 fail 逻辑， 会报错提示 Uncaught (in promise)，开发者可通过 catch 来进行捕获。
    wx.onUnhandledRejection 可以监听未处理的 Promise 拒绝事件。
```css
    // callback 形式调用
    wx.chooseImage({
        success(res) {
            console.log('res:', res)
    }
})
    // promise 形式调用
    wx.chooseImage().then(res => console.log('res: ', res))
```