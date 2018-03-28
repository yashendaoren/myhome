---
layout: post
title: 基于CTMediator解耦
date: 2018-03-24
category: [iOS]

---

CTMediator 模块化开发用的还是比较多的，网上教程和讲解也是非常多的，我就不班门弄斧了，也不详细讲解原理了，只是简单记录一下样式和使用

###  首先 创建Target-Action   继承自 NSObject 就行

Target_XXXXXX    这里的 XXXXXX 最好有意义，后面需要通过传入Target 的XXXXXX代码决定进入Target_XXXXXX类中执行方法，比如我下面的 Target_Home 是说这是首页的接口处理

```markdown

.h文件中


#import <Foundation/Foundation.h>

@interface Target_Home : NSObject

//获取首页对象
- (UIViewController *)Action_HomeViewController:(NSDictionary *)params;
//刷新首页方法
- (void)Action_refreshUpdate:(NSDictionary *)params;

//获取另外页面对象
- (UIViewController *)Action_SecondViewController:(NSDictionary *)params;

@end






.m文件中


#import "Target_Home.h"
#import "HomeViewController.h"
#import "SecondViewController.h"

@implementation Target_Home
- (UIViewController *)Action_HomeViewController:(NSDictionary *)params{
    HomeViewController *homeVC = [HomeViewController sharedInstance];
    return homeVC;
}
- (void)Action_refreshUpdate:(NSDictionary *)params{
    HomeViewController *homeVC = [HomeViewController sharedInstance];
    [homeVC refreshUpdate];
}


- (UIViewController *)Action_SecondViewController:(NSDictionary *)params{

    SecondViewController *SecondVC = [[SecondViewController alloc] init];
    //参数
    SecondVC.sId = [params objectForKey:@"sId"];
    return SecondVC;
}

@end

```

### 创建 CTMediator 的分类

action 传入要执行的方法名 target中的方法名 都是以action为前缀的，这里的action前缀可以修改，这里不做过多解释

```markdown

.h文件中


#import "CTMediator.h"

@interface CTMediator (HomeComponent)
/**
paramters 参数以字典样式传入
*/
-(UIViewController*)home_homeViewControllerWithParamters:(NSDictionary*)paramters;

/**
刷新方法
*/
- (void)home_refreshUpdate;

/**
paramters 参数写清楚 必要参数和非必要参数
*/
-(UIViewController*)home_SecondViewControllerWithParamters:(NSDictionary*)paramters;

@end







.m文件中


#import "CTMediator+HomeComponent.h"

@implementation CTMediator (HomeComponent)
- (UIViewController *)home_homeViewControllerWithParamters:(NSDictionary *)paramters{
    return [self performTarget:@"Home"
    action:@"HomeViewController" params:paramters shouldCacheTarget:NO];
}
- (void)home_refreshUpdate{
    [self performTarget:@"Home"
    action:@"refreshUpdate" params:nil shouldCacheTarget:NO];
}

- (UIViewController *)home_SecondViewControllerWithParamters:(NSDictionary *)paramters{

    return [self performTarget:@"Home"
    action:@"SecondViewController" params:paramters shouldCacheTarget:NO];
}

@end

```

### 最终使用



```markdown

#import "CTMediator+HomeComponent.h"


- (void)bButtonClick:(UIButton *)sender {
    UIViewController *viewController = [[CTMediator sharedInstance]
    home_homeViewControllerWithParamters:@{@"home_id":@"123456"}];
    
    [self.navigationController pushViewController:viewController animated:YES];
}

```
使用的好处 就是不用在各处包含#import "XXXX.h" 实现所有页面间的方法 通过路由层间接调用 实现解耦








