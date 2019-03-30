# week3 Report
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
第一周设计模式没有细讲，所以这周自己学习一下



