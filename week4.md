# 第四周周记
16340164 吕雪萌

这一周主要理解数据存储方面的内容，IOS数据存储常用的五种方式
- XML 属性列表plist归档
- Preference 偏好设置
- NSKeyedArchiver归档
- SQLite 3
- Core Data


## 应用沙盒

应用沙盒就是文件系统目录。每个iOS应用都有自己的应用沙盒，与其他文件系统隔离。应用必须待在自己的沙盒里,其他应用不能访问该沙盒。

模拟器应用沙盒的根路径一般在这两个目录下: (apple是用户名, 6.0是模拟器版本)

    /Users/apple/Library/Application Support/iPhone Simulator/6.0/Applications
    /Users/用户名/资源库/Application Support/iPhone Simulator/6.1/Applications
    
获取沙盒根目录：
```Objective-C
NSString *home = NSHomeDirectory();
```
    
 **沙盒的结构分析**
 
 
 - Documents
 
  保存应用运行时生成的需要持久化的数据，iTunes同步设备时会备份该目录。例如 *游戏应用可将游戏存档保存在该目录*。
  利用沙盒根目录拼接"Documents"字符串:
```Objective-C
  NSString *home = NSHomeDirectory();
  NSString *documents = [home stringByAppendingPathComponent:@"Documents"];
  // 不建议采用，因为新版本的操作系统可能会修改目录名
 ```
  利用NSSearchPathForDirectoriesInDomains函数:
```Objective-C
// NSUserDomainMask 代表从用户文件夹下找
// YES 代表展开路径中的波浪字符“~”
NSArray *array = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory,NSUserDomainMask, NO);
// 在iOS中，只有一个目录跟传入的参数匹配，所以这个集合里面只有一个元素
NSString *documents = [array objectAtIndex:0];
```
  
 - Temp
 
    保存应用运行时所需的临时数据，使用完毕后再将相应的文件从该目录删除。应用没有运行时，系统也可能会清除该目录下的文件。iTunes同步设备时不会备份该目录。
```Objective-C
    NSString *tmp= NSTemporaryDirectory();
```

- Library/Caches

  保存应用运行时生成的需要持久化的数据，iTunes同步设备时不会备份该目录。一般存储体积大，不需要备份的非重要数据。
  可以利用沙盒跟目录拼接"Catches"字符串.
  也利用NSSearchPathForDirectoriesInDomains函数(将函数的第一个参数改为:NSCachesDirectory即可)
   
 - Library/Preference
 
  保存应用的所有偏好设置，iOS 的 setting (设置)应用会在该目录中查找应用的设置信息。iTunes同步设备时会备份该目录。
  通过NSUserDefaults类存取该目录下的设置信息。

## 属性列表
 
 属性列表是一种XML格式的文件，拓展名为plist。 如果对象是NSString, NSDictionary, NSArray, NSData, NSNumber等类型，就可以使用:writeToFile:atomiclly:方法直接将对象写到属性列表文件中。
 
 - 归档：将数据封装成字典
 ```Objective-C
NSMutableDictionary *dict = [NSMutableDictionary dictionary];
//例子
[dict setObject:@"15013141314" forKey:@"phone"];

[dict setObject:@"27" forKey:@"age"];

 ```
 - 归档：将字典持久化到Documents/stu.plist中
  ```Objective-C
  [dict writeToFile:path atomically:YES];
 ```
 - 读取plist，实例化NSDictionary对象
```Objective-C
NSDictionary *dict = [NSDictionary dictionaryWithContentsOfFile:path];
//例子
NSLog(@"phone:%@", [dict objectForKey:@"phone"]);

NSLog(@"age:%@", [dict objectForKey:@"age"]);

 ```
 
## 偏好设置

很多iOS应用都支持偏好设置，比如保存用户名，密码，是否自动登录等设置。iOS提供了一套标准的解决方案来为应用加入偏好设置功能。

每个应用都有个NSUserDefaults实例，通过它来存取偏好设置。

*UserDefaults设置数据时，不是立即写入，而是根据时间戳定时地把缓存中的数据写入本地磁盘。*
*所以调用了set方法之后数据有可能还没有写入磁盘应用程序就终止了。出现此问题，可以通过调用synchornize方法（[defaults synchornize];）强制写入。*


- 保存设置
```Objective-C
NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
//用户名，字体大小，自动登陆
[defaults setObject:@"JN521" forKey:@"username"];

[defaults setFloat:18.0f forKey:@"text_size"];

[defaults setBool:YES forKey:@"auto_login"];

```
- 读取设置
```Objective-C
NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];

NSString *username = [defaults stringForKey:@"username"];

float textSize = [defaults floatForKey:@"text_size"];

BOOL autoLogin = [defaults boolForKey:@"auto_login"];
```

