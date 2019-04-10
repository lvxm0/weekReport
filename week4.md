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

## 心得体会

最后，清明节玩的很开心:)
