# 浅谈Notification，Delegate，KVO，Block的使用场景

近儿换工作，几次面试都问及对Notification，Delegate，KVO，Block的理解，这里谈一些粗浅的看法，欢迎一起讨论，补充。

在iOS开发中，我们经常遇到以下问题：如何在不同的对象中传递信息？如何在完成某些操作后执行回调？苹果给我们提供了下面四种方法：

* Notification
* Delegate
* KVO
* Block

上面四种方式都可以使对象之间有很好解耦，增加代码复用性和简洁度，但有不同的使用场景。

###Notification
Notification是 `观察者－被观察者` 模式，其允许一些对象成为某个事件的观察者，当事件发生时，这些观察者对象会被通知，从而满足不同对象之间通信的目的。

* 优势：
	* 可以同时向多个对象（观察者）发送通知，让多个对象同时响应变化。
	* 代码相比Delegate简单，易于实现。
* 劣势：
	* 不利于调试，通知流程很难跟踪。
* 使用场景：
	* 一个事件需要多个对象感知及响应变化，比如系统级别的通知，键盘弹出，屏幕旋转等，可能需要多个viewController或view响应通知从而改变UI。

###Delegate
Delegate是`委托人－被委托人`模式。委托对象（委托人）要制订一套规则（协议@protocol），如要成为被委托对象，需要遵循这个协议，并实现这个协议中必需的方法。这样委托对象在需要与被委托对象通信时可以直接调用协议中的方法。

* 优势
	* 代码流程清晰，易于调试
	* 相比Notification有一个很大的优点，Delegate是双向的，委托人调用协议方法后可以接收返回值，而Notification是单向的。
* 劣势
	* 代码实现繁杂。
	* 一个被委托对象中假设有多个相同的委托对象，那么很难区分是哪一个委托对象调用了协议方法（虽然也有办法－ －），这时候Block就能很好的解决此问题。
* 使用场景
	* 抽象的说，一个对象有自己的使用模式和场景，如果另外一个对象要使用此对象，则需要遵循这种规则，而具体的模式和场景是一系列事件或者过程（比如UITableviewDelegate和UITableviewDataSourse），不是Notification这样的单一事件。

###KVO
Key Value Observing其实也算是一种Notification（具体实现机制皆然不同，之后再说说KVO的实现原理），不同的是其观察对象的某个属性，而不是某个事件。

* 优势
	* 相比Notification有一个很大的优势，其不需要对被观察对象内部进行任何修改（Notification需要被观察对象主动postNotification），这样可以很方便的监听一个黑盒对象（比如系统对象）的状态变化。
* 劣势
	* 为观察属性而生，暂时没想到啥劣势－ －。
* 使用场景
	* 如优势所述，可以观察黑盒对象的状态变化，比如一个UIView可以观察NSOperation的finished属性。
	* View-Modal场景下，View观察Modal的属性变化从而展示不同的UI。

###Block和Delegate
自从iOS 4.0增加了Block后，极大的方便了我们编写代码，特别是在一些需要回调的场景下，block实现简洁高效，取代了很多Delegate的使用场景。比如：

``` objective-c
#import <UIKit/UIKit.h>

@protocol LSViewDelegate <NSObject>

@required
- (void)loginButtonClicked:(id)sender;

@end

@interface LSView : UIView

@property (nonatomic, weak) id<LSViewDelegate> delegate;

@end
```
原先要在viewController实现一个subView Button的点击事件回调，需要用Delegate。现在可以很方便的使用Block。

``` objective-c
#import <UIKit/UIKit.h>

@interface LSView : UIView

@property (nonatomic, copy) void (^loginButtonHandler)(id);

@end
```
那么，问题来了，是否所有使用Delegate的场景都可以被Block替换呢？通过编程实践，观察和Google搜索，发现有下面几个方面：

1. 从设计模式上来说，Block无法取代Delegate。Delegate的协议要求被委托对象遵循某些规则后才可以合理的使用委托对象的所有特性。而Block从设计模式上看是一个简单的回调代码块。
2. 具体使用场景，Delegate注重过程，Block注重结果。我们可以观察苹果自家的SDK，比如UIWebviewDelegate，询问是否发起请求（shouldStartLoadWithRequest），通知请求已经开始（webViewDidStartLoad），通知请求已经完成（webViewDidFinishLoad），都是一个请求过程中的不同事件。相对来说，我们平常使用Block，多是某个简单事件或是操作结束的回调，比如上面代码片段的例子，一个Button点击的回调，再比如AFNetworking中大量使用Block作为请求完成后的回调。
3. 因为Block需要拷贝外部使用对象，所以Block相比Delegate性能上不具有优势。
4. Block的使用难度比Delegate高，有坑，比如Block中的循环引用。

总结来说，Delegate注重一个对象遵循某种模式和一系列事件中的不同阶段。而Block能在一些简单事件（比如点击事件）回调中起到很好的效果，同时能很好的作为一个操作结束的回调（比如一个网络请求方法，不同的请求可能需要不同的回调，可以在Block中写不同的代码）。


####参考
1. [When to use Delegation, Notification, or Observation in iOS](http://blog.shinetech.com/2011/06/14/delegation-notification-and-observation/)
2. [BLOCKS OR DELEGATES](http://blog.stablekernel.com/blocks-or-delegates/)


