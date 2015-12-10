# O2O孵化项目iOS代码规范

版本 | 日期 | 修改人 | 修改内容
----- | ----- | ----- | --------------------
1.0 | 2015.12.17 | 林盛 | 初稿

========
[TOC]

## 0 基本原则
* 求同存异。每个人的编码风格不同，没有一个定性的标准，尊重每位开发人员的编码习惯，本规范皆在提供一些建议，供开发人员参考。
* 本规范继承自苹果官方代码规范[Coding Guidelines for Cocoa](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html#//apple_ref/doc/uid/10000146-SW1)，是官方规范的子类，阅读本规范前请详细阅读官方文档。
* 注意！写代码时，母语是英语，慎用拼音！

## 1 项目结构
参考下图，自由发挥，原则：结构清晰，便于开发人员快速定位源文件位置。
![项目结构](media/14494596330082/%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84.png)


## 2 命名规范

### 2.0 基本原则
* 在清晰明确的基础上保持命名的简洁，尽量不使用缩写，仿照Cocoa风格使用长命名。保持整个Cocoa风格的一致性，比如`tag`是`UIView`,`UIControl`都用到的属性，在自定义的类中可以继续使用`tag`命名，不需要使用`viewTag`等。
* 所有命名需添加模块名前缀，比如`O2OMovie`,`O2OYummy`,`O2OUtilities`。

### 2.1大小写规则
* 类名采用大驼峰（UpperCamelCase），每个英文单词的首字母大写。
* 类成员、方法小驼峰(lowerCamelCase)，开头英文单词的首字母小写，其余单词首字母大写。
* 局部变量大小写首选小驼峰，也可以使用小写下划线的形式(snake_case)
* C函数的命名用大驼峰。
* 以上规则的基础上，如果开头的英文单词是国际通用的缩写，比如`URL`,那么首字母可以保持大写。

### 2.2 类名
* 类名需要以当前模块名作为前缀，比如`O2OMovieRootObject`。
* 继承`UIView`时以`View`作为结尾，继承`UIViewController`时以`Controller`作为结尾，所有`Model`对象都以`Model`作为结尾。

### 2.3 协议名
* 命名良好的协议名具有表示性，能让开发者识别出这不是一个类名，所以一般以`delegate`，`ing`作为结尾，比如`UITableViewDelegate`，`NSCopying`，`NSCoding`。

### 2.4 方法名
* 对于代表一个类动作的方法，以动词开头。不要使用`do`或`does`这样语义不明的动词。同时，不要使用副词和形容词。

``` objective-c
- (void)invokeWithTarget:(id)target;
- (void)selectTabViewItem:(NSTabViewItem *)tabViewItem
```

* 协议方法命名以协议相关类作为开头，并作为入参。同时辅以`will`，`did`, `should`来表示协议调用时的状态。

``` objective-c
- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
- (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;
- (void)scrollViewDidScroll:(UIScrollView *)scrollView;
```

* 以get、set开头的方法是属性值设置和获取方法，不要随便定义。
	* set是属性默认的设置方法，如果函数不是为了设置类成员，则不要用set开头，可用setup替代；
	* 获取属性值，直接以变量名为名，如：userInfomation，而不是getUserInfomation。

``` objective-c
- (NSSize)cellSize;			//Right.
- (NSSize)calcCellSize; 	//Wrong.
- (NSSize)getCellSize;		//Wrong.
```

* 对于私有方法，可以通过添加前缀加以区分。

``` objctive-c
- (void)LS_addObject:(id)object;
```

* 不要使用`and`连接多个入参，这会造成方法名过长，但是如果方法描述两个独立的动作，那么可以用`and`连接它们。

``` objective-c
- (BOOL)openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag;
```

### 2.5 变量名
* 对于一些特殊类型的变量，命名时建议带上类型，如`NSArray`的变量命名为`xxxArray`或`xxxList`，其他的类型可同样命名，如`xxxSize`、`xxxDictionary`（可简写为`xxxDic`）等。
* 对于常用系统的UI控件，建议命名时以控件类型名作为后缀，比如`showButton`，`usernameLabel`。
* 循环控制变量可使用单一的字符，如`i`、`j`、`k`等，也可以使用有意义的名字，如`index`，建议使用后者。
* 私有实例变量前加一个下划线，如`_myPrivateVarible`。
* 表示引用的`*`号属于变量`NSString *text`，不是`NSString* text`或`NSString * text`，定义常量时除外。

### 2.6 常量名
定义常量时需声明为静态常量，不建议使用`#define`

```objective-c
static NSString * const O2OMovieModuleName = @"O2OMovie";
static CGFloat const O2OMovieHomeViewHeaderViewHeight = 50.0;
```

### 2.7 通知名
基本命名格式是：`[与通知相关的类名] + [Did | Will] + [UniquePartOfName] + Notification`，例如：

```objective-c
NSApplicationDidBecomeActiveNotification
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
```

## 3 编程规范
### 3.1 代码结构
* 在.m文件中使用`pragma mark -`对方法进行分组区分，可以将方法种类作为区分为度。建议把属性的Setter和Getter方法放在文件的末段，以便查阅文件前段的业务逻辑代码。

``` objective-c
#pragma mark - Lifecycle Methods

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Public Methods

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - Private Methods

- (void)doSomething:(NSString *)inputString {}

#pragma mark - Event Response Methods

- (IBAction)submitData:(id)sender {}

#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - Setter & Getter Methods

- (void)setProperty:(id)value {}

```

### 3.3 空格，空行和换行
* 使用`Tabs`键，不使用空格，一个`Tabs`设置为四个空格。
* 单目操作符不应与它们的操作数分开（如`!`和`^`等），双目操作符应与它们的操作数用空格隔开。
* `@property`后留1个空格，`()`里面，逗号紧跟前一变量，与后一变量之间留一个空格。`()`外面，先留1个空格，再声明属性。

``` objective-c
@property (nonatomic, copy) NSArray *seatDataArray;
```

* 类方法声明在方法类型与返回类型之间要有空格。

```objective-c
- (void)updateDataSourceWithModelArray:(NSArray *)modelArray {}
```

* `if、else、else if、for、while、do`等语句自占一行，执行语句不要紧跟其后。不论执行语句有多少都要加`{ }`，防止书写失误，易于阅读。
* 空行起着分隔段落的作用，适当的空行可以使程序的布局更加清晰，比如每个方法之间增加空行。
* 关于大括号，推荐左括号和当前语句保持一行，右括号单独一行标示代码段结束。

``` objectiv-c
  - (void)methodName:(NSString *)string {
   ↑空格                               ↑空格，推荐花括号在一行
      if () {
	空格↑  ↑空格，花括号不要另起一行
      } else {
	空格↑    ↑空格，花括号不要另起一行
      }    
  }
```
* 关于换行，google推荐每行最多80个字符，可以在`Xcode => Preferences => TextEditing => Show Page Guide`设置换行提醒，目前建议超过100个字符，拆分新行进行缩进和重拍版。

### 3.4 枚举
推荐采用Cocoa API的普遍格式，因为该格式有更严格的类型检查和代码自动填充。

``` objective-c
typedef NS_ENUM(NSInteger, O2OMovieMenuTopItemType) {
	O2OMovieMenuTopItemMain,
	O2OMovieMenuTopItemShows,
	O2OMovieMenuTopItemSchedule
};
```

### 3.5 Block
block需注意循环引用的问题，推荐按如下方式处理。

``` objective-c
//全局文件中定义
#define WeakSelfDeclaration     __weak typeof(self) weakSelf = self;
#define StrongSelfDeclaration   typeof(weakSelf) strongSelf = weakSelf;

//使用时
WeakSelfDeclaration
self.block = ^() {
	StrongSelfDeclaration
	strongSelf.property = XXXXX
}
``` 

### 3.5 第三方库
我们的模块以静态库的形势存在云掌上生活中，OC又没有命名空间的概念，所以需尽量减少项目中第三方库的引用，能不引用就不引用。如引用了第三方库，需要更换第三方库所有的类名，文件名和category方法名，避免合入掌上生活主线后出现符号重定义问题。

### 3.6 日志
调试日志不建议使用普通的`NSLog`，建议设置`Debug`开关生产环境下不添加`NSLog`。如下

``` objective-c
#ifdef DEBUG
// Only log when attached to the debugger
#    define DLog(...) NSLog(__VA_ARGS__)
#else
#    define DLog(...) /* */
#endif
// Always log, even in production
#define ALog(...) NSLog(__VA_ARGS__)
```

## 4 注释规范

由于Cocoa长命名风格具有很有的代码自解释性，所以iOS开发中代码注释并不是特别重要。是否添加代码注释依具体场景和个人风格而定，但是下面几个场景建议添加代码注释：

1. 复杂业务逻辑函数，建议将业务逻辑用通俗的文字描述出来添加在函数注释中。业务逻辑函数是变化最频繁但又最复杂的函数，添加注释有利于开发人员理清思路并保持该业务代码的可维护性。
2. 对外提供的API，建议在注释中定义好入参和返回值。（可使用[VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode)这个插件）
3. 可以通过一些标注，提示自己之后有些事情是需要处理的，防止遗漏。

``` objective-c
// TODO: 处理异常情况
// FIXME: 此处需要清楚缓存
// !!!: 此处逻辑不严谨，待完善
// ???: 这块的实现上还存在难度，待研究跟进
```

## 5 其他

* 编写viewController代码时，将viewController代码量控制在500行以下，如果超出500行，那么这个viewController中的代码一定可以拆分到别的地方。
* 处理时间和日期的代码，注意`NSDateFormatter`的使用，如频繁`alloc`十分耗费性能。
* 一个代码[风格纠错](https://github.com/ChenYilong/iOSInterviewQuestions/blob/master/01%E3%80%8A%E6%8B%9B%E8%81%98%E4%B8%80%E4%B8%AA%E9%9D%A0%E8%B0%B1%E7%9A%84iOS%E3%80%8B%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88/%E3%80%8A%E6%8B%9B%E8%81%98%E4%B8%80%E4%B8%AA%E9%9D%A0%E8%B0%B1%E7%9A%84iOS%E3%80%8B%E9%9D%A2%E8%AF%95%E9%A2%98%E5%8F%82%E8%80%83%E7%AD%94%E6%A1%88%EF%BC%88%E4%B8%8A%EF%BC%89.md#1-%E9%A3%8E%E6%A0%BC%E7%BA%A0%E9%94%99%E9%A2%98)的例子，很有借鉴意义，值得学习。

## 6 参考文档
 
 * [Coding Guidelines for Cocoa](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html#//apple_ref/doc/uid/10000146-SW1)
 * [The official raywenderlich.com Objective-C style guide](https://github.com/raywenderlich/objective-c-style-guide)
 * [GitHub objective-c-style-guide](https://github.com/github/objective-c-style-guide)
 * [Google Objective-C Style Guide](https://google.github.io/styleguide/objcguide.xml)
 * [NYTimes Objective-C Style Guide](https://github.com/NYTimes/objective-c-style-guide)
 * [Objective-C 代码规范](https://www.zybuluo.com/FoxBabe/note/84957)






