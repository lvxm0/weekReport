# week 7
16340164 吕雪萌

确定了小组分工，确定做前端UI。这周学习制作Feed流APP的UI资料:

- [toolbar、tabbar的区别和联系](https://www.zhihu.com/question/28789746)
- [UIColor自定义颜色](https://www.jianshu.com/p/12cecb7e7912)
- [侧滑返回](https://www.jianshu.com/p/945a72263f72)
- [拖动手势](https://www.jianshu.com/p/81bf52e3d04a)

## bar的联系和区别

- Tab bar 标签栏
  - 标签栏在app屏幕底部出现，提供了在app不同部分间快速切换的途径，在横竖屏都保持一致的高度，并且在出现键盘时会被隐藏。一般来说，利用标签栏组织应用程序级别的信息。
  - 标签栏只能作为导航，标签栏按钮不应该执行其它操作。
  - 了解开发细节，参阅[UITabBar](https://link.zhihu.com/?target=https%3A//developer.apple.com/reference/uikit/uitabbar)
  
- Tool bar 工具栏
  - 工具栏在app屏幕底部出现，包含了执行当前视图或包含内容相关操作的按钮，通常在用户不太需要它们时被隐藏。
  - 提供相关的工具栏按钮。
  - 避免在工具栏使用分段控件，分段控件让用户切换内容，而工具栏更针对于当前屏幕。
  - 了解开发细节，参阅[UIToolbar](https://developer.apple.com/documentation/uikit/uitoolbar)
  
- **tab bar、tool bar的区别**
  - 标签栏让用户在app的不同部分间快速切换，比如时钟app中的“闹钟”、“秒表”、“计时器”。工具栏包含了执行当前视图相关操作的按钮，比如创建项、删除项、添加注释或是拍照。标签栏和工具栏决不会在同一个视图内同时出现
  - ToolBar切换一个场景，适用每个场景下的Button不一样的情况。TabBar适合应用的每个场景的Button都一样的情况。
 
## UIColor

  - [概念](https://www.jianshu.com/p/12cecb7e7912)： UIColor类是UIKit中用来存储颜色数据的一个类，可以用来自定义颜色。一个UIColor对象包含了颜色和透明度的值，它的颜色空间已经针对IOS进行了优化
  
  - UIColor包含了一些类方法用于创建一些最常见的颜色，如白色，黑色，红色，透明色等，这些颜色的色彩空间也不尽相同（白色和黑色是kCGColorSpaceDeviceGray，红色的色彩空间是kCGColorSpaceDeviceRGB）
  
  - UIColor还有两个重要的属性：一个是CGColor，一个是CIColor(5.0之后添加)
  
  - [使用方法](https://www.cnblogs.com/xunziji/archive/2012/09/27/2706136.html)
  
  ```Objective-c
  UIColor *testColor1= [UIColor colorWithRed:240/255.0 green:240/255.0 blue:240/255.0 alpha:1];
  labelColor.backgroundColor =  testColor1;
  ```
  
  
## 侧滑返回

  - 系统为UINavigationController添加 interactivePopGestureRecognizer属性，这个属性就是系统提供的右滑返回动画，我们可以通过滑动左边边缘实现pop效果。
  - 也可以关闭系统默认的边缘返回动画： ```self.navigationController.interactivePopGestureRecognizer.enabled = NO;```
  - 缺点
    - 只能实现边缘拖动，如果从视图除左边边缘以外的部分没有效果
    - 如果push到其他界面，如果重写的leftBarButtonItem就失效了
  - 
  
  
## 拖动手势


## 心得体会

## 资源链接

[storyboard 编程](https://juejin.im/post/5a3cbbbc51882527a00f8e7e)

[xib简述](https://www.jianshu.com/p/ea3f90cc744b)

[bar区别](https://www.zhihu.com/question/28789746)

[UIColor自定义颜色](https://www.cnblogs.com/xunziji/archive/2012/09/27/2706136.html)

 [UIColor、CGColor、CIColor 区别和联系](http://www.cnblogs.com/smileEvday/archive/2012/06/05/UIColor_CIColor_CGColor.html)

[侧滑返回](https://www.jianshu.com/p/945a72263f72)

[拖动手势](https://www.jianshu.com/p/81bf52e3d04a)

[Assets.xcassets使用](https://www.jianshu.com/p/c35ce599f7da)

[详细的使用](https://www.cnblogs.com/W-Kr/p/5381750.html)

[WMPageController 使用1](https://juejin.im/post/5a3889bb518825127e745af5)

[使用2](https://www.jianshu.com/p/e2503fb3241b)

[使用3 重点！](https://blog.csdn.net/yubo_725/article/details/51159633)

[pod使用](https://www.jianshu.com/p/5a74c0842cf2)
