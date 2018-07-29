---
layout: post
title: 自定义组件、插件 和 wepy框架
date: 2018-06-06
category: 小程序与快应用

---

### 自定义组件 Component 注意事项

```markdown

//页面json文件中 添加自己需要用到的自定义组件
{
    "usingComponents":{
        "test": "../../component/test",
        "login":"../../component/login/login"
    }
}

//页面wxml中使用 myevent自定义的监听事件的key值，triggerEvent中使用
<login canshu="canshu" title="我的标题" bind:myevent="onMyEvent"  text="{{text}}">
    <button slot="cancel">取消</button>
</login>

//组件中
options: {
    multipleSlots: true // 在组件定义时的选项中启用多slot支持
},
properties: {
    title:{
        type:String,
        value:"标题",
        observer: function (newVal, oldVal, changedPath){
            console.log(newVal+","+oldVal+","+changedPath)

        }
    },
    canshu: String,
},

methods: {
    loginClick:function(){
    
        console.log("loginClick" + " " + this.properties.canshu)
        //用于回传事件给页面
        this.triggerEvent('myevent', { text:"111"})
    },
},


```

### 插件

```markdown
//plugin.json中 列出所有插件 index.js中暴露插件接口
{
    "publicComponents": {
        "list": "components/list/list"
    },
    "main": "index.js"
}

//app.json中 写明版本号和插件的appid
"plugins": {
    "myPlugin": {
        "version": "dev",
        "provider": "wxd322cd2139b1914d"
    }
}

//page的json文件中
{
    {
        "usingComponents": {
        "list": "plugin://myPlugin/list"
    }
}

```

### wepy注意事项

```markdown

//编辑器中改完代码，需要重新运行wepy build 命令

//wxss
<style lang="less"></style>
//js
<script></script>
//wxml
<template></template>
//json 是script 里面的 config
config = {
    navigationBarTitleText: 'test'
}

//手动创建页面，app.wpy中不会自己添加，需要手动添加 page/one
config = {
    pages: [
        'pages/index',
        'pages/one',
    ],
    window: {
        backgroundTextStyle: 'light',
    }
}

wepy刚刚开始看，持续更新中。。。。也可能以后就忘了更新了。。。。



```





