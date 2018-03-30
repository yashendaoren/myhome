---
layout: post
title: iOS / C语言 结构体（Struct）和 联合体（Union）
date: 2014-7-13
category: [iOS]

---


### Struct 和 Union

```markdown

struct sTest
{

    int a;  //sizeof(int) = 4
    char b;  //sizeof(char) = 1
    shot c； //sizeof(shot) = 2

}sx;
//所以在内存中至少占用 4+1+2 = 7 byte。然而实际中占用的内存并不是7 byte，这就涉及到了字节对齐方式


union uTest
{

    int a;   //sizeof(int) = 4
    double b;  //sizeof(double) = 8
    char c;  //sizeof(char) = 1

}ux;

//分配给union的内存size 由类型最大的元素 size 来确定，为8 byte


```


### 区别

struct 可以存储多个成员信息（以下都能存储）
- sx.a = 4
- sx.b = "A"
- sx.c = 2

Union每个成员会用同一个存储空间，只能存储最后一个成员的信息。(只保存了ux.c，其他的已经失去意义了)
- ux.a = 4
- ux.b = 1.5
- ux.c = "A"


### 他们的对齐方式记不太清了
C语言好久不用了，好多东西都忘了









