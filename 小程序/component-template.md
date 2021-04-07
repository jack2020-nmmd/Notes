## 组件中纯数据字段,有利于提升性能
```css
    Component({
    options: {
        pureDataPattern: /^_/ // 指定所有 _ 开头的数据字段为纯数据字段
    },
    data: {
        a: true, // 普通数据字段
        _b: true, // 纯数据字段
    }}
    上述组件中的纯数据字段不会被应用到 WXML 上：
    <view wx:if="{{a}}"> 这行会被展示 </view>
    <view wx:if="{{_b}}"> 这行不会被展示 </view>
    可以在页面或自定义组件的 json 文件中配置 pureDataPattern （这样就不需在 js 文件的 options 中再配置）但是设置了的监听属性observe不可以监听了,因为不会触发,如果要监听使用监听器observes:{}
```