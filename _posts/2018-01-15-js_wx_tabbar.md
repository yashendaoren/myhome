---
layout: post
title: tabBar 、 navBarTitle 和常用window配置
date: 2018-01-15
category: 小程序与快应用

---

### 动态改变NavigationBarTitle(

```markdown

wx.setNavigationBarTitle({
    title: "动态title"
}) 

```

### window常用配置

```markdown

"window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#d81e06",
    "navigationBarTitleText": "demo",
    "navigationBarTextStyle": "white",
    "enablePullDownRefresh": true,
    "disableScroll": true //禁止页面滚动

},

```
### tabBar配置

```markdown

"tabBar": {
    "list": [
        {
            "pagePath": "pages/send/send",
            "text": "发红包",     
            "iconPath": "/style/images/hongbao_h.png",
            "selectedIconPath": "/style/images/hongbao.png"
        },
        {
            "pagePath": "pages/myhome/myhome",
            "text": "我的",
            "iconPath": "/style/images/me_h.png",
            "selectedIconPath": "/style/images/me.png"
        }
    ]
}

```
### 跳转tabBar页面

```markdown

wx.switchTab({
    url: '../send/send'
});

//跳转之后，tabBar的文字默认是绿色的，如果想自定义颜色，可以在该页面onShow方法中修改
onShow: function () {

    wx.setTabBarStyle({
        color: "#000000",
        selectedColor: "#d81e06",
    })

},

```




