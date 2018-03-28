---
layout: post
title: iOS常用小知识
date: 2015-3-24
category: [iOS]

---

记录一些常用的 易忘的 小的知识点。有自己遇到的，有拷贝网上的，希望加深一下记忆。持续更新中......

### UIlabel 设定好固定的宽高，让文字大小自适应

```markdown

label.adjustsFontSizeToFitWidth = YES;

```

### 从图片的二进制格式判断图片类型

```markdown

- (NSString *)typeForImageData:(NSData *)data
{
    uint8_t c;
    [data getBytes:&c length:1];
    switch (c)
    {
        case 0xFF:
            return @".jpeg";
        case 0x89:
            return @".png";
        case 0x47:
            return @".gif";
        case 0x49:
            case 0x4D:
        return @".tiff";
    }
    return nil;
}

```

### 组头视图悬停
UITableView有两个headerView：tableHeaderView、和sectionHeaderView（组头视图）。
tableHeaderView不会悬停。sectionHeaderView可以悬停，由UITableViewStyle控制
<br/>
UITableViewStylePlain，悬停。
<br/>
UITableViewStyleGrouped，不悬停。

### 请求返回的数据类型 text/html 结果 json 解析失败
```markdown

Error Domain=com.alamofire.error.serialization.response
Code=-1016 "Request failed: unacceptable content-type: text/html"

修改方法 在 AFURLResponseSerialization.m 中

self.acceptableContentTypes = [NSSet setWithObjects:
    @"application/json", @"text/json", @"text/javascript",nil
    ];
    
改为

self.acceptableContentTypes = [NSSet setWithObjects:
    @"application/json", @"text/json", @"text/javascript", @"text/html",nil
    ];

json解析成功

```


### 第三方库重复引入
大概会报错
```markdown

 duplicate symbol __isBackDrop in: XXX.0 XXX.0,
 ld: 65 duplicate symbols for architecture arm64 clang:
 error: linker command failed with exit code 1 (use -v to see invocation)

```
other Linker Flags  把 -all_load去掉，就可以正常运行了。

### 去除navigationController下面的线
```markdown

if ([self.navigationController.navigationBar
respondsToSelector:@selector( setBackgroundImage:forBarMetrics:)]){
    //取到navigationBar的子控件数组
    NSArray *list=self.navigationController.navigationBar.subviews;
    for (id object in list) {
        if ([object isKindOfClass:[UIImageView class]]) {
        
            UIImageView *imageView=(UIImageView *)obj;
            NSArray *list2=imageView.subviews;
            
            for (id object2 in list2) {
                if ([object2 isKindOfClass:[UIImageView class]]) {
                    UIImageView *imageView2=(UIImageView *)object2;
                    //取到该线，隐藏
                    imageView2.hidden=YES;
                }
            }
        }
    }
}


//设置导航控制器透明
[self.navigationController.navigationBar
setBackgroundImage:[[UIImage alloc] init] forBarMetrics:UIBarMetricsDefault];



```


### 字典转化成字符串

```markdown

- (NSString*)dictionaryToJson:(NSDictionary *)dic

{

    NSError *parseError = nil;

    NSData *jsonData = [NSJSONSerialization dataWithJSONObject:dic
    options:NSJSONWritingPrettyPrinted error:&parseError];

    return [[NSString alloc] initWithData:jsonData
    encoding:NSUTF8StringEncoding];

}
```

### 统计中英文混合字符串的长度

```markdown

-  (int)convertToInt:(NSString*)strtemp {
    int strlength = 0;
    char* p = (char*)[strtemp cStringUsingEncoding:NSUnicodeStringEncoding];
    for (int i=0 ; i<[strtemp lengthOfBytesUsingEncoding:NSUnicodeStringEncoding] ;i++) {
        if (*p) {
            p++;
            strlength++;
        }else {
            p++;
        }
    }
    return strlength;

}

```

### 模糊背景实现

```markdown

UIBlurEffect *eff = [UIBlurEffect effectWithStyle:UIBlurEffectStyleLight];
UIVisualEffectView *v = [[UIVisualEffectView alloc] initWithEffect:eff];
UIVisualEffectView *v1 = [[UIVisualEffectView alloc] initWithEffect:eff];
v.frame = self.leftBackgroundView.bounds;
v1.frame = self.rightbackgroundView.bounds;

//左右侧背景图片
[self.leftBackgroundView sd_setImageWithURL:[NSURL URLWithString:model.image_url]];
[self.rightbackgroundView sd_setImageWithURL:[NSURL URLWithString:model.image_url]];
[self.rightbackgroundView addSubview:v];
[self.leftBackgroundView addSubview:v1];

```

### KVC修改 UITextField的placeHolder的占位符颜色

```markdown

[_textField setValue:[UIColor orangeColor] forKeyPath:@"_placeholderLabel.textColor"];

```









