# week3 Report
16340164 吕雪萌

自由实训，对前两周项目的基础进行消化，重新阅读file_explore demo。

## 基础知识整理
对objective-C基础模糊不理解的地方梳理。

1. 类的定义
```objective C
@interface SimpleClass:NSObject
@end

@implementation SimpleClass:NSObject
@end
```
在.h中利用关键字声明@interface一个类,放成员变量，属性，方法声明，不参与编译。注意‘：’为继承关系，NSObject是父类。不允许多继承。
在.m中实现类的方法属性，注意所有对象类型的成员变量要打* ，基本类型不用 ，实质是指针，利用[]调用，调用本质是给这个对象/类发消息。共有成员用‘->’调用。

2. 为什么在.m文件中还要定义一次？
**在.h声明的方法公开，在.m声明的方法私有**

3. 类的方法
+表示类的方法；
-表示对象/实例的方法； 

4. 协议和类别（非正式协议）

- 协议定义：Objective-C中，不支持多继承，即不允许一个类有多个父类，但是OC提供了类似的实现方法，也就是协议。协议有点类似于Java里的接口，不同点就是在协议里，可以提供可选的方法，不要求全部继承。

- 协议：‘<protocolname>’其声明一个方法列表，协议的实现者需要实现声明的方法，可以使用@required和@optional关键字指定方法必须实现或可选实现，默认为必须实现。子类会继承其父类采用的协议，一个协议本身也可以采用别的协议。

- 类别：'(附加的类别名)'是Objective-C的一个*语言特点*，可以让你在无需子类化的前提下为一个类增加方法。凡是NSObject或其子类的类别，都是非正式协议。一个类不必实现非正式协议中的每个方法，只需实现它想要的方法就可以。声明非正式协议的类在向某个目标对象发送协议消息之前，必须首先向它发送respondsToSelector:消息并得到肯定的回答，才可以使用。需要.m文件。

- 扩展/匿名类别‘()’只需要.h文件。
```objective C
@interface ClassName(){

　　//实例变量--只能在自身类内使用

}

//方法定义--在.h文件中声明为公开的，在.m中声明的方法为私有的。

@end
```
## FileExplore 文件浏览器
功能：
首页tabBarController，第一个页面文件浏览，第二个页面‘关于’内容随意
文件浏览展示当前目录内容，文件夹可以展开，present显示文件属性

### 实现逻辑，重要代码

- tableview窗口来显示文件列表，dir保存根目录，两个可变数组记录目录和文件。
  tableView的创建
```
//导航栏项的title
    self.navigationItem.title = self.directory;
//创建tableView
    self.tableView = ({
        UITableView *tableView = [[UITableView alloc] initWithFrame:self.view.bounds style:UITableViewStylePlain];
        tableView.delegate = self;
        tableView.dataSource = self;
        tableView;
    });
    [self.view addSubview:self.tableView];
```

- 第一页文件浏览页实现协议<UITableViewDelegate, UITableViewDataSource>中的部分方法,前者实现页面的导航，后者产生每一行的tableViewCell。
导航逻辑：如果选中的path是目录，再产生一个文件目录的类（当前的类），根目录是这个path,push这个页面到导航控制器。如果path不是目录，产生一个文件信息类，并导航。
```
    if (isDirectory) {
        NSString *path = [self.directory stringByAppendingPathComponent:self.directoryList[indexPath.row]];
        FileExploreViewController *controller = [[FileExploreViewController alloc] initWithDirectory:path];
        controller.hidesBottomBarWhenPushed = YES;
        [self.navigationController pushViewController:controller animated:YES];
    } else {
        NSString *path = [self.directory stringByAppendingPathComponent:self.fileList[indexPath.row]];
        FileInfoViewController *controller = [[FileInfoViewController alloc] initWithFilePath:path];
        UINavigationController *navController = [[UINavigationController alloc] initWithRootViewController:controller];
        [self presentViewController:navController animated:YES completion:nil];
    }
```

- 设置cell的例子
```
NSString *cellID = ...;
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellID];
    if (nil == cell) {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:cellID];
    }
    cell.imageView.image = [UIImage imageNamed:@"..."];
    cell.textLabel.text = self.directoryList[indexPath.row];
    cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
    
```


## 常用设计模式
第一周设计模式没有细讲，所以这周自己学习一下常用的设计模式。

### 观察者模式

- 定义：多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。这种模式有时又称作发布-订阅模式、模型-视图模式，它是对象行为型模式。

- 优点：
1. 降低了目标与观察者之间的耦合关系，两者之间是抽象耦合关系。
2. 目标与观察者之间建立了一套触发机制。

- 缺点：
1. 目标与观察者之间的依赖关系并没有完全解除，而且有可能出现循环引用。
2. 当观察者对象很多时，通知的发布会花费很多时间，影响程序的效率。

### 单例模式

- 定义：指一个类只有一个实例，且该类能自行创建这个实例的一种模式。

- 特点：
1. 单例类只有一个实例对象;
2. 该单例对象必须由单例类自行创建;
3. 单例类对外提供一个访问该单例的全局访问点;

### 适配器模式

- 定义：将一个类的接口转换成客户希望的另外一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。

- 优点：
1. 客户端通过适配器可以透明地调用目标接口。
2. 复用了现存的类，程序员不需要修改原有代码而重用现有的适配者类。
3. 将目标类和适配者类解耦，解决了目标类和适配者类接口不一致的问题。

- 缺点：更换适配器的实现过程比较复杂。

### 原型模式

- 定义：用一个已经创建的实例作为原型，通过复制该原型对象来创建一个和原型相同或相似的新对象。

### 工厂方法

- 定义：一个创建产品对象的工厂接口，将产品对象的实际创建工作推迟到具体子工厂类当中。
- 优点：
1. 用户只需要知道具体工厂的名称就可得到所要的产品，无须知道产品的具体创建过程;
2. 在系统增加新的产品时只需要添加具体产品类和对应的具体工厂类，无须对原工厂进行任何修改，满足开闭原则;
- 缺点:
1. 每增加一个产品就要增加一个具体产品类和一个对应的具体工厂类，这增加了系统的复杂度。
