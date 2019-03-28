# UI编程

简单了解IOS开发的UI编程以及网络存储部分，接受起来有点困难，所以对课件中的知识进行梳理，并且多看提供的demo

## UI控件

了解常用的UI控件的使用语法

* UILabel 文本框
* UIImageview 静态图 
* UIImageview 动态图
* UIButton按钮
* UISwitch 开关
* UITextfield 单行输入框
* UITextview 多行输入框

```
// label
UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(20,100,300,100)];
[label sizeToFit];
[self.view addSubview:label];

//image
UIImage *image = [UIImage imageNamed:@"test.png"];
UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
[self.view addSubview:imageView];

//button
UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
[self.view addSubview:button];

//switch
UISwitch *switchView = [[UISwitch alloc] initWithFrame:CGRectZero];
[self.view addSubview:switchView];

//textField
UITextField = *textField = [[UITextField alloc] initWithFrame:CGRectMake(20,100,300,100) ];
[self.view addSubview:textfield];
```

## IOS绘图

使用 **UIKit** 绘图, UIKit 框架提供 UIView , 所有视图控件的基类。
**UIView** 绘制的几种方式：
* 视图叠加
* 设置透明度/边框/背景颜色
* 实现 **drawRect**



# 网络与存储
