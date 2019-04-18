# week5 周记
本周讲解了3种实训项目方向：Feeds流, IM,MV录歌 。小组选定信息流方向，所以着重讲解Feed流。

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
之后的每次加载操作（滚动/点击）中，依次执行数据截取、DOM 渲染、插入结构的过程，直至最后一页。

  - 流程图
  
  ![](https://misc.aotu.io/Yettyzyt/2017-06-27-infinite-scrolling/fontend_pagination.png)
  
  - 后端为前端提供接口 
  ```diviner.jd.com/diviner?p=610009&callback=jsonpCallbackMoreGood&lid=1&lim=100&ec=utf-8```

## IM聊天APP

### 主要功能

消息收发，消息保存,联系人管理，聊天管理。

网络通信，消息收发，本地存储（维护一个列表），联系人管理，会话管理（seq=5 seq=7），高性能。

### 架构设计
![](https://github.com/lvxm0/weekReport/blob/master/1.PNG)
![](https://github.com/lvxm0/weekReport/blob/master/2.PNG)
![](https://github.com/lvxm0/weekReport/blob/master/3.PNG)



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
  核心类: 使用方式参考[链接](http://www.cnblogs.com/taoxu/p/8022957.html)
  
  * AVCaptureDevice代表了输入设备,例如摄像头与麦克风。
  * AVCaptureInput代表了输入数据源
  * AVCaptureOutput代表了输出数据源
  * AVCaptureSession用于协调输入与输出之间的数据流
  
- [实现录制视频并保存功能的实例](https://www.jianshu.com/p/81d17b92fb1b)



## 心得体会

非常感谢小T老师对职业发展规划的分享，尤其是对待现有工作的态度上，“现在的绩效是做给未来的老板看的”。
也很残酷的给出了现阶段，技术人员的年龄和应该匹配的职位，以及淘汰的年龄上限，感受到了一定的压力。
想要一直呆在圈子里，就要保持对新鲜事物的敏感度。以后要多关注技术交流会，大牛的讲座等等。一定要重视基础！





