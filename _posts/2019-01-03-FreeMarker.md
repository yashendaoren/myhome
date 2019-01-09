---
layout: post
title: FreeMarker
date: 2019-01-03
category: [web开发]

---
### 总体结构

```
<html>
    <head>
        <title>Welcome!</title>
    </head>
    <body>
        <#-- Greet the user with his/her name -->
        <h1>Welcome ${user}!</h1>
        <p>We have these animals:
        <ul>[BR]
        <#list animals as animal>
            <li>${animal.name} for ${animal.price} Euros
        </#list>
        </ul>
    </body>
</html>

```


### 引入

```
一个重要的规则就是路径不应该包含大写字母，
为了分隔词语， 使用下划线 _，就像 wml_form (而不是 wmlForm )。

<#import "../../../include/layout_wap.ftl" as layout />
<@layout.standard title="大点" cssMap={"app":"course/course_wap"}>

    <style>
    // css
    </style>
    <script>
    // js
    </script>

</@layout.standard>

```

### 公用样式
```
<#include "../include/position.ftl" />

```



### 定义变量

```
<#assign dk_list=[
    {"uid":"1","name":"曹雁飞","des":"曹雁飞，女，教育学原"},
    {"uid":"2","name":"曹雁飞","des":"曹雁飞，女，教育学原"},
    {"uid":"3","name":"曹雁飞","des":"曹雁飞，女，教育学原"},
] />

```

### 列表

```
<#list dk_list as item>

    <div class="celebrity-item">

        ${item_index}

        <div class="celebrity-info">
            <div onclick="gotoUserForJson(${item.title},${item.uid})">
                <img src="https://avatar2.citysbs.com/m.gif">
            </div>
            <div class="celebrity-name">${item.name}</div>
            <div class="celebrity-introduce">${item.title}</div>
        </div>
        <div class="celebrity-desc">
            <div class="desc">
            ${item.des}
            </div>
        </div>
    </div>

</#list>

```
### 判断
```
<#if centerHotText?? && boardList?exists && boardList?size gt 0 >

<#elseif board_index == 1 >

<#else>

</#if>

```

### 内建函数

```
<#function createUrl url params={}>
    <#assign tUrl = urlCollect.forumFrontDomainNoHttps+ url >
    <#if (params?size>0)>
        <#assign tUrl = tUrl+"?">
        <#assign keys = params?keys>
        <#list keys as key>
            <#assign tUrl = tUrl + key + "=" + params[key] + "&">
        </#list>
        <#assign tUrl = tUrl?substring(0, tUrl?length-1)>
    </#if>
    <#return tUrl>
</#function>

```

### 表达式

```
字符串切分： 包含结尾： name[0..4]，
           不包含结尾： name[0..<5]，
           基于长度(宽容处理)： name[0..*5]，
           去除开头： name[5..]

序列切分：包含结尾： products[20..29]， 
        不包含结尾： products[20..<30]，
        基于长度(宽容处理)： products[20..*10]，
        去除开头： products[20..]
        
<#list seq[1..3] as i>${i}</#list>

连接： passwords + { "joe": "secret42" }
<#list ["Joe", "Fred"] + ["Julia", "Kate"] as user>${user}</#list>
<#assign ages = {"Joe":23, "Fred":25} + {"Joe":30, "Julia":18}>

内建函数： name?upper_case, path?ensure_starts_with('/')
方法调用： repeat("What", 3) 如：${repeat("Foo", 3)}


处理不存在的值：
默认值： name!"unknown" 或者 (user.name)!"unknown" 或者 name! 或者 (user.name)!
检测不存在的值： name?? 或者 (user.name)??
赋值操作： =, +=, -=, *=, /=, %=, ++, --

使用 >= 和 > 的时候有一点小问题。FreeMarker解释 > 的时候可以把它当作FTL标签的结束符。
为了避免这种问题，可以使用 lt 代替 <， lte 代替 <=， gt 代替 > 还有 gte 代替 >=， 
例如 <#if x gt y>。
另外一个技巧是将表达式放到 圆括号 中， 尽管这么写并不优雅，
例如 <#if (x > y)>。

```

### 自定义指令

```
<#macro greet person color>
    <font size="+2" color="${color}">Hello ${person}!</font>
</#macro>

<@greet></@greet>

<@greet color="black" person="Fred"/>

```

### 嵌套内容

```
//<#nested> 可以多次被调用

<#macro do_thrice>
    <#nested>
    <#nested>
    <#nested>
</#macro>

<@do_thrice>
    Anything.
</@do_thrice>

```

### 空间命名

```
<#macro copyright date>
    <p>Copyright (C) ${date} Julia Smith. All rights reserved.
    <br>Email: ${mail}</p>
</#macro>
<#assign mail = "jsmith@acme.com">


<#import "/lib/my_test.ftl" as my>
<#assign mail="fred@acme.com">
<@my.copyright date="1999-2002"/>

不一样
${my.mail} jsmith@acme.com
${mail} fred@acme.com


```





