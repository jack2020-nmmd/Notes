# 视图层
## WXML在前面的文档
## WXSS
```css
    样式导入,使用@import语句可以导入外联样式表，@import后跟需要导入的外联样式表的相对路径，用;表示语句结束。
    内联样式可以和html的类名一样,也可以向Vue一样使用<view style="color:{{color}};" />,但要注意style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。
    支持的选择器没有html多,只有.class/#id/element/后代选择器/::after和::before这几个,
    也分为全局样式和局部样式
```
## WXS
    前面的md文件有写
## 事件
    事件是视图层到逻辑层的通讯方式。
    事件可以将用户的行为反馈到逻辑层进行处理。
    事件可以绑定在组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
    事件对象可以携带额外信息，如 id, dataset, touches。
### 事件的使用方式
```css
    在组件中绑定一个事件处理函数。
    如bindtap，当用户点击该组件的时候会在该页面对应的Page中找到相应的事件处理函数。
        <view id="tapTest" data-hi="Weixin" bindtap="tapName"> Click me! </view>
    在相应的Page定义中写上相应的事件处理函数，参数是event。
        Page({
            tapName: function(event) {
                console.log(event)
            }
        })
```
### 使用WXS函数响应事件
    从基础库版本2.4.4开始，支持使用WXS函数绑定事件，WXS函数接受2个参数，第一个是event，在原有的event的基础上加了event.instance对象，第二个参数是ownerInstance，和event.instance一样是一个ComponentDescriptor对象
```css
<wxs module="wxs" src="./test.wxs"></wxs>
<view id="tapTest" data-hi="Weixin" bindtap="{{wxs.tapName}}"> Click me! </view>
**注：绑定的WXS函数必须用{{}}括起来**,如果在页面中定义只需要双引号
```
### 事件分类
    事件分为冒泡事件和非冒泡事件：
    冒泡事件：当一个组件上的事件被触发后，该事件会向父节点传递。
    非冒泡事件：当一个组件上的事件被触发后，该事件不会向父节点传递。
