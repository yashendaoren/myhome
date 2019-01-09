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


```

### wepy初见

```
import wepy from 'wepy';
//Index 命名首字母一定要大写
export default class Index extends wepy.page{ }

//默认setData await使用
this.userInfo = await wepy.getUserInfo();
//异步执行的时候调用
this.$apply();

```
### 基础必记

```
import wepy from 'wepy';

export default class MyPage extends wepy.page {
    // export default class MyComponent extends wepy.component {
    customData = {}  // 自定义数据

    customFunction ()　{}  //自定义方法

    onLoad () {}  // 在Page和Component共用的生命周期函数

    onShow () {}  // 只在Page中存在的页面生命周期函数

    config = {};  // 只在Page实例中存在的配置数据，对应于原生的page.json文件

    data = {};  // 页面所需数据均需在这里声明，可用于模板数据绑定

    components = {};  // 声明页面中所引用的组件，或声明组件中所引用的子组件

    mixins = [];  // 声明页面所引用的Mixin实例

    computed = {};  // 声明计算属性（详见后文介绍）

    watch = {};  // 声明数据watcher（详见后文介绍）

    methods = {};  
    /*
        声明页面wxml中标签的事件处理函数。
        注意，此处只用于声明页面wxml中标签的bind、catch事件
        自定义方法需以自定义方法的方式声明
    */

    events = {};  // 声明组件之间的事件处理函数
}

```

### 组件引用

```

import Child from '../components/child';
components = {
    //为两个相同组件的不同实例分配不同的组件ID，从而避免数据同步变化的问题
    child: Child,
    anotherchild: Child
};


```

### 循环

```
<!-- 注意，使用for属性，而不是使用wx:for属性 -->
<repeat for="{{list}}" key="index" index="index" item="item">
    <!-- 插入<script>脚本部分所声明的child组件，同时传入item -->
    <child :item="item"></child>
</repeat>

```

### 计算属性 监听器 传值

```
// 计算属性aPlus，在脚本中可通过this.aPlus来引用，在模板中可通过{{ aPlus }}来插值
computed = {
    aPlus () {
        return this.a + 1
    }
}


// 监听器函数名必须跟需要被监听的data对象中的属性num同名，
data = {
    num: 1
}
// 其参数中的newValue为属性改变后的新值，oldValue为改变前的旧值
watch = {
    num (newValue, oldValue) {
        console.log(`num value: ${oldValue} -> ${newValue}`)
    }
}

//传值
props = {
    // 静态传值
    title: String,

    // 父向子单向动态传值
    syncTitle: {
        type: String,
        default: 'null'
    },

    twoWayTitle: {
        type: String,
        default: 'nothing',
        twoWay: true
    }
};

onLoad () {
    console.log(this.title); // p-title
    console.log(this.syncTitle); // p-title
    console.log(this.twoWayTitle); // p-title

    this.title = 'c-title';
    console.log(this.$parent.parentTitle); // p-title.
    this.twoWayTitle = 'two-way-title';
    this.$apply();
    
    // two-way-title. 
        twoWay为true时，子组件props中的属性值改变时，会同时改变父组件对应的值
    console.log(this.$parent.parentTitle); 
    
    this.$parent.parentTitle = 'p-title-changed';
    this.$parent.$apply();
    
    // 'c-title';
    console.log(this.title); 
    
    // 'p-title-changed' 
        有.sync修饰符的props属性值，当在父组件中改变时，会同时改变子组件对应的值。
    console.log(this.syncTitle); 
}

```

### 组件通信与交互

```
//$broadcast事件是由父组件发起，所有子组件都会收到
//父
this.$broadcast('fuviewclick','aaaaaaaa')
//子
events = {
    fuviewclick(e){
        console.log(e);
    }
};


//$emit 子组件发起 父组件收到
//子
test1click() {
    this.$emit('test1func', 'hehe');
},
//父
events = {
    test1func(){
        console.log(`other events func`);
    }
};

//$invoke 一个页面调用另一个页面的方法 或者 一个组件调用另一个组件的方法。
     此方法用的最多，因为页面和组件都能交互
//子一
testclick() {
    this.$invoke("../test2", 'test1click', 'haha');
}
//子二
methods = {
    //或者直接写在最外面 不能写在events中，不知道为什么
    test1click(e) {
        console.log(e);
    }
};
```

### 组件自定义事件处理函数

```
.default: 绑定小程序冒泡型事件，如bindtap，.default后缀可省略不写；
.stop: 绑定小程序捕获型事件，如catchtap；
.user: 绑定用户自定义组件事件通过$emit触发。
//注意，如果用了自定义事件，则events中对应的监听函数不会再执行。

//定义
@customEvent.user="myFn"
//触发
this.$emit('customEvent', 100) 
//实现
methods = {
    myFn (num, evt) {
        console.log('myFn received emit event, number is: ' + num)
    }
}


```

### 参数传递

```
// WePY 1.1.8以后的版本，只允许传string。
<view @tap="tapName({{index}}, 'wepy', 'otherparams')"> Click me! </view>

methods: {
    tapName (id, title, other, event) {
        console.log(id, title, other)// output: 1, wepy, otherparams
    }
}

```





