# UI编程

简单了解IOS开发的UI编程以及网络存储部分，接受起来有点困难，所以对课件中的知识进行梳理，并且多看提供的demo。

*注意*
- UIViewController生命周期中，self.view中就调用了viewDidLoad函数。
- TabBarItem个数不建议超过5个。
- UITabBarController生命周期中，加载TabController只会加载第一个，切换时只移除不销毁。
- 不同类型的UITableViewCell ,通过维护可视数组和重用数组来完成重用机制。


## UI控件

了解常用的UI控件的使用语法

* UILabel 文本框
* UIImageview 静态图 
* UIImageview 动态图
* UIButton按钮
* UISwitch 开关
* UITextfield 单行输入框
* UITextview 多行输入框

```objective-c
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

### **视图绘制周期**

1. 调用视图的setNeedsDisplay会将视图标记为重新绘制，在下一个次绘制周期中会进行重新绘制。
2. 视图会回调drawRect方法，在drawRect上实现绘制代码即可。
3. setNeedsDisplayInRect会触发局部区域重绘。
4. 注意：drawRect是系统回调，主动调用是无效的。

### **Graphic State**

- 使用 **CGContextSaveGstate,CGContextRestoreGstate** 保存读取状态，中间的状态不会影响已经保存的状态。
- 使用 **UIGraphicsPushContext,UIGraphicsPopContext** 把当前状态压栈，中间对其修改，再弹出，是修改后的状态。

### **实现 drawRect**

推荐使用高度封装好的UIBezierPath进行绘制

* 绘制图形 
```objective-c
//三角形
UIBezierPath *path = [[UIBezierPath alloc] init];
[path stroke];
//圆形
UIBezierPath *path = [[UIBezierPath bezierPathWithRoundRect:CGRectMake(50,100,300,300) cornerRadius:150] init];
[path stroke];
```
* 绘制文字
```objective-c
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
```objective-c
UIImage *image = ...
[image drawAtPoint:(CGPoint)p];
[image drawInRect:(CGRect)r];
[image drawAsPatternInRect::(CGRect)r];
```

## UITableView

UITableView是一个列表控件，广泛运用于APP的各个界面，本质是垂直方向滚动的ScrollView。 UITableView有两种风格：UITableStylePlain,UITableStyleGrouped。

### **UITableView 结构**
- TableHeader
- section （分组）：Header,Row,Footer
- TableFooter

## UIViewController

ViewController是iOS应用程序中重要的部分，是应用程序数据和视图之间的重要桥梁，ViewController管理应用中的众多视图。

### **ViewController分类**
- 展示型ViewController
- 容器型ViewController

### **ViewController创建**
```objective-c
UIViewController *controller = [[UIViewController alloc] init];
```


# 网络与存储
网络编程与本地存储

## 网络编程

- get请求:  构建request对象，创建session，创建NSUPLSessionDataTask,执行Task,Response处理。
- post请求: 构建可变的。request对象，把请求方法改为post,设置请求数据，其余不变。
- download请求: 创建NSUPLSessionDownloadTask，其余不变。

### URL 

1. URL对象建立：

```objective-c
//创建URL从网络服务器
NSURL *url = [NSURL URLWithString:@"http://www.baidu.com"];
//创建URL从本地文件
NSURL *url = [NSURL fileURLWithPath:@"/Users/apple/desktop/text.txt"];
```

2. URLRequest 对象建立：

```objective-c
//默认的Request
NSURLRequest *req = [NSURLRequest requestWithURL:url];
//设定缓存策略，及网络请求超时时间
NSURLRequest *req1 = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestReloadIgnoringLocalCacheData timeoutInterval:60];
```

3. URLConnection的异步请求

```objective-c
[NSURLConnection sendAsynchronousRequest:req queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse *response, NSData *data, NSError *connectionError) {
        if (connectionError) {
            NSLog(@"error:%@",connectionError);
        }else{
            NSLog(@"response:%@",response);
            NSLog(@"datalength:%lu",data.length);
        }
}];
```

## 本地存储

- ios沙盒: 每个app都有一个存储空间，ios系统为每个app创建自己的目录，只能访问自己的目录。
- NSUserDefault:用来储存用户设置，系统配置等数据。
