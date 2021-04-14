## pages.json
```css
    pages.json 文件用来对 uni-app 进行全局配置，决定页面文件的路径、窗口样式、原生的导航栏、底部的原生tabbar 等。类似于微信小程序的app.json
    配置内容:https://uniapp.dcloud.io/collocation/pages
    uni-app 通过 pages 节点配置应用由哪些页面组成，pages 节点接收一个数组，数组每个项都是一个对象
    一个包含所有配置选项的pages
    {
    "pages": [{//path不需要写后缀,框架会自动寻找
        "path": "pages/component/index",
        "style": {//配置页面窗口表现,用于设置每个页面的状态栏、导航条、标题、窗口背景色等。会覆盖globalStyle的相同项配置
            "navigationBarTitleText": "组件"
        }
    }, {
        "path": "pages/API/index",
        "style": {
            "navigationBarTitleText": "接口"
        }
    }, {
        "path": "pages/component/view/index",
        "style": {
            "navigationBarTitleText": "view"
        }
    }],
    "condition": { //模式配置，仅开发期间生效
        "current": 0, //当前激活的模式（list 的索引项）
        "list": [{
            "name": "test", //模式名称
            "path": "pages/component/view/index" //启动页面，必选
        }]
    },
    "globalStyle": {//用于设置应用的状态栏、导航条、标题、窗口背景色等
        "navigationBarTextStyle": "black",
        "navigationBarTitleText": "演示",
        "navigationBarBackgroundColor": "#F8F8F8",
        "backgroundColor": "#F8F8F8",
        "usingComponents":{
            "collapse-tree-item":"/components/collapse-tree-item"
        },
        "renderingMode": "seperated", // 仅微信小程序，webrtc 无法正常时尝试强制关闭同层渲染
        "pageOrientation": "portrait", //横屏配置，全局屏幕旋转设置(仅 APP/微信/QQ小程序)，支持 auto / portrait / landscape
        "rpxCalcMaxDeviceWidth": 960,
        "rpxCalcBaseDeviceWidth": 375,
        "rpxCalcIncludeWidth": 750
    },
    "tabBar": {
        "color": "#7A7E83",
        "selectedColor": "#3cc51f",
        "borderStyle": "black",
        "backgroundColor": "#ffffff",
        "height": "50px",
        "fontSize": "10px",
        "iconWidth": "24px",
        "spacing": "3px",
        "list": [{
            "pagePath": "pages/component/index",
            "iconPath": "static/image/icon_component.png",
            "selectedIconPath": "static/image/icon_component_HL.png",
            "text": "组件"
        }, {
            "pagePath": "pages/API/index",
            "iconPath": "static/image/icon_API.png",
            "selectedIconPath": "static/image/icon_API_HL.png",
            "text": "接口"
        }],
        "midButton": {
            "width": "80px",
            "height": "50px",
            "text": "文字",
            "iconPath": "static/image/midButton_iconPath.png",
            "iconWidth": "24px",
            "backgroundImage": "static/image/midButton_backgroundImage.png"
        }
    },
    "easycom": {
        "autoscan": true, //是否自动扫描组件
        "custom": {//自定义扫描规则
            "^uni-(.*)": "@/components/uni-$1.vue"
        }
    },
    "topWindow": {
        "path": "responsive/top-window.vue",
        "style": {
            "height": "44px"
        }
    },
    "leftWindow": {
        "path": "responsive/left-window.vue",
        "style": {
            "width": "300px"
        }
    },
    "rightWindow": {
        "path": "responsive/right-window.vue",
        "style": {
            "width": "300px"
            "navigationBarTitleText": "首页",//设置页面标题文字
            "enablePullDownRefresh":true//开启下拉刷新
        },
        "matchMedia": {
        "minWidth": 768
        }
    }
}
```
## 底部分割线
    SplitLineStyles{//底部分割线
        color : 底部分割线的颜色
        height : 分割线的像素,可取小数
    }
