# week8 Report
16340164 吕雪萌

## 实验心得
本周学习使用Assets.xcassets管理图片，了解使用方法。学习WMPageController框架，对做Feed流APP实现有一定的帮助。

## Assets.xcassets简介

- Assets.xcassets是用来存放图像资源文件
- 优点

  - 展现1X,2X,3X图片简练，自动管理图片，如 @1x,@2x 图片，使用的时候使用Asset名字即可
  - 管理应用的 Icon 和 Default 图片,方便使用,抛开以前规范命名如 Icon.png，Icon@2x.png，Xcode自动识别尺寸进行匹配
  - 方便管理模块图片，可以针对模块建立Component1.xcassets, 在这个Category中去建立新的Image set
  - 可视化管理图片拉伸，抛弃到处使用resizeImage来获取拉伸图片
  - [方便app图标和启动页图片设置](https://www.jianshu.com/p/a5cf847970d1)
  
- 注意事项
  - 在Assets中的图片不能通过imageWithContentsOfFile:加载；
  - imageName:加载的图片要么是Assets中的图片，要么是资源包中的图片，如果要用imageName:加载其他的图片，必须在文件名后面添加扩展名。如：
  ```UIImage *image=[UIImage imageNamed:@"plus.png"];```
  
## Assets.xcassets使用方法
  - 选择 File - new - File - Resource - Asset Catalog，输入名字和选择Target创建
  - 操作区域：左侧为SetList，中间为Set Viewer，右侧区域为Set attributes inspector
  - SetList区域可以新建删除：
    - New Image Set：管理图片夹
    - New App Icon: 管理图标
    - New Lauch Image: 创建启动图
    - New Folder: 新文件夹，用于文件嵌套
  
  - 创建 New Image Set 文件夹，可以通过添加图片导入，也可以通过将图片直接拖到占位符方式导入
  - [拉伸图片](https://www.cnblogs.com/W-Kr/p/5381750.html)：
    - 选中图片，选择 Show Slicing开始切割
    - 在右侧属性栏中更改slicing属性：水平拉伸/垂直拉伸/水平垂直拉伸
    - 拉伸形式：
      - 左边到左句柄,右句柄到右边部分为不变部分;
      - 左句柄到内句柄为填充样式,将来就会用这部分去填充其他扩充部分;
      - 内句柄到片句柄部分为可扩充部分,随着拉伸或者缩小,这部分将会被填充样式填充;
      ![](http://img.blog.csdn.net/20131115003352531?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0NzQ2OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
  
## WMPageController简介

- WMPageController控件是iOS开发中第三方库，WMPageController类似于Android平台上的ViewPager控件，主要是用来分页显示内容，可以通过手势滑动来切换页面，也可以通过点击标题部分来切换页面

- [官方介绍](https://github.com/wangmchn/WMPageController/blob/master/README_zh-CN.md)


## WMPageController使用方法

- 新建工程，通过cocoapods将WMPageController引入到项目中，Podfile文件的内容如下：
  ```
  platform :ios,'7.0'

  target 'DemoTest1' do

   pod 'WMPageController', '~> 1.6.4'

  end
  ```
- 在ViewController.m文件中加入以下代码,在ViewController中加入了一个UILabel标签，实现第一个分页
  ```
  #import "ViewController.h"

  @interface ViewController ()

  @end

  @implementation ViewController

  - (void)viewDidLoad {
      [super viewDidLoad];
      // Do any additional setup after loading the view, typically from a nib.
      UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(50, 50, 100, 50)];
      label.text = @"Page 1";
      [self.view addSubview:label];
  }

  - (void)didReceiveMemoryWarning {
      [super didReceiveMemoryWarning];
      // Dispose of any resources that can be recreated.
  }

  @end
  ```
- 创建一个ViewController2继承自UIViewController，并且在ViewController2.m文件中加入以下代码，实现第二个分页
  ```
  #import "ViewController2.h"

  @interface ViewController2 ()

  @end

  @implementation ViewController2

  - (void)viewDidLoad {
      [super viewDidLoad];
      // Do any additional setup after loading the view.
      UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(50, 50, 100, 50)];
      label.text = @"Page 2";
      [self.view addSubview:label];
  }

  - (void)didReceiveMemoryWarning {
      [super didReceiveMemoryWarning];
      // Dispose of any resources that can be recreated.
  }

  @end
  ```
- 打开AppDelegate.m文件，在其中加入下面一个方法，创建了一个WMPageController对象
  ```
  - (WMPageController *) getPages {
    //WMPageController中包含的页面数组
    NSArray *controllers = [NSArray arrayWithObjects:[ViewController class], [ViewController2 class], nil];
    //WMPageController控件的标题数组
    NSArray *titles = [NSArray arrayWithObjects:@"体育新闻", @"娱乐新闻", nil];
    //用上面两个数组初始化WMPageController对象
    WMPageController *pageController = [[WMPageController alloc] initWithViewControllerClasses:controllers andTheirTitles:titles];
    //设置WMPageController每个标题的宽度
    pageController.menuItemWidth = 100;
    //设置WMPageController标题栏的高度
    pageController.menuHeight = 35;
    //设置WMPageController选中的标题的颜色
    pageController.titleColorSelected = [UIColor colorWithRed:200 green:0 blue:0 alpha:1];
    return pageController;
  ```
- 在AppDelegate.m的didFinishLaunchingWithOptions方法中，加入如下代码，将窗口的根视图控制器设置为这个导航栏
  ```
  - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    WMPageController *pageController = [self getPages];
    UINavigationController *navController = [[UINavigationController alloc] initWithRootViewController:pageController];
    self.window.rootViewController = navController;
    return YES;
  }

  ```

## 资源链接

- [Assets.xcassets 资料汇总](https://blog.csdn.net/seadogprogrammer/article/details/52170722)
- [Assets.xcassets 资料总结](https://blog.csdn.net/u012371575/article/details/78584996)
- [Assets.xcassets 使用说明](https://www.jianshu.com/p/c35ce599f7da)
- [Assets.xcassets 详细使用](https://www.cnblogs.com/W-Kr/p/5381750.html)
- [WMPageController 使用](https://juejin.im/post/5a3889bb518825127e745af5)
- [WMPageController 使用](https://www.jianshu.com/p/e2503fb3241b)
- [WMPageController 详细使用](https://blog.csdn.net/yubo_725/article/details/51159633)
- [WMPageController 注意事项](https://www.jianshu.com/p/c803761b232f)
- [pod使用](https://www.jianshu.com/p/5a74c0842cf2)
