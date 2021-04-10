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
        },
        "matchMedia": {
        "minWidth": 768
        }
    }
}
```