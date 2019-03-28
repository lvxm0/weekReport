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

### 视图绘制周期 
1. 调用视图的setNeedsDisplay会将视图标记为重新绘制，在下一个次绘制周期中会进行重新绘制。
2. 视图会回调drawRect方法，在drawRect上实现绘制代码即可。
3. setNeedsDisplayInRect会触发局部区域重绘。
4. 注意：drawRect是系统回调，主动调用是无效的。

### 实现 drawRect
推荐使用高度封装好的UIBezierPath进行绘制
* 绘制图形 
```
//三角形
UIBezierPath *path = [[UIBezierPath alloc] init];
[path stroke];
//圆形
UIBezierPath *path = [[UIBezierPath bezierPathWithRoundRect:CGRectMake(50,100,300,300) cornerRadius:150] init];
[path stroke];
```
* 绘制文字
```
//NSString
NSString *text = ...;
[text drawAtPoint:(CGPoint)p withAttributes:(NSDictionary*)attri];
[text drawInRect:(CGRect)r withAttributes:(NSDictionary*)attri];
[text drawAsPatternInRect::(CGRect)r options:(NSDictionary*)attri context:(NSStringDrawingContext*)c];

//NSAttibutedString
NSAttibutedString *attibutedString = ...;
[attibutedString drawAtPoint:(CGPoint)p];
[attibutedString drawInRect:(CGRect)r];
[attibutedString drawAsPatternInRect::(CGRect)r options:(NSDictionary*)attri context:(NSStringDrawingContext*)c];
```
* 绘制图片
```
UIImage *image = ...
[image drawAtPoint:(CGPoint)p];
[image drawInRect:(CGRect)r];
[image drawAsPatternInRect::(CGRect)r];
```

# 网络与存储
