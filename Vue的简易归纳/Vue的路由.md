## Vue的路由对象$route
```css
    $route对象表示当前的路由信息，包含了当前 URL 解析得到的信息。包含当前的路径，参数，query对象等。
    $route.path 字符串，对应当前路由的路径，总是解析为绝对路径，如"/foo/bar"。
    $route.params 一个 key/value 对象，包含了 动态片段 和 全匹配片段， 如果没有路由参数，就是一个空对象(get的请求参数)
    route.query一个key/value对象，表示URL查询参数。例如，对于路径/foo?user=1，则有route.query.user == 1， 如果没有查询参数，则是个空对象。(post的请求参数)
    $route.hash 当前路由的hash值 (不带#) ，如果没有 hash 值，则为空字符串。
    $route.fullPath 完成解析后的 URL，包含查询参数和hash的完整路径。
    $route.matched 数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
    $route.name 当前路径名字
    $route.meta 路由元信息(很有用,可以在管理系统的权限管理里面应用)
```

## $router路由器对象
```css
    $router对象是全局路由的实例，是router构造方法的实例。
    路由实例的方法:
    push(),字符串this.$router.push(‘home’)
    对象this.$router.push({path:‘home’})
    命名的路由this.$router.push({name:‘user’,params:{userId:123}})
    go方法
    页面路由跳转
    前进或者后退this.$router.go(-1) // 后退
    replace方法,不会向 history 栈添加一个新的记录,一般使用replace来做404页面
    this.$router.replace(’/’)
    配置路由时path有时候会加 ‘/’ 有时候不加,以’/'开头的会被当作根路径，就不会一直嵌套之前的路径。

## 钩子函数后期补充
```css
    导航钩子的参数：
    router.beforeEach((to,from, next)=>{//to 和from都是 路由信息对象,后面使用路由的钩子函数就容易理解了})
```