## NSKeyedArchiver

 只有遵守了NSCoding协议的的对象才可以直接使用NSKeyedArchiver进行归档和恢复，如NSString, NSDictionary, NSArray, NSData, NSNumber等。

- encodeWithCoder: 归档对象时调用，指定如何归档对象中的每个实例变量。
```Objective-C
NSString *path = [NSString stringWithFormat:@"Documents/array.archive"];

NSArray *array = [NSArray arrayWithObjects:@"a",@"b",nil];

[NSKeyedArchiver archiveRootObject:array toFile:path];

```
- initWithCoder: 恢复对象时调用，指定如何解码文件中的数据为对象的实例变量。
```Objective-C
NSArray *array = [NSKeyedUnarchiver unarchiveObjectWithFile:path];
```

- 对多对象归档接档
    - 归档
    ```Objective-C
    [archvier encodeCGPoint:point forKey:@"kPoint"];  
    [archvier encodeObject:info forKey:@"kInfo"];  
    [archvier encodeInteger:value forKey:@"kValue"];  
    [archvier finishEncoding];  
    [data writeToFile:multiHomePath atomically:YES];  

    ```
    - 接档
    ```Objective-C
    NSMutableData *dataR = [[NSMutableData alloc]initWithContentsOfFile:multiHomePath];  
    NSKeyedUnarchiver *unarchiver = [[NSKeyedUnarchiver alloc]initForReadingWithData:dateR];  
    CGPoint pointR = [unarchiver decodeCGPointForKey:@"kPoint"];  
    NSString *infoR = [unarchiver decodeObjectForKey:@"kInfo"];  
    NSInteger valueR = [unarchiver decodeIntegerForKey:@"kValue"];  
    [unarchiver finishDecoding];  
    NSLog(@"%f,%f,%@,%d",pointR.x,pointR.y,infoR,valueR);  
    ```

注意 *如果父类也可以直接归档*：

应该在uencodeWithCoder:方法中加上一句 [super encodeWithCode:encode];确保继承的实例变量也能被编码，即也能被归档。

应该在initWithCoder:方法中加上一句 self = [super initWithCoder:decoder];确保继承的实例变量也能被解码，即也能被恢复。

## SQLite3

- SQLite3是一款开源的嵌入式关系型数据库: 可移植性好，易使用,，内存开销小
- SQLite3是无类型的，可以保存任何类型的数据到任意表的任意字段中
- SQLite3常用的4种数据类型: text 文本字符串 ，integer 整型值 ， real 浮点值 ，  blob 二进制数据(文件等)
- 在iOS中使用SQLite3，首先要添加库文件 'libsqlite3.dylib' 和导入主头文件 #import<sqlite3.h>

以学生表格举例，含有学号，姓名，年龄，分数。
创表: 

    create table if not exists student (id integer, name text, age inetger, score real) ;

删表:

    drop table if exists student;

插入数据(insert):

    insert into student (name, age) values ('LUO', 20) ;


更新数据(updata):

    pupdate student set name = 'jack', age = 10 ;  
    update student set age = 10 where age > 10 and name != 'jack' ; 


删除数据(delete):

    delete from student;
    delete from student where age <= 10 or age > 20 ;


## CoreData

Core Data框架提供了对象-关系映射(ORM)的功能，即能够将OC对象转化成数据，保存在SQLite3数据库文件中，也能将保存在数据库中的数据还原成OC对象。
在次数据操作期间,不需要编写任何SQL语句，使用此功能,要添加 CoreData.framework 和导入主头文件 <CoreDate/CoreData.h> 。


- NSManagedObject
    
    - 通过Core Data从数据库中取出的对象，默认情况下都是NSManagedObject对象
    - NSManagedObject的工作模式有点类似于NSDictionary对象,通过键-值对来存取所有的实体属性
    - setValue:forkey:存储设置属性值(属性名为key)
    - valueForKey:获取属性值(属性名为key)


- CoreData主要对象

    - NSManagedObjectContext:负责数据和应用库之间的交互(CRUD);
     
    - NSPersistentStoreCoordinator:添加持久化存储库(比如SQLite数据库);

    - NSManagedObjectModel:代表Core Data的模型文件;

    - NSEntityDescription：用来描述实体;
    
- Core Data 延迟加载
 
     Core Data不会根据实体中的关联关系立即获取相应的关联对象。

     如通过Core Data取出Person实体时，并不会立即查询相关联的Card实体，当应用真的需要使用Card时，才会查询数据库,加载Card实体信息。



## 心得体会

加深了对IOS数据存储的理解，对上课中比较模糊的知识点有了新的认识。所以又查了很多相关的博客和资料。
找到了不错的视频教程：https://study.163.com/course/courseMain.htm?courseId=1002858003&_trace_c_p_k2_=579a672e26c543b4ad5359ce402aba5d#/courseDetail?tab=1
清明节玩的很开心:)
