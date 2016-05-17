## 前言

Objective-C语言主要用于iOS和Mac开发，本文是根据苹果和Google规范然后结合网上一些程序规范整理得出的一系列编码规范。

[苹果 Objective-C规范]
(https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)

[Google Objective-C规范]
(http://zh-google-styleguide.readthedocs.org/en/latest/google-objc-styleguide/)
## 代码缩进和行代码

关于代码缩进Apple和Google有两种规范：Apple是以4个空格缩进，而Google以2个空格缩进。在此我们采用Apple的规范即4个空格缩进，但是不能使用TAB制表符，所以Xcode里面我们这样设置(见图1)，即以TAB代表4个空格，一次编写代码时候缩进的时候就用TAB键而不必自己输入空格。
 
![图1.png](http://upload-images.jianshu.io/upload_images/224953-2ac49875a4cfb835.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
关于每行代码最大长度我们选用100个字符作为最大长度限制，过长的代码会导致可读性问题，可以在Xcode设置(见图2)，编写的代码长度不可以大于这个最大值。

![图2.png](http://upload-images.jianshu.io/upload_images/224953-dd3f3ff8a4075396.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##  文件定义
#### 头文件引入

1.当引入Objective-C或者Objective-C++头文件的时候使用```#import```引入，当引入C或者C++头文件的时候使用```#include```引入
2.引入系统API时应该直接引用这个根头文件，而不是其它子模块的头文件，即使是你只用到了其中的一小部分，编译器会自动完成优化的。每一个框架都会有一个和框架同名的头文件，它包含了框架内接口的所有引用。例如：

```
//正确做法:
#import <UIKit/UIKit.h>
//错误做法 
#import <UIKit/UIButton.h>
```

3.引入系统的API时使用```#import <>```格式
4.引入第三方框架优先使用```#import <>```格式
5.引入自己定义的类使用```#import ""```格式
6.将 import和其他的文件名之间加一个空格。如果有一个以上的 import 语句，就对这些语句进行分组。每个分组的注释是可选的。 
注：对于模块使用 #import语法。除了子类化或是协议之外，最好使用 @class 这种方式，避免过多的头文件引入。在引入协议的时候，如果不是连当前类也引入的情况下，将协议单独声明出来再引入。

## 命名规范
#### 基本原则

所有的命名必须是英文规范，命名应该尽量的清晰和简洁。在Objective-C中，命名清晰重要程度要比简洁更重。

命名通知名称等一些key时必须用英文!

```
// 正确
NSString *kLogoutSuccessNotification = @"LogoutSuccessNotification";
// 错误
NSString *kLogoutSuccessNotification = @"退出登录";
```

所有修饰符例如```=, +, -, &, |```两边必须留有空格

```
// 清晰
insertObject:atIndex:
// 不清晰， insert 的对象类型和 at 的位置属性没有说明
insert:at:
```

但是有一些单词简写在 Objective-C 编码过程中是非常常用的,例如:

```
alloc == Allocate 
init == Initialize 
max == Maximum
min == Minimum
alt == Alternate 
app == Application 
msg == Message
calc == Calculate 
nib == Interface Builder archive
dealloc == Deallocate 
pboard == Pasteboard
func == Function 
rect == Rectangle
horiz == Horizontal 
vert == Vertical
Rep == Representation 
info == Information 
temp == Temporary
```

#### 一致性

整个工程的命名风格要保持一致性，最好和苹果 SDK 的代码保持统一。不同类中完成相似功能的方法应该叫一样的名字，比如我们总是用 count 来返回集合的个数，不能在 A 类中使用 count 而在 B 类中使用 getNumber

#### 前缀

Apple规定：Apple保留所有两个字母前缀的使用权，因此为了防止与官方API冲突，项目中所有文件的前缀必须是三个(及以上)大写字母。

在好有钱项目中，需要在前面加入OLW（历史遗留问题，正常应该是HYQ）


```
//正确
OLWFileUploadFailCell
HYQFileUploadFailCell
// 错误
// 不要使用两个前缀
OLWQZGFileUploadFailCell 
// 不要使用小写
OlwFileUploadFailCell
```

可以在为类、协议、函数、常量以及 typedef 宏命名的时候使用前缀，但注意不要为成员变量或者方法使用前缀，因为他们本身就包含在类的命名空间内。

#### 命名属性或实例变量（property）

定义一个属性时，编译器会自动生成线程安全的存取方法（ Atomic ），但这样会大大降低性能，特别是对于那些需要频繁存取的属性来说，是极大的浪费。所以如果定义的属性不需要线程保护，记得手动添加属性关键字 ```nonatomic``` 来取消编译器的优化。

变量名应该尽可能命名为描述性的。除了 for 循环外，其他情况都应该避免使用单字母的变量名。 星号表示指针属性变量，例如： NSString \*text 不要写成 NSString* text 或者NSString * text ，常量除外。

尽量定义属性来代替直接使用实例变量,同时声明内存的管理方式。如果一个属性只在 init 方法里设置了一次，声明为 readonly 。
格式如下：

```
@property (nonatomic, strong) ClassName *name;
```

注意：
 1.在 ```@property``` 后有一个空格
 2.修饰符之间用```, + 空格```隔开
 3.类名与“)”之间有一个空格
 4.如果是对象类型，属性名与```*```在一起并与类名之间隔开一个空格
 
定义NSString和Block类型时，使用copy。
定义基本数据类型时，使用assign
定义代理对象时，使用weak， 格式

``` 
@property(nonatomic, weak) id<UIAlertViewDelegate> delegate;
```
定义对象类型时使用strong
定义BOOL型属性时，应该表明getter方法

```
// 错误
@property (nonatomic, assign) BOOL isShow;
// 正确
@property (nonatomic, assign, getter=isShow) BOOL show;
```

在定义变量时有个两种方式：
第一种：

```
// 正确做法
{
    NSData *_data;
}
// 错误做法
{
    NSData *data;
}

``` 

第二种：

``` 
@property (nonatomic, strong) NSData *data;
```

建议采取第二种方式，因为第二种方式系统会默认生成getter和setter方法

#### 命名方法（ Methods ）

Objective-C 的方法名通常都比较长，这是为了让程序有更好地可读性，按苹果的说法 “ 好的方法名应当可以以一个句子的形式朗读出来 ” 。

在方法签名中，在 -/+ 符号后应该有一个空格。第一个大括号{的位置在函数所在行的末尾，同样应该有一个空格。方法片段之间也应该有一个空格。构造方法使instancetype作为返回类型来代替 id。

对于私有方法，应该加前缀用以区分。具体使用可以自行决定，建议使用p加下划线的方式：```p_``` , p表示"private"，不建议使用单个下划线的方式，这种方式是预留给苹果使用的。

方法一般以小写字母打头，每一个后续的单词首字母大写，方法名中不应该有标点符号（包括下划线），有两个例外：

可以用一些通用的大写字母缩写打头方法，比如 PDF,TIFF 等。
可以用带下划线的前缀来命名私有方法或者类别中的方法。
如果方法表示让对象执行一个动作，使用动词打头来命名，注意不要使用 do ， does 这种多余的关键字，动词本身的暗示就足够了：

```
// 动词打头的方法表示让对象执行一个动作
- (void)invokeWithTarget:(id)target;
- (void)selectTabViewItem:(NSTabViewItem *)tabViewItem;
```
如果方法是为了获取对象的一个属性值，直接用属性名称来命名这个方法，注意不要添加 get 或者其他的动词前缀：

```
// 正确，使用属性名来命名方法
- (NSSize)cellSize;
// 错误，添加了多余的动词前缀
- (NSSize)calcCellSize;
- (NSSize)getCellSize;
```

对于有多个参数的方法，务必在每一个参数前都添加关键词，关键词应当清晰说明参数的作用,不要用 and 来连接两个参数：

```
// 正确，保证每个参数都有关键词修饰
- (void)sendAction:(SEL)aSelector toObject:(id)anObject forAllCells:(BOOL)flag;
// 错误，遗漏关键词
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
// 正确
- (id)viewWithTag:(NSInteger)aTag;
// 错误，关键词的作用不清晰
- (id)taggedView:(int)aTag;
```

方法的参数命名也有一些需要注意的地方 :


* 和方法名类似，参数的第一个字母小写，后面的每一个单词首字母大写
不要再方法名中使用类似 pointer,ptr 这样的字眼去表示指针，参数本身的类型足以说明
* 不要使用只有一两个字母的参数名
* 不要使用简写，拼出完整的单词


下面列举了一些常用参数名：

```
...action:(SEL)aSelector
...alignment:(int)mode
...atIndex:(int)index
...content:(NSRect)aRect
...doubleValue:(double)aDouble
...floatValue:(float)aFloat
...font:(NSFont *)fontObj
...intValue:(int)anInt
...keyEquivalent:(NSString *)charCode
...point:(NSPoint)aPoint
...stringValue:(NSString *)aString
...target:(id)anObject
```

#### 函数调用
函数调用的格式和书写差不多，可以按照函数的长短来选择写在一行或者分成多行：

```
//写在一行
[myObject doFooWith:arg1 name:arg2 error:arg3];

//分行写，按照':'对齐
[myObject doFooWith:arg1
               name:arg2
              error:arg3];

//第一段名称过短的话后续可以进行缩进
[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];
                
```
                
以下写法是错误的：

```
//错误，要么写在一行，要么全部分行
[myObject doFooWith:arg1 name:arg2
              error:arg3];
[myObject doFooWith:arg1
               name:arg2 error:arg3];

//错误，按照':'来对齐，而不是关键字
[myObject doFooWith:arg1
          name:arg2
          error:arg3];
```
 
#### 命名基本数据类型
为了兼容32-bit和64-bit 当定义基本数据类型的时候应该采用苹果SDK提供的数据类型

```
int -> NSInteger
unsigned -> NSUInteger
float -> CGFloat
double -> CGFloat
动画时间 -> NSTimeInterval
…
```

#### 命名代理（ Delegate ）

当特定的事件发生时，对象会触发它注册的代理方法。代理是 Objective-C 中常用的传递消息的方式。代理有它固定的命名范式。

一个代理方法的第一个参数是触发它的对象，第一个关键词是触发对象的类名，除非代理方法只有一个名为 sender 的参数：

```
// 第一个关键词为触发代理的类名
- (BOOL)tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
- (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;
// 当只有一个 "sender" 参数时可以省略类名
- (BOOL)applicationOpenUntitledFile:(NSApplication *)sender;
// 根据代理方法触发的时机和目的，使用 should,will,did 等关键词
- (void)browserDidScroll:(NSBrowser *)sender;
- (NSUndoManager *)windowWillReturnUndoManager:(NSWindow *)window; 、
- (BOOL)windowShouldClose:(id)sender;
```

#### 命名常量（Constants）

不要使用 ```#define``` 宏来定义常量， ```#define``` 通常用来给编译器决定是否编译某块代码，比如常用的：

```
#ifdef DEBUG
```

使用 ```const``` 定义基本数据类型或字符串常量，常量的命名规范和函数相同,但是常量一般用k：

定义全局常量: 不管你定义在任何文件夹，外部都能访问

```
const NSString *title = @"title"
```

局部常量：用static修饰后，不能提供外界访问

```
static const CGFloat xxxx;
static const NSString *title = @"title";
```

如果定义公用的常量方式如下：

```
.h文件： extern NSString *const title; 
.m文件： NSString *const title = @"title";
```

#### 命名通知（ Notifications ）

通知常用于在模块间传递消息，所以通知要尽可能地表示出发生的事件，通知的命名范式是或者全用大写描述但是要用_分隔开：

```
[ 触发通知的类名 ] + [Did | Will] + [ 动作 ] + Notification 例如：
NSApplicationDidBecomeActiveNotification
NSWindowDidMiniaturizeNotification
NSTextViewDidChangeSelectionNotification
NSColorPanelColorDidChangeNotification
```

## 代码规范
#### 不要使用 new 方法

尽管很多时候能用 new 代替 alloc init 方法，但这可能会导致调试内存时出现不可预料的问题。 Cocoa 的规范就是使用 alloc init 方法，使用 new 会让一些读者困惑。

#### 新语法使用

在Objective-C新语法中，.h声明类中要在```@interface```和```@end```上下分别加上```NS_ASSUME_NONNULL_BEGIN```和```NS_ASSUME_NONNULL_END``` 并留有空格， 格式如下：

```
NS_ASSUME_NONNULL_BEGIN

@interface APPTextView : UIView

@end

NS_ASSUME_NONNULL_END
```

```NS_ASSUME_NONNULL_BEGIN```和```NS_ASSUME_NONNULL_END```  默认会对属性和方法参数及返回值增加```nonnull```，如果确定某个属性或者方法参数可以为空，请加上修饰符```nullable```

#### @public和@private标记符

@public和@private标记符应该以一个空格来进行缩进：

```
@interface MyClass : NSObject {
 @public
  ...
 @private
  ...
}
@end
```

#### 闭包（Blocks）

根据block的长度，有不同的书写规则：

* 较短的block可以写在一行内。
* 如果分行显示的话，block的右括号}应该和调用block那行代码的第一个非空字符对齐。
* block内的代码采用4个空格的缩进。
* 如果block过于庞大，应该单独声明成一个变量来使用。
* \^和(之间，\^和{之间都没有空格，参数列表的右括号)和{之间有一个空格。

```
//较短的block写在一行内
[operation setCompletionBlock:^{ [self onOperationDone]; }];

//分行书写的block，内部使用4空格缩进
[operation setCompletionBlock:^{
    [self.delegate newDataAvailable];
}];

//使用C语言API调用的block遵循同样的书写规则
dispatch_async(_fileIOQueue, ^{
    NSString* path = [self sessionFilePath];
    if (path) {
      // ...
    }
});

//较长的block关键字可以缩进后在新行书写，注意block的右括号'}'和调用block那行代码的第一个非空字符对齐
[[SessionService sharedService]
    loadWindowWithCompletionBlock:^(SessionWindow *window) {
        if (window) {
          [self windowDidLoad:window];
        } else {
          [self errorLoadingWindow];
        }
    }];

//较长的block参数列表同样可以缩进后在新行书写
[[SessionService sharedService]
    loadWindowWithCompletionBlock:
        ^(SessionWindow *window) {
            if (window) {
              [self windowDidLoad:window];
            } else {
              [self errorLoadingWindow];
            }
        }];

//庞大的block应该单独定义成变量使用
void (^largeBlock)(void) = ^{
    // ...
};
[_operationQueue addOperationWithBlock:largeBlock];

//在一个调用中使用多个block，注意到他们不是像函数那样通过':'对齐的，而是同时进行了4个空格的缩进
[myObject doSomethingWith:arg1
    firstBlock:^(Foo *a) {
        // ...
    }
    secondBlock:^(Bar *b) {
        // ...
    }];
```

#### 语法糖的使用

应该使用可读性更好的语法糖来构造NSArray，NSDictionary等数据结构，避免使用冗长的alloc,init方法。

元素之前要用空格分开

```
//正确，
NSArray *array = @[[foo description], @"Another String", [bar description]];
NSDictionary *dict = @{NSForegroundColorAttributeName : [NSColor redColor]};

//不正确，不留有空格降低了可读性
NSArray* array = @[[foo description],[bar description]];
NSDictionary* dict = @{NSForegroundColorAttributeName: [NSColor redColor]};
如果构造代码不写在一行内，构造元素需要使用两个空格来进行缩进，右括号]或者}写在新的一行，并且与调用语法糖那行代码的第一个非空字符对齐：

NSArray *array = @[
							  @"This",
							  @"is",
							  @"an",
							  @"array"
							];

NSDictionary *dictionary = @{
  NSFontAttributeName : [NSFont fontWithName:@"Helvetica-Bold" size:12],
  NSForegroundColorAttributeName : fontColor
};
```

构造字典时，字典的Key和Value与中间的冒号:都要留有一个空格，多行书写时，也可以将Value对齐：

```
//正确，冒号':'前后留有一个空格
NSDictionary *option1 = @{
  NSFontAttributeName : [NSFont fontWithName:@"Helvetica-Bold" size:12],
  NSForegroundColorAttributeName : fontColor
};

//正确，按照Value来对齐
NSDictionary *option2 = @{
  NSFontAttributeName :            [NSFont fontWithName:@"Arial" size:12],
  NSForegroundColorAttributeName : fontColor
};

//错误，冒号前应该有一个空格
NSDictionary *wrong = @{
  AKey:       @"b",
  BLongerKey: @"c",
};

//错误，每一个元素要么单独成为一行，要么全部写在一行内
NSDictionary *alsoWrong= @{ AKey : @"a",
                            BLongerKey : @"b" };

//错误，在冒号前只能有一个空格，冒号后才可以考虑按照Value对齐
NSDictionary *stillWrong = @{
  AKey       : @"b",
  BLongerKey : @"c",
};
```

#### BOOL 的使用

BOOL 在 Objective-C 中被定义为 signed char 类型，这意味着一个 BOOL 类型的变量不仅仅可以表示 YES(1) 和 NO(0) 两个值，所以永远不要将 BOOL 类型变量直接和 YES 比较：

```
// 错误，无法确定 |great| 的值是否是 YES(1) ，不要将 BOOL 值直接与 YES 比较
BOOL great = [foo isGreat];
if (great == YES)
// ...be great!
// 正确
BOOL great = [foo isGreat];
if (great)
// ...be great!
```

同样的，也不要将其它类型的值作为 BOOL 来返回，这种情况下， BOOL 变量只会取值的最后一个字节来赋值，这样很可能会取到 0（ NO ）。但是，一些逻辑操作符比如 &&,||,! 的返回是可以直接赋给 BOOL 的：

```
// 错误，不要将其它类型转化为 BOOL 返回
- (BOOL)isBold {
return [self fontTraits] & NSFontBoldTrait;
}
- (BOOL)isValid {
return [self stringValue];
}
// 正确
- (BOOL)isBold {
return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
}
// 正确，逻辑操作符可以直接转化为 BOOL
- (BOOL)isValid {
return [self stringValue] != nil;
}
- (BOOL)isEnabled {
return [self isValid] && [self isBold];
}
```

#### 逻辑判断的使用
关于逻辑判断一般就是if else 判断，在if else 判断中，必须加上```{ }```,无论是判断语句是一句 还是多句。判断应该先筛除不符合的逻辑，然后处理重点逻辑。如果判断的执行逻辑是```return```可以不写```{ }```，但是return必须与判断语句在一行，防止出错。
例如：

```
if (判断语句) return;
```

更多情况下应该是

```
if (判断) {
   // 逻辑代码
} else {
  // 逻辑代码
}
```

不要留有没有执行逻辑的判断

```
// 错误
if (i == 3) {

} else{
  // 执行逻辑
} 
//正确
if (i != 3) {
  // 执行逻辑
}
```

#### 在 init 和 dealloc 中不要用存取方法访问实例变量

当 init dealloc 方法被执行时，类的运行时环境不是处于正常状态的，使用存取方法访问变量可能会导致不可预料的结果，因此应当在这两个方法内直接访问实例变量。

```
// 正确，直接访问实例变量
- (instancetype)init {
 self = [super init];
 if (self) {
  _bar = [[NSMutableString alloc] init];
 }
 return self;
}
- (void)dealloc {
 [_bar release];
 [super dealloc];
}
// 错误，不要通过存取方法访问
- (instancetype)init {
 self = [super init];
 if (self) {
  self.bar = [NSMutableString string];
 }
 return self;
}
- (void)dealloc {
 self.bar = nil;
 [super dealloc];
}
```

#### 保证 NSString 在赋值时被复制

NSString 非常常用，在它被传递或者赋值时应当保证是以复制（ copy ）的方式进行的，这样可以防止在不知情的情况下 String 的值被其它对象修改。

```
- (void)setTitle:(NSString *)aTitle {
 _title = [aTitle copy];
}
```

#### nil 检查

因为在 Objective-C 中向 nil 对象发送命令是不会抛出异常或者导致崩溃的，只是完全的 “ 什么都不干 ” ，所以，只在程序中使用 nil 来做逻辑上的检查。

另外，不要使用诸如 nil == Object 或者 Object == nil 的形式来判断。

```
// 正确，直接判断
if (!objc) {
 ...
}
// 错误，不要使用 nil == Object 的形式
if (nil == objc) {
 ...
}
```

#### 命名枚举

项目中强烈建议用枚举替代魔法数字
项目中强烈建议用枚举替代魔法数字
项目中强烈建议用枚举替代魔法数字
(重要的事情说三遍)并且枚举项的注释也得完善
枚举类型优先采用Objective-C语法定义，格式如下：

```
/// 枚举注释
typedef NS_ENUM (NSInteger, QZGProfile) {
    QZGProfileIDCard = 0, //!< 枚举项注释
};
```

凡是需要按位或操作来组合的枚举都应该使用NS_OPTIONS定义。

```
/// 枚举注释
typedef NS_OPTIONS(NSUInteger, OKState) {
   OKState1 = 1 << 0,
   OKState2 = 1 << 1
 };
```

#### 魔法数字
项目中必须尽可能的避免魔法数字的出现,如果是一组数据,应当采用枚举方式.
如果用到tag,建议采用char类型定义,因为字符类型会对应ASCII表中的数字,比如'A','b'等,但是最多设置情况为四个字符,比如'ABCD'.(小技巧而已不必严格遵守)

####  其他
不要在对象类型前和 *protocol*之间添加空格。

```
// 推荐：
@property (nonatomic, weak) id<OKDelegate> delegate;

//反对：
@property (nonatomic, weak) id <OKDelegate> delegate; 
```

## Xcode 工程

为了避免文件杂乱，物理文件应该保持和 Xcode 项目文件同步。Xcode 创建的任何组（group）都必须在文件系统有相应的映射。为了更清晰，代码不仅应该按照类型进行分组，也可以根据功能进行分组。

## 注释

好的代码应该是 “ 自解释 ” （ self-documenting ）的，但仍然需要详细的注释来说明参数的意义、返回值、功能以及可能的副作用。方法、函数、类、协议、类别的定义都需要注释，推荐采用 Apple 的标准注释风格，好处是可以在引用的地方 alt+ 点击自动弹出注释，非常方便。
推荐使用 VVDocumenter

#### 文件注释

文件注释放在.h文件的头部，格式为：

```
/// @brief   对类的功能进行说明
/// @since   从哪个APP版本号开始使用和时间
/// @author  最近更改代码的人
```

#### 属性注释

1.如果注释内容过多使用格式：

```
/**
 * 很长很长的注释内容
 * 很长很长的注释内容
 */
 @property (nonatomic, strong) ClassName *name;
```

2.当注释内容不多时，注释使用```///```或```//!<```格式：

```
/// 注释内容
@property (nonatomic, strong) ClassName *name;
```

或者

```
@property (nonatomic, strong) ClassName *name; //!< 注释内容
```

#### 方法注释

1.方法注释在.h生命类里面应该用

```
/**
 * 方法功能说明
 * 
 * @param 参数说明
 * 
 * @return 返回值说明
 */
```

建议用VVDocumenter添加注释， 如果方法注释只有一行内容可以用```///```注释

2.使用 ```#pragma mark - 注释内容```或 ```#pragma mark 注释内容```  隔离块功能

## 警告与错误
关于警告与错误我专门分出来写，因为我认为应该对警告和错误有足够的重视。关于错误这个可以说的不多，如果编写的代码出错了，Xcode也会看不下去的，因此Xcode会给你一个修改提示。
对于警告，优秀的程序员提交的最终代码应该是不包含任何警告的，因此在编写的时候一旦出现警告，我们应该尽力去解决。如果实在避免不了可以用

```
#pragma clang diagnostic push
#pragma clang diagnostic ignored "警告类型"
  //要忽略的警告内容
#pragma clang diagnostic pop
```

这种方式来解除警告，但是这种方式不可滥用，除非你非常确定这种警告对于项目的伤害微乎其微。

## 总结

无规矩不成方圆，愿这篇文章能够帮助到大家。本人会不定时更新，如有异议，请issue我。

## Link
* 一个很好用的添加注释插件[VVDoucument](https://github.com/onevcat/VVDocumenter-Xcode)
* 一个很好用的格式化代码插件[BBUncrustifyPlugin](https://github.com/benoitsan/BBUncrustifyPlugin-Xcode)

