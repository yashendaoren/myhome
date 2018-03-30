---
layout: post
title: iOS启动过程及控制器的生命周期
date: 2014-12-07
category: [iOS]

---

对于iOS开发者，需要熟悉一些基本概念，虽然有时候我们并不会用打他们。一下的一些记录，有自己总结的，也有来源于网络的，虽然并不全面，以后有机会就补充吧。


### iOS的main函数

如同任何基于C的应用程序，程序启动的主入口点都应该是main函数。虽然我们很少动它，但是应该懂它，它的主要工作是控制UIKit framework的。

```markdown

int main(int argc, char * argv[])
{
    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil,
            NSStringFromClass([AppDelegate class]));
    }
}

```

UIApplicationMain 函数有四个参数，主要是控制程序进程和加载各种资源

- argc和argv：是ISO C标准的main函数的参数，参数包含应用程序何时从系统启动等信息等。
- principalClassName：这个参数标识了应用程序的类的名称（该类必须继承自UIApplication类）。这是负责运行应用程序的类。一般这个参数传nil，此时，它的值将从Info.plist中获取，如果Info.plist中没有，则默认为UIApplication
- delegateClassName：是应用程序类的代理类。应用程序的代理负责管理系统和你的代码之间的高层次的互动。 一般默认为Xcode自己帮我们创建的[AppDelegate class]。



### 程序启动时 运行的先后顺序 与 事件处理
```markdown

//1，在程序启动过程中 一定会走的方法
//检查启动选项字典中的内容，查看程序启动的方式，并做出适当的反应。
//初始化应用程序的关键数据结构
//准备好你的应用程序的窗口和视图进行显示
- (BOOL)application:(UIApplication *)application
    willFinishLaunchingWithOptions:(NSDictionary *)launchOptions;

- (BOOL)application:(UIApplication *)application
    didFinishLaunchingWithOptions:(NSDictionary *)launchOptions;

//2，创建window窗口

//3，当程序被激活
- (void)applicationDidBecomeActive:(UIApplication *)application;

//其他进入后台 取消激活等都是常用方法，就不多说了
applicationDidEnterBackground:
applicationWillEnterForeground:
applicationWillResignActive:
applicationDidBecomeActive:


```

### UIViewController的生命周期
```markdown

//UIViewController的指定初始化方法
- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil;

//加载视图  通常这一步不需要去干涉
- (void)loadView;

//视图加载完成
- (void)viewDidLoad;

//出现内存警告
//模拟内存警告:点击模拟器->hardware-> Simulate Memory Warning
- (void)didReceiveMemoryWarning;

//视图将要出现
- (void)viewWillAppear:(BOOL)animated;

//视图已经出现
- (void)viewDidAppear:(BOOL)animated;

//视图将要消失
- (void)viewWillDisappear:(BOOL)animated;

//视图已经消失
- (void)viewDidDisappear:(BOOL)animated;

//销毁
- (void)dealloc;


```

### alloc 和 init

alloc   创建对象，分配空间;

init (initWithNibName) 初始化对象，初始化数据;

