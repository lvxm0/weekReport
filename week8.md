# week8 Report
16340164 吕雪萌

## 实验心得
本周学习使用Assets.xcassets管理图片，了解使用方法。学习WMPageController框架，对做Feed流有了一定的了解。

## Assets.xcassets简介

- Assets.xcassets是用来存放图像资源文件
- 优点

  - 展现1X,2X,3X图片简练，自动管理图片，如 @1x,@2x 图片，使用的时候使用Asset名字即可
  - 管理应用的 Icon 和 Default 图片,方便使用,抛开以前规范命名如 Icon.png，Icon@2x.png，Xcode自动识别尺寸进行匹配
  - 方便管理模块图片，可以针对模块建立Component1.xcassets, 在这个Category中去建立新的Image set
  - 可视化管理图片拉伸，抛弃到处使用resizeImage来获取拉伸图片
  - 方便app图标和启动页图片设置
  
- 注意事项
  - 在Assets中的图片不能通过imageWithContentsOfFile:加载；
  - imageName:加载的图片要么是Assets中的图片，要么是资源包中的图片，如果要用imageName:加载其他的图片，必须在文件名后面添加扩展名。
  ```UIImage *image=[UIImage imageNamed:@"plus.png"];```
  
## Assets.xcassets使用方法

## WMPageController简介

## WMPageController使用方法


## 资源链接

- [Assets.xcassets 资料汇总](https://blog.csdn.net/seadogprogrammer/article/details/52170722)
- [Assets.xcassets 资料总结](https://blog.csdn.net/u012371575/article/details/78584996)
- [Assets.xcassets 使用说明](https://www.jianshu.com/p/c35ce599f7da)
- [Assets.xcassets 详细使用](https://www.cnblogs.com/W-Kr/p/5381750.html)
- [WMPageController 使用1](https://juejin.im/post/5a3889bb518825127e745af5)
- [WMPageController 使用2](https://www.jianshu.com/p/e2503fb3241b)
- [WMPageController 使用3 重点！](https://blog.csdn.net/yubo_725/article/details/51159633)
- [pod使用](https://www.jianshu.com/p/5a74c0842cf2)
