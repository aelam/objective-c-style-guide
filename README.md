#关于代码风格规范的内容整理

##参考资料:

[Apple: Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html#//apple_ref/doc/uid/10000146-SW1)

[github: Objective C Style Guide](https://github.com/github/objective-c-style-guide)

[Google: Objective-C Style Guide](http://zh-google-styleguide.readthedocs.org/en/latest/google-objc-styleguide/contents/)


##一些需要改进的地方

###常量


*  常量名（如宏定义、静态局部变量等）应该以小写字母 k 开头，使用驼峰格式分隔单词，如：`kInvalidHandle`，`kWritePerm`
*  枚举，和bitmask使用`NS_ENUM`, `NS_OPTIONS`, 并跟上EM前缀

  	```objc
		typedef NS_ENUM(NSInteger, UIViewAnimationTransition) {
    		UIViewAnimationTransitionNone,
	    	UIViewAnimationTransitionFlipFromLeft
	    	//...
 		};

		typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
    		UIViewAutoresizingNone                 = 0,
		    UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
		    UIViewAutoresizingFlexibleWidth        = 1 << 1,
	    	//...
		};

	  ```

*  不必要的宏定义使用静态局部变量代替。

	  ```objc
      	#define kAlertTag 100 // Not so cool
      	static NSInteger kAlertTag = 100; // cool
  	```

*  自定义通自定义Key 等使用extern,写在对应的类里面
	  ```objc
      extern NSString *const EMThemeChangedNotification;
	  ```


###类名

* 类名应该以EM开头，使用驼峰格式分隔单词，如：`EMBaseController`

###声明
* 不要暴露不需要的实例变量, 实例变量以下划线开头， 尽量写在m文件. 不需要让其他类知道的协议确认写在m文件.
  ```objc
	// EMBaseViewController.m
	@interface EMBaseViewController()<UIAlertViewDelegate>

	@property (nonatomic, strong) id uncoveredProperty;

	@end

	@implementation EMBaseViewController {
      `NSString *_secretIVar;`
  }

	@end
  ```

* C函数大写开头

	```objc
	void GHAwesomeFunction(BOOL hasSomeArgs);
	```

* 对象的命名参照Apple Code Guides, 如NSDictionary, NSArray

	```objc
	NSDictionary *stocks; //good

	NSDictionary *stocksDictionary // bad

	NSArray *stocksArray // bad

	```

* 不需要让其他类知道的协议确认写在m文件

  ```objc
    // EMBaseViewController.m
	@interface EMBaseViewController()<UIAlertViewDelegate> //good

	@end
	```

###方法命名
* 根据功能用#pragma marks 分组
* 方法开头小写
* `instancetype`代替`id`

	```objc
  + (instancetype)objectWithThing:(id)thing {}
  - (instancetype)init {}
  - (id)init{} // outdated
	```
* 准备淘汰的的方法加上`__deprecated`
* 尽量不要用get
* `BOOL`类型的属性get方法

  ```objc
  @property(nonatomic,getter=isUserInteractionEnabled) BOOL     userInteractionEnabled;
  ```


###类扩展名
* 类别的文件名应该包含被扩展的类名，如：`GTMNSString+Utils.h` 或`GTMNSTextView+Autocomplete.h`。
* 类别方法应该带上em前缀  `-[UIColor em_redColor]`


###变量换行和对齐方式说明
* 不同类型或关联性不强的成员变量之间可以空一行，相关联的变量之间不用空行。
* 成员变量对齐方式，一般情况下，变量名根据最长的变量类型空一格对齐。如果某个变量类型过长，则可以空一行。
* `*`号写在变亮名称的左边。
* 注释空一个写在;后面

###注释
* 类、协议等注释用//方式 写在类的声明上方
* 方法注释用`/**... */`方式 写在方法声明上方,并满足JavaDoc Style


###方法声明和定义

* 实例方法中 `-` 和返回类型之间须使用一个空格，参数列表中只有参数之间可以有空格。星号前需要留一个空格
方法应该像这样：

	```objc
	- (void)doSomethingWithString:(NSString *)theString
	{
	  //...
	}
	```


* 如果一行有非常多的参数，应该将每个参数单独拆成一行。如果使用多行，将每个参数前的冒号对齐。
  ```objc
  - (void)doSomethingWith:(GTMFoo *)theFoo
                     rect:(NSRect)theRect
                  interval:(float)theInterval
  {
    、、...
  }
  ```


* 当第一个关键字比其它的短时，保证下一行至少有4个空格的缩进。这样可以使关键字垂直对齐，而不是使用冒号对齐：
  ```objc
  - (void)short:(GTMFoo *)theFoo
      longKeyword:(NSRect)theRect
      evenLongerKeyword:(float)theInterval
  {
      ///  ...
  }
  ```


###文件目录规范
* 文件夹目录原则上按照功能模块区分，每个模块下面包含`model`、`view`、`controller`例如：
* 通用的类放在根目录的controller、model、view文件夹下
* 第三方api放在utility文件夹下
* 资源文件上放在`ymStockEM.bundle`或`ymStockPublic.bundle`中。如果是单独模块会在多个项目中用到，则可以在改模块文件夹下单独建一个bundle或res文件夹

###一些命名规则摘要
* 方法名含义要清晰明确

Code                  | Commentary    |
----------------------| ------------- |
insertObject:atIndex: | Good  
insert:at:            | Not clear; what is being inserted? what does “at” signify?  
removeObjectAtIndex:  | Good.
removeObject:         | Good, because it removes object referred to in argument.
remove:               | Not clear; what is being removed?


* 尽量不要使用缩写

Code                  | Commentary    |
----------------------| ------------- |
destinationSelection  | Good  
destSel               | Not clear
setBackgroundColor:   | Good.
setBkgdColor:         | Not clear

* 命名不要自我指涉？No Self Reference (Names shouldn’t be self-referential.)

Code                  | Commentary    |
----------------------| ------------- |
NSString              | Good  
NSStringObject        | Not clear





* 更详细的一些命名规则和方法 请查看苹果文档




###图片命名规则
* 通用图标, 功能开头，功能_名称_[状态]，如`btn_share_selected.png`。
* 模块图标, 先模块名称开头，模块_功能_名称_[状态]，如`login_btn_share_normal.png`
* 没有多状态的图片，不需要状态标记，如：`btn_share.png`
* 对于两个模块都用到的图标，就认为是通用图片，不需要加前缀模块。通用图标命名为主，少部分模块内需要区别于通用图标的，才使用模块内图标方式命名
* 命名尽量短，图片尽量复用。模块图标被复用后，得改名为通用图标命名方式，防止修改单个模块图标造成其他模块图标混乱


```
注：

大图片或基本不频繁复用的图片， 图片存储于EM.bundle。读取的时候采用文件读取的方式读取图片。
会频繁复用的图片，命名前使用前缀，图片存储于public.bundle。去读的时候通过名称读取，读取后系统会将图片缓存。


模块:
自选股:zxg
弹窗:alert
个股:stock
大势:ds
特色功能:tsgn
搜索:srch
系统:sys
资讯:inf
交易:trd
积分:jf
键盘:kb
首页:home
tabbar:tab
navigationbar:nav
弹出框:pop
主力:zl
开机帮助:help
预警:alm


功能：
背景图片：bg
标志性的图片我们取名为：logo；
在页面上位置不固定并且带有链接的小图片我们取名为btn ；
在页面上某一个位置连续出现，性质相同的链接栏目的图片我们取名：menu ；
装饰用的照片我们取名：pic ；
小标识(不知道怎么归类的):icon

状态后缀 	普通:_normal
			选中:_selected
			禁用:_disabled

segment背景图：segment
放置在页面顶部的广告、装饰图案等长方形的图片我们取名：banner ；
在页面上位置不固定并且带有链接的小图片我们取名为btn ；
不带链接表示标题的图片我们取名：title
有onmouse效果的图片，两张分别在原有文件名后加"_on"和"_off"命名。
```
