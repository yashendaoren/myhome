---
layout: post
title: 获取手机信息，节点位置，去除button样式 等常用操作
date: 2018-03-14
category: 小程序与快应用

---

### 获取手机信息

```markdown

wx.getSystemInfo({  
    success: function(res) {  
    console.log(res.model)  
    console.log(res.pixelRatio)  
    console.log(res.windowWidth)  
    console.log(res.windowHeight)  
    console.log(res.language)  
    console.log(res.version)  
    }  
})  

```

### 小程序获取 节点位置信息和滚动

```markdown

var bindId = event.currentTarget.dataset.bindid;
var query = wx.createSelectorQuery()
    query.select('#scrollView' + bindId).boundingClientRect()
    query.exec(function (res) {
        wx.pageScrollTo({ //小程序滚动程序
        scrollTop: res[0].top //距离现在定位的距离
    })
})

```
### button 去除原有样式

```markdown
// 改样式
button{
    width: 100%;
    height: 100rpx;
    color: darkorange;
    text-align: left;
    background: #FFF; 
}
//　去边框
button::after{
    border: none;
}

```
### image自定义弹窗icon

```markdown

wx.showToast({
    title: '成功',
    icon: 'success',
    image: '../image/warn.png',
    duration: 2000
})

```

### 数组去空

```markdown

for(var i = 0; i < array.length;i++){
    var obj = array[i]
    if (obj==null||obj==undefined){
        array.splice(i, 1)    
    }
}

```

### 其他

小程序的bug还是有点多的。textarea 下方有布局，弹起之后又可能会被键盘遮挡。placeholder 在键盘收起的时候，可能不会收回，发生错位。video、textarea 层级都是最高，不能通过z-index改变，不能用在scroll-view等视图中使用，video滑动会遮挡所有下面的视图。