```css
    WXML的冒泡事件列表：

    类型	触发条件
    touchstart	手指触摸动作开始	
    touchmove	手指触摸后移动	
    touchcancel	手指触摸动作被打断，如来电提醒，弹窗	
    touchend	手指触摸动作结束	
    tap	手指触摸后马上离开	
    longpress	手指触摸后，超过350ms再离开，如果指定了事件回调函数并触发了这个事件，tap事件将不被触发	
    longtap	手指触摸后，超过350ms再离开（推荐使用longpress事件代替）	
    transitionend	会在 WXSS transition 或 wx.createAnimation 动画结束后触发	
    animationstart	会在一个 WXSS animation 动画开始时触发	
    animationiteration	会在一个 WXSS animation 一次迭代结束时触发	
    animationend	会在一个 WXSS animation 动画完成时触发	
    touchforcechange	在支持 3D Touch 的 iPhone 设备，重按时会触发

    除上表之外的其他组件自定义事件如无特殊声明都是非冒泡事件，如 form 的submit事件，input 的input事件，scroll-view 的scroll事件，(详见各个组件)
    在大多数组件和自定义组件中， bind 后可以紧跟一个冒号，其含义不变，如 bind:tap 
```
### 绑定并阻止事件冒泡
```css
    除 bind 外，也可以用 catch 来绑定事件。与 bind 不同， catch 会阻止事件向上冒泡。
```
### 互斥事件绑定
```css
    除 bind 和 catch 外，还可以使用 mut-bind 来绑定事件。一个 mut-bind 触发后，如果事件冒泡到其他节点上，其他节点上的 mut-bind 绑定函数不会被触发，但 bind 绑定函数和 catch 绑定函数依旧会被触发。
    换而言之，所有 mut-bind 是“互斥”的，只会有其中一个绑定函数被触发。同时，它完全不影响 bind 和 catch 的绑定效果。

```
### 事件的捕获阶段
```css
    触摸类事件支持捕获阶段。捕获阶段位于冒泡阶段之前，且在捕获阶段中，事件到达节点的顺序与冒泡阶段恰好相反。需要在捕获阶段监听事件时，可以采用capture-bind、capture-catch关键字，后者将中断捕获阶段和取消冒泡阶段。
    在下面的代码中，点击 inner view 会先后调用handleTap2、handleTap4、handleTap3、handleTap1。
    <view id="outer" bind:touchstart="handleTap1" capture-bind:touchstart="handleTap2">
        outer view
        <view id="inner" bind:touchstart="handleTap3" capture-bind:touchstart="handleTap4">
            inner view
        </view>
    </view>
```
### 事件对象
```css
    如无特殊说明，当组件触发事件时，逻辑层绑定该事件的处理函数会收到一个事件对象。
    BaseEvent 基础事件对象属性列表：
    属性	类型	说明	
    type	String	事件类型	
    timeStamp	Integer	事件生成时的时间戳	
    target	Object	触发事件的组件的一些属性值集合	
    currentTarget	Object	当前组件的一些属性值集合	
    mark	Object	事件标记数据

    CustomEvent 自定义事件对象属性列表（继承 BaseEvent）：
    属性	类型	说明
    detail	Object	额外的信息

    TouchEvent 触摸事件对象属性列表（继承 BaseEvent）：
    属性	类型	说明
    touches	Array	触摸事件，当前停留在屏幕中的触摸点信息的数组
    changedTouches	Array	触摸事件，当前变化的触摸点信息的数组

    对象里面的属性
    type    代表事件的类型
    timeStamp   页面打开到触发事件所经过的毫秒数

    target触发事件的源组件。
    属性	类型	说明
    id	    String	事件源组件的id
    dataset	Object	事件源组件上由data-开头的自定义属性组成的集合

    currentTarget事件绑定的当前组件。
    属性	类型	说明
    id	    String	当前组件的id
    dataset	Object	当前组件上由data-开头的自定义属性组成的集合,和上面的大部分一样

    dataset
    在组件节点中可以附加一些自定义数据。这样，在事件中可以获取这些自定义的节点数据，用于事件的逻辑处理。
    在 WXML 中，这些自定义数据以 data- 开头，多个单词由连字符 - 连接。这种写法中，连字符写法会转换成驼峰写法，而大写字符会自动转成小写字符。如：
    data-element-type ，最终会呈现为 event.currentTarget.dataset.elementType ；
    data-elementType ，最终会呈现为 event.currentTarget.dataset.elementtype 。
    如:<view data-alpha-beta="1" data-alphaBeta="2" bindtap="bindViewTap"> DataSet Test </view>

    mark 在基础库版本 2.7.1 以上，可以使用 mark 来识别具体触发事件的 target 节点。此外， mark 还可以用于承载一些自定义数据（类似于 dataset ）。
    当事件触发时，事件冒泡路径上所有的 mark 会被合并，并返回给事件回调函数。（即使事件不是冒泡事件，也会 mark 。）
    如:<view mark:myMark="last" bindtap="bindViewTap">
        <button mark:anotherMark="leaf" bindtap="bindButtonTap">按钮</button>
    </view>
    则:e.mark.myMark === "last"
    e.mark.anotherMark === "leaf" 

    mark 和 dataset 很相似，主要区别在于： mark 会包含从触发事件的节点到根节点上所有的 mark: 属性值；而 dataset 仅包含一个节点的 data- 属性值。
    细节注意事项：
    如果存在同名的 mark ，父节点的 mark 会被子节点覆盖。
    在自定义组件中接收事件时， mark 不包含自定义组件外的节点的 mark 。
    不同于 dataset ，节点的 mark 不会做连字符和大小写转换。

    touches
    touches 是一个数组，每个元素为一个 Touch 对象（canvas 触摸事件中携带的 touches 是 CanvasTouch 数组）。 表示当前停留在屏幕上的触摸点。

    Touch 对象
    属性	            类型	说明
    identifier	        Number	触摸点的标识符
    pageX, pageY	    Number	距离文档左上角的距离，文档的左上角为原点 ，横向为X轴，纵向为Y轴
    clientX, clientY	Number	距离页面可显示区域（屏幕除去导航条）左上角距离，横向为X轴，纵向为Y轴

    CanvasTouch 对象
    属性	    类型	说明	特殊说明
    identifier	Number	触摸点的标识符	
    x, y	    Number	距离 Canvas 左上角的距离，Canvas 的左上角为原点 ，横向为X轴，纵向为Y轴
```

## 小程序是和react一样的单向数据绑定,所以如果数据更改想视图的也更改就使用this.setData({key:value}),则这样显示框的也会更改,但如果想实现双向数据绑定的话,像Vue一样使用model:value="{value}",但双向数据绑定只能绑定数据,不能进行拼接和计算
```css
    自定义组件还可以自己触发双向绑定更新，做法就是：使用 setData 设置自身的属性。例如：
    // custom-component.js
    Component({
    properties: {
        myValue: String
    },
    methods: {
        update: function() {
        // 更新 myValue
        this.setData({
            myValue: 'leaf'
        })
        }
    }
    })
    如果页面这样使用这个组件：
    <custom-component model:my-value="{{pageValue}}" />,底层就和Vue一样,会传值和触发update函数,Vue的是传value和触发input函数
```

## 选择某一元素
const query = wx.createSelectorQuery()  
var the-id = query.select('#the-id')