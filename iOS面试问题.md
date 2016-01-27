# iOS面试问题
## 语言基础
* property修饰词的含义和使用场景（assign,weak, copy,strong,nonatomic）？
* 如何定义一个对外只读，对内可读可写的属性。
* .h中的成员变量，外部是否可以直接访问。
* Category和Protocol可以定义属性吗？如何实现？
* ARC是为了解决什么问题诞生的，原理是什么？它是运行时特性吗？
* 描述一个你遇到的循环引用的场景，如果一个block执行时间很长，执行过程中weak对象被释放，有什么方法可以避免这个问题发生。

##Cocoa框架
* ViewController生命周期？viewWillLayoutSubviews是什么时候调用，如果使用AutoLayout，调用的顺序是什么样的？如果要在viewController中手动添加AutoLayout，放在哪个函数比较好？View中呢？
* UIView和CALayer有什么关系？
* 手势（UIGestureRecognizer）是否能同时响应？如果需要同时响应怎么办？
* iOS中事件响应链是什么样的？
* 是否使用过AutoLayout第三方库，写一个简单的例子，比如Mansory？
* 如何自定义viewController转场动画？
* 简述iOS沙盒目录结构？
* UIScrollView的contentSize和CotentOffset的原理是什么？
* 如何使一个Button的可点击区域大于它的可见区域？

##网络相关
* 简述NSURLConnection和NSURLSession区别。
* 简述AFNetworking实现大致思路。

##持久化和数据库
* iOS有哪些持久化方法？
* 经典的CoreData基础结构是什么样的？

##多线程
* GCD中队列有哪两种类型？并行队列有哪些类型？
* NSOperationQueue和GCD有什么区别？
* NSOperationQueue中已经开始执行的任务是否可以取消？
* 如何用GCD同步若干个异步调用？
* dispatch_barrier_async的作用是什么？

##底层和Runtime
* 简述NSRunLoop是什么，平常使用时需要注意哪些问题？
* 为什么其他语言叫函数调用，Objective-C叫给对象发送消息？
* +(void)load; +(void)initialize；有什么用处？
* Method Swizzing?
* Category定义的方法和类本身方法重名，使用时会调用哪个方法？为什么？

##设计模式
* 说说对MVC设计模式的理解，MVVM呢？
* 说一说Block，Notification，Delegate，KVO的区别，和具体使用场景？

##调试
* BAD_ACCESS在什么情况下出现？怎么解决？
* LLDB的调试语句用过哪些？
* 如何利用Instruments进行代码检查和性能调优?

##手写代码或描述思路
* 如何绘制一个圆形？
* 如何实现一个view折叠展开效果（打开书本的效果）？
* 两个view纵向放置并居中，如何实现？一个view呢？
* 三个view，宽度相等，间距相等但可变，如何利用AutoLayout实现？
* 如何实现一个跟随手指移动的view？
* 如何实现一个Button的Badge？
* 如何实现一个图片循环的效果？
* 如何实现下拉刷新效果？
* 手写代码：从当前view获取一个子imageView，从当前view获取第一个子imageView
* 给定图片的URL，如何实现图片获取并且显示？
* 如何实现UITableView cell重用机制？
* 如何设计一个图片缓存系统？

##开源框架
* AfNetworking
* SDWebImage
* ReactiveCocoa
* ReactNative
* POP

##其他
* iOS工作至今最记忆犹新的一个问题是什么？如何解决的？
* 平时从哪些网站了解到iOS最新资讯，前沿技术和优秀实践？
* 如何优雅的使用Google？