## searchInput
    searchInput可以在titleNView的原生导航栏上放置搜索框。其宽度根据周围元素自适应。如果想获取它的内容的话可以使用在生命周期里通过参数e.text.
## 部分导航栏配置
```css
    {
    "pages": [{
            "path": "pages/index/index", //首页
            "style": {
                "app-plus": {
                    "titleNView": false //禁用原生导航栏
                }
            }
        }, {
            "path": "pages/log/log", //日志页面
            "style": {
                "app-plus": {
                    "bounce": "none", //关闭窗口回弹效果
                    "titleNView": {
                        "buttons": [ //原生标题栏按钮配置,
                            {
                                "text": "分享" //原生标题栏增加分享按钮，点击事件可通过页面的 onNavigationBarButtonTap 函数进行监听
                            }
                        ],
                        "backButton": { //自定义 backButton
                            "background": "#00FF00"
                        }
                    }
                }
            }
        }, {
            "path": "pages/detail/detail", //详情页面
            "style": {
                "navigationBarTitleText": "详情",
                "app-plus": {
                    "titleNView": {
                        "type": "transparent"//透明渐变导航栏 App-nvue 2.4.4+ 支持
                    }
                }
            }
        }, {
            "path": "pages/search/search", //搜索页面
            "style": {
                "app-plus": {
                    "titleNView": {//导航栏
                        "type": "transparent",//透明渐变导航栏 App-nvue 2.4.4+ 支持
                        "searchInput": {
                            "backgroundColor": "#fff",
                            "borderRadius": "6px", //输入框圆角
                            "placeholder": "请输入搜索内容",
                            "disabled": true //disable时点击输入框不置焦，可以跳到新页面搜索
                        }
                    }
                }
            }
        }
        ...
    ]
}
subNVues 是 vue 页面的原生子窗体
{
    "pages": [{
        "path": "pages/index/index", //首页
        "style": {
            "app-plus": {
                "titleNView": false , //禁用原生导航栏
                "subNVues":[{//侧滑菜单
                    "id": "drawer", //subNVue 的 id，可通过 uni.getSubNVueById('drawer') 获取
                    "path": "pages/index/drawer.nvue", // nvue 路径
                    "style": { //webview style 子集，文档可暂时开放出来位置，大小相关配置
                        "position": "popup", //除 popup 外，其他值域参考 5+ webview position 文档
                        "width": "50%"
                    }

                }, {//弹出层
                    "id": "popup",
                    "path": "pages/index/popup",
                    "style": {
                        "position": "popup",
                        "margin":"auto",
                        "width": "150px",
                        "height": "150px"
                    }

                }, {//自定义头
                    "id": "nav",
                    "path": "pages/index/nav",
                    "style": {
                        "position": "dock",
                        "height": "44px"
                    }

                }]
            }
        }
    }]
}
```
## 导航栏设置的属性
```css
    属性	类型	默认值	描述	最低版本
    backgroundColor	String	#F7F7F7	背景颜色，颜色值格式为"#RRGGBB"。	
    buttons	Array		自定义按钮，参考 buttons	
    titleColor	String	#000000	标题文字颜色	
    titleText	String		标题文字内容	
    titleSize	String		标题文字字体大小	
    type	String	default	导航栏样式。"default"-默认样式；"transparent"-透明渐变。	
    searchInput	Object		导航栏上的搜索框样式，详见：searchInpu
```
## 省略组件安装,引用,注册三个步骤
    传统vue组件，需要安装、引用、注册，三个步骤后才能使用组件。easycom将其精简为一步。 只要组件安装在项目的components目录下，并符合components/组件名称/组件名称.vue目录结构。就可以不用引用、注册，直接在页面中使用,不管components目录下安装了多少组件，easycom打包后会自动剔除没有使用的组件，对组件库的使用尤为友好,它是自动开启的,easycom引入的组件可以在任意页面中使用.但是它只处理Vue组件