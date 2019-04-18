# week5 周记

第五周介绍三种项目方向的功能及架构：

- Feeds流
- IM聊天
- MV录歌

小组选定信息流方向，着重整理Feed流相关内容，对其他方向简单查询资料。

## Feeds信息流资讯App

### 主要功能

多Tab列表展示，Feeds流展示，图片浮层，详情页展示，写操作。

* 多Tab展示：使用UIScrollView，Container View Controller, ChildViewController。实现功能：分页逻辑和下拉刷新。
* feed流展示：tableViewCell重用（纯文本，单图，多图）
* 图片浮层：使用 UIViewController Transitioning 实现图片动效，以及需要完成多图预加载。
* 详情页：实现评论分页，TableViewCell复用。
* 写操作（点赞评论）：要求信息同步，运用观察者模式。

### 相关知识

* [使用UIScrollView 实现下拉刷新](https://blog.csdn.net/jeffasd/article/details/51201698)

  通过在 UIScrollView 的 contentOffset 和 contentInset 来实现的，mjrefresh在处理开始刷新事件时使用的是动画来解决。
  ```
  
    // 根据状态做事情
    if (state == MJRefreshStateIdle) {
        if (oldState != MJRefreshStateRefreshing) return;
        
        // 保存刷新时间
        [[NSUserDefaults standardUserDefaults] setObject:[NSDate date] forKey:self.lastUpdatedTimeKey];
        [[NSUserDefaults standardUserDefaults] synchronize];
        
        // 恢复inset和offset
        [UIView animateWithDuration:MJRefreshSlowAnimationDuration animations:^{
            self.scrollView.mj_insetT += self.insetTDelta;
            
            // 自动调整透明度
            if (self.isAutomaticallyChangeAlpha) self.alpha = 0.0;
        } completion:^(BOOL finished) {
            self.pullingPercent = 0.0;
        }];
    }
  ```
* [feed流式分页实现](https://aotu.io/notes/2017/06/27/infinite-scrolling/index.html)

  - 流式分页有如下几个特点：

    - 通过滚动/上拉/点击等方式加载新一页
    - 无页码
    - 无上/下页按钮
    - 不可跳转至指定页面
    - pc端和移动端均有使用
  
  - 前端实现逻辑：在前端分页的实现中，通过接口一次性获取列表的所有内容，根据数据的总长度和每页需展示的个数计算总页数；
之后的每次加载操作（滚动/点击）中，依次执行数据截取、DOM 渲染、插入结构的过程，直至最后一页。流程图如下：
  
  ![](https://misc.aotu.io/Yettyzyt/2017-06-27-infinite-scrolling/fontend_pagination.png)
  
  - 后端为前端提供接口 
  ```diviner.jd.com/diviner?p=610009&callback=jsonpCallbackMoreGood&lid=1&lim=100&ec=utf-8```
  
  - 后端实现逻辑:后端分页的实现中，在加载时，前端通过页码来拉数据，若返回非空数组，则进行 DOM 渲染，插入接口的操作；若返回空数组，则说明当前请求的为最后一页的数据，无需再发送请求。流程图如下：
  
  ![](https://misc.aotu.io/Yettyzyt/2017-06-27-infinite-scrolling/backend_pagination.png)
  
* [图片浮层](https://www.jianshu.com/p/b83aefdc9519)
  - 可供参考的实现浮层的博客： https://juejin.im/post/5a54d08d518825734859d46d
  
  - 实现逻辑: 使用控件SCSuspendedView （可由Pod导入），实现遮挡或不遮挡层级下的ViewController。
  
  - 使用方法：在你要设置的VC的初始化方法里调用sc_makeSuspendedView方法，并在viewDidLoad之前，设置浮层配置信息sc_suspendedViewConfiguration属性。
  
* 点赞机制
  - [点赞动画实现](https://juejin.im/post/5c3eb6d1518825253b5ea544)
    - 实现逻辑: 实际就是用CABasicAnimation的keypath是path和CABasicAnimation的keypath是transform.scale的动画组合在一起作用于一个三角形上,并且一共创建6个三角形图形.
  - [信息同步](https://juejin.im/post/5b1a3a795188257d9417ac26)
  
    - NSPostWhenIdle：空闲发送通知 (当运行循环处于等待或空闲状态时，发送通知，对于不重要的通知可以使用)
    - NSPostASAP：尽快发送通知 (当前运行循环迭代完成时，通知将会被发送，有点类似没有延迟的定时器)
    - NSPostNow ：同步发送通知 (如果不使用合并通知和postNotification:一样是同步通知)

  - 设计模式：观察者模式，实现对象间一对多的依赖关系，可能用到：
  
    - NSNotification：方便 NSNotificationCenter 广播到其他对象的封装对象
    - NSNotificationCenter：管理通知调度表中对象的发送和接收通知
    
## IM聊天APP

### 主要功能

消息收发，消息保存,联系人管理，聊天管理。

网络通信，消息收发，本地存储（维护一个列表），联系人管理，会话管理（seq=5 seq=7），高性能。

### 架构设计
![](https://github.com/lvxm0/weekReport/blob/master/1.PNG)
![](https://github.com/lvxm0/weekReport/blob/master/2.PNG)
![](https://github.com/lvxm0/weekReport/blob/master/3.PNG)

### 相关知识

[即时通讯代码实现](https://www.cnblogs.com/taoxu/p/5486417.html)

[聊天页面实现](https://cloud.tencent.com/developer/article/1018256)

## 录歌APP

### 主要功能

下载音乐伴奏，录制歌曲，录制视频，编辑视频，合成MV，上传视频，互动点赞。

* 使用AVAudioSession Category(APIS)
* 使用AVAudioPlayer APIS
* 使用AVAudioRecorder APIS
* 使用AVCaptureSession Core Classes
* 使用AVFoundation Core Classes
* 使用NSURLSession APIS
* 使用NSMutabeURLRequest APIs

### 架构设计

![](https://github.com/lvxm0/weekReport/blob/master/8.PNG)
![](https://github.com/lvxm0/weekReport/blob/master/9.PNG)
![](https://github.com/lvxm0/weekReport/blob/master/10.PNG)

### 相关知识
附学习链接
- [AVFoundation 框架解析](https://www.jianshu.com/p/4db87a1c170e)

  VFoundation框架是ios中很重要的框架，所有与视频音频相关的软硬件控制都在这个框架里面，接下来这几篇就主要对这个框架进行介绍和讲解。
  想要使用框架，引入头文件
  ```
  #import <AVFoundation/AVFoundation.h>
  ```
- 核心类: 使用方式参考[链接](http://www.cnblogs.com/taoxu/p/8022957.html)
  
  * AVCaptureDevice代表了输入设备,例如摄像头与麦克风。
  * AVCaptureInput代表了输入数据源
  * AVCaptureOutput代表了输出数据源
  * AVCaptureSession用于协调输入与输出之间的数据流
  
- [实现录制视频并保存功能的实例](https://www.jianshu.com/p/81d17b92fb1b)
  
  步骤如下：
  -  创建AVCaptureSession对象。
  -  使用AVCaptureDevice的静态方法获得需要使用的设备，例如拍照和录像就需要获得摄像头设备，录音就要获得麦克风设备。
  -  利用输入设备AVCaptureDevice初始化AVCaptureDeviceInput对象。
  -  初始化输出数据管理对象，如果要拍照就初始化AVCaptureStillImageOutput对象；如果拍摄视频就初始化AVCaptureMovieFileOutput对象。
  -  将数据输入对象AVCaptureDeviceInput、数据输出对象AVCaptureOutput添加到媒体会话管理对象AVCaptureSession中。
  -  创建视频预览图层AVCaptureVideoPreviewLayer并指定媒体会话，添加图层到显示容器中，调用AVCaptureSession的startRuning方法开始捕获。
  -  将捕获的音频或视频数据输出到指定文件。




## 心得体会

非常感谢小T老师对职业发展规划的分享，尤其是对待现有工作的态度上，“现在的绩效是做给未来的老板看的”。

很残酷的给出了现阶段，技术人员的年龄和应该匹配的职位，以及淘汰的年龄上限，感受到了一定的压力。

想要一直呆在圈子里，就要保持对新鲜事物的敏感度，需要对这个行业保持热情和求知欲。

以后要多关注技术交流会，大牛的讲座等等。一定要重视基础！






