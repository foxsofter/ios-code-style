# IOS编码规范
>统一规范XCode编辑环境，以及编写Objective-C代码的风格  
>关于命名的一般性的原则
 * 最少字符，就是要尽量的减少命名对象的长度，尽量选择字符少的名词
 * 名符其实，命名应该能直观的描述被命名对象是什么或者做什么
 * 避免歧义，尽量不要采用多义词，也不要使用命名组合之后产生多义的方式
 * 上下文一致，比如谓词的统一性，如果都是集合类，那么使用Remove表示删除操作，那么所有上下文就应该都用这个Remove，而不要再用Delete
 * 少用缩写，除非是很常见的缩写或者项目中定义好的缩写，否则不要使用缩写
 
1. Xcode配置
  * 安装插件管理插件[alcatraz](http://alcatraz.io/)，方式在链接中找；
  * 安装库管理插件[CocoaPods](http://cocoapods.org/)，可以通过alcatraz安装也可以通过命令行安装；
  * 通过alcatraz安装VVDocument插件，这个插件是用来插入函数注释的，输入///对一个函数进行注释；
  * 由于空格缩进相比Tab缩进，所敲键盘次数过多，也没有Tab缩进直观，所以这里使用Tab缩进而非空格缩进，并且缩进宽度为2个字符。Xcode通过Preferences->Text Editing->Indentation，Prefer indent using选择Tabs，Tab width输入2，Indent width输入2来进行设置；   
2. 工程名及代码文件名 
  * 所有命名都只能采用英文字符，下划线，加号；
  * 所有命名采用Pascal命名方式；
  * 头文件的命名方式很重要，我们可以根据其命名知晓头文件的内容；
  * 扩展类的文件命名，原类文件名+类别，类似GTMNSString+Parsing.h  
  * 文件扩展名 
<table>
    <tr>
        <td>.h</td>
        <td>C/C++/Objective-C header file</td>
    </tr>
    <tr>
        <td>.m</td>
        <td>Objective-C implementation file </td>
    </tr>
    <tr>
        <td>.mm</td>
        <td>Objective-C++ implementation file </td>
    </tr>
    <tr>
        <td>.cc</td>
        <td>Pure C++ implementation file</td>
    </tr>
    <tr>
        <td>.c</td>
        <td>C implementation file</td>
    </tr>
</table>
  *  声明孤立的类或协议：将孤立的类或协议声明放置在单独的头文件中，该头文件名称与类或协议同名。
  *  声明相关联的类或协议：将相关联的声明（类，类别及协议) 放置在一个头文件中，该头文件名称与主要的类/类别/协议的名字相同。
<table>
    <tr>
        <td>NsString.h</td>
        <td>NSString 和 NSMutableString 类</td>
    </tr>
    <tr>
        <td>NsLock.h</td>
        <td>NSLocking 协议和 NSLock, NSConditionLock, NSRecursiveLock 类</td>
    </tr>
</table>
  * 为已有框架中的某个类扩展 API：如果要在一个框架中声明属于另一个框架某个类的范畴类的方法，该头文件的命名形式为：原类名+“Additions”。如 Application Kit 中的 NSBundleAdditions.h。
  * 相关联的函数与数据类型：将相联的函数，常量，结构体以及其他数据类型放置到一个头文件中，并以合适的名字命名。如 Application Kit 中的 NSGraphics.h。
  * 工程中的group与本地文件夹要一一对应
3. 类及协议
  * 采用Pascal命名方式
  * 类名应包含一个明确描述该类（或类的对象）是什么或做什么的名词。类名要有合适的前缀（请参考“前缀”一节）。Foundation 及 Application Kit 有很多这样例子，如：NSString, NSData, NSScanner, NSApplication, NSButton 以及 NSEvent;
  * 协议应该根据对方法的行为分组方式来命名，大多数协议仅组合一组相关的方法，而不关联任何类，这种协议的命名应该使用动名词(ing)，以不与类名混淆；
<table>
    <tr>
        <td>NsLocking</td>
        <td>好的做法</td>
    </tr>
    <tr>
        <td>NsLock</td>
        <td>糟糕的做法，看起来像类名</td>
    </tr>    
</table>
  *  有些协议组合一些彼此无关的方法（这样做是避免创建多个独立的小协议）。这样的协议倾向于与某个类关联在一起，该类是协议的主要体现者。在这种情形，我们约定协议的名称与该类同名。NSObject 协议就是这样一个例子。这个协议组合一组彼此无关的方法，有用于查询对象在其类层次中位置的方法，有使之能调用特殊方法的方法以及用于增减引用计数的方法。由于 NSObject 是这些方法的主要体现者，所以我们用类的名称命名这个协议； 
4. 方法
  * 采用Camel命名方式
  * 避免歧义，如displayName，是返回一个displayName还是执行这个操作，会产生歧义，如果是执行这个操作，应该采用showName，避免采用可以是形容词的display
  * 表示对象行为的方法，名称以动词开头，名称中不要出现 do 或 does，因为这些助动词没什么实际意义。也不要在动词前使用副词或形容词修饰

      ```Objective-C
      - (void) invokeWithTarget:(id)target:
      - (void) selectTabViewItem:(NSTableViewItem *)tableViewItem
      ```

  * get属性方法不要带get前缀，也不要带其它前缀

      ```Objective-C
      - (NSSize) cellSize;  对
      - (NSSize) calcCellSize;  错
      - (NSSize) getCellSize;  错
      ```

  * 参数要用描述该参数的标签命名

      ```Objective-C
      - (void) sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;  对
      - (void) sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;  错
      ```  

  * 第一个参数，方法名要能描述该参数
 
      ```Objective-C
      - (id) viewWithTag:(int)aTag;  对
      - (id) taggedView:(int)aTag;  错
      ```  

  * 不要使用 and 来连接用属性作参数标签，虽然上面的例子中使用 add 看起来也不错，但当你方法有太多参数关键字时就有问题。

      ```Objective-C
      - (int) runModalForDirectory:(NSString *)path file:(NSString *)name types:(NSArray *)fileTypes;  对
      - (int) runModalForDirectory:(NSString *)path addFile:(NSString *)name addTypes:(NSArray *)fileTypes;  错
      ``` 
 
  * 如果方法描述两种独立的行为，使用 and 来串接它们

      ```Objective-C
      - (BOOL) openFile:(NSString *)fullPath withApplication:(NSString *)appName andDeactivate:(BOOL)flag; NSWorkspace 
      ```  

  * 方法的参数个数尽量保持四个以内，如果需要超过四个的参数，应该考虑重构方法或者抽象参数为类型
  * 方法的实现需要进行参数有效性检查；
  * 在方法声明中，在(-/+)符号与返回值之间留1个空格。此外，参数与前面的冒号不留空，方法段与之间留1个空格
  * 方法间保持一个空行
  * 方法内的空行用于区分不同的功能代码，但是如果方法中又太多的功能区块那么需要考虑重构代码
  * 不要使用冒号对齐含实现块的方法，因为Xcode的缩进会使代码更难读
      推荐：

      ```Objective-C
      // blocks are easily readable
      [UIView animateWithDuration:1.0 animations:^{
        // something
      } completion:^(BOOL finished) {
       // something
      }];
      ```

      不推荐：

      ```Objective-C
      // colon-aligning makes the block indentation hard to read
      [UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
      ```

5. 访问方法  
  * 如果属性是用名词描述的，则命名格式为

      ```Objective-C
      - (void) setNoun:(type)aNoun;
      - (type) noun;
      ```

    例如：

      ```Objective-C
      - (void) setgColor:(NSColor *)aColor;
      - (NSColor *) color;
      ```

  * 如果属性是用形容词描述的，则命名格式为：

      ```Objective-C
      - (void) setAdjective:(BOOL)flag;
      - (BOOL) isAdjective;
      ```

    例如：

      ```Objective-C
      - (void) setEditable:(BOOL)flag;
      - (BOOL) isEditable;
      ```

  * 如果属性是用动词描述的，则命名格式为：（动词要用现在时时态）

      ```Objective-C
      - (void) setVerbObject:(BOOL)flag;
      - (BOOL) verbObject;
      ```

    例如：

      ```Objective-C
      - (void) setShowAlpha:(BOOL)flag;
      - (BOOL) showsAlpha;
      ```

  * 不要使用动词的过去分词形式作形容词使用
     
      ```Objective-C
      - (void) setAcceptsGlyphInfo:(BOOL)flag;  对
      - (BOOL) acceptsGlyphInfo;  对
      - (void) setGlyphInfoAccepted:(BOOL)flag;  错
      - (BOOL) glyphInfoAccepted;  错
      ```

  * 可以使用情态动词（can, should, will 等）来提高清晰性，但不要使用 do 或 does
      
      ```Objective-C
      - (void) setCanHide:(BOOL)flag;  对
      - (BOOL) canHide;  对
      - (void) setShouldCloseDocument:(BOOL)flag;  对
      - (void) shouldCloseDocument;  对
      - (void) setDoseAcceptGlyphInfo:(BOOL)flag;  错
      - (BOOL) doseAcceptGlyphInfo;  错
      ```

  * 方法调用时，如果参数太多，则遵照以下方式

      ```Objective-C
      [myObject doFooWith:arg1  
                     name:arg2  
                    error:arg3];  
      ```

      禁用以下方式

      ```Objective-C
      [myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
                    error:arg3];

      [myObject doFooWith:arg1
                     name:arg2 error:arg3];

      [myObject doFooWith:arg1
                name:arg2  // aligning keywords instead of colons
                error:arg3];
      ```

      如果无法使用冒号对齐，则如下缩进两个空格

      ```Objective-C
      [myObj short:arg1
        longKeyword:arg2
        evenLongerKeyword:arg3];
      ```

6. 委托方法
委托方法是那些在特定事件发生时可被对象调用，并声明在对象的委托类中的方法。它们有独特的命名约定，这些命名约定同样也适用于对象的数据源方法。
  * 名称以标示发送消息的对象的类名开头，省略类名的前缀并小写类第一个字符

      ```Objective-C
      - (BOOL) tableView:(NSTableView *)tableView shouldSelectRow:(int)row;
      - (BOOL)application:(NSApplication *)sender openFile:(NSString *)filename;
      ```

  * 冒号紧跟在类名之后（随后的那个参数表示委派的对象）。该规则不适用于只有一个 sender 参数的方法

      ```Objective-C
      - (BOOL) applicationOpenUntitledFile:(NSApplication *)sender;
      ```

  * 上面的那条规则也不适用于响应通知的方法。在这种情况下，方法的唯一参数表示通知对象

      ```Objective-C
      - (void) windowDidChangeScreen:(NSNotification *)notification;
      ```

  * 用于通知委托对象操作即将发生或已经发生的方法名中要使用 did 或 will

      ```Objective-C
      - (void) browserDidScroll:(NSBrowser *)sender;
      - (NSUndoManager *) windowWillReturnUndoManager:(NSWindow *)window;
      ```

  * 用于询问委托对象可否执行某操作的方法名中可使用 did 或 will，但最好使用 should 

      ```Objective-C
      - (BOOL) windowShouldClose:(id)sender;
      ```

7. 方法参数
  * 不要在参数名中使用 pointer 或 ptr，让参数的类型来说明它是指针
  * 避免使用 one， two，...，作为参数名,更不要用1，2，...
  * 避免为节省几个字符而缩写
  * 采用以下参数标签与参数组合的惯例
    
       ```Objective-C
      ...action:(SEL)aSelector
      ...alignment:(int)mode
      ...atIndex:(int)index
      ...content:(NSRect)aRect
      ...doubleValue:(double)aDouble
      ...floatValue:(float)aFloat
      ...font:(NSFont *)fontObj
      ...frame:(NSRect)frameRect
      ...intValue:(int)anInt
      ...keyEquivalent:(NSString *0charCode
      ...length:(int)numBytes
      ...point:(NSPoint)aPoint
      ...stringValue:(NSString *)aString
      ...tag:(int)anInt
      ...target:(id)anObject
      ...title:(NSString *)aString
      ```
8. 常量及枚举
  * 避免使用k-style命名常量
  * 通常不实用宏来定义常量，整数常量用枚举，浮点常量使用const
  * 使用大写字母来定义预处理编译宏，类似的 #ifdef DEBUG
  * 编译器定义的宏名收尾都有双下划线，如`__MACH__`
  * 优先使用全局常量而非宏，应使用static方式声明常量
  * 定义一般枚举使用NS_ENUM，命名方式为：前缀+Pascal命名，具体为什么可参考[nshipster](http://nshipster.com/ns_enum-ns_options)

      ```Objective-C
      typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
          UITableViewCellStyleDefault,
          UITableViewCellStyleValue1,
          UITableViewCellStyleValue2,
          UITableViewCellStyleSubtitle
      };
      ```

  * 定义位与枚举使用NS_OPTION，命名方式为：前缀+Pascal命名+Mask

      ```Objective-C
      typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
          UIViewAutoresizingNone                 = 0,
          UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
          UIViewAutoresizingFlexibleWidth        = 1 << 1,
          UIViewAutoresizingFlexibleRightMargin  = 1 << 2,
          UIViewAutoresizingFlexibleTopMargin    = 1 << 3,
          UIViewAutoresizingFlexibleHeight       = 1 << 4,
          UIViewAutoresizingFlexibleBottomMargin = 1 << 5
      };
      ```

  * const常量命名方式：前缀+Pascal命名

9. 属性和变量
  * 数据成员保持最小公开原则
  * 在需要公开一个数据成员时将其声明为属性，反之声明为实例变量
  * 在不需要改变一个属性时，添加@readonly，需要时添加@readwrite
  * 属性采用Camel命名方式
  * 实例变量的命名方式同属性，不过首尾都加下划线，加尾部下划线可区分属性对应的实例变量
  * 局部变量的命名方式同属性，禁用下划线
  * 定义NSString时，如果不是明确的想要引用，则应该添加@copy,大多数情况下我们是把NSString当成值类型使用的
  * @public,@protected,@private缩进一个空格
  * 使用如下方式

      ```Objective-C
      view.backgroundColor = [UIColor orangeColor];
      [UIApplication sharedApplication].delegate;
      ``` 

      而不是

      ```Objective-C
      [view setBackgroundColor:[UIColor orangeColor]];
      UIApplication.sharedApplication.delegate;
      ```

  * 最好使用auto-synthesis。但是，如果有必要，@synthesize和@dynamic应各自在实现的新行定义
  * 当使用属性，实例变量时应该使用self.来访问，这意味着所有的属性将很容易区分，因为它们都使用 self. 开头
  * 声明指针时星号应该靠近变量名

10. import和include
  * 用#import 导入Objective-C或Objective-C++头文件，用#include 导入C或C++头文件；
  * 导入框架根的头文件而不是分别导入框架头文件，

      ```Objective-C
      #import <Foundation/Foundation.h>     // good
      #import <Foundation/NSArray.h>        // avoid
      #import <Foundation/NSString.h>
      ...
      ```

11. 前缀
  * 前缀有规定的格式。它由两到三个大写字符组成，不能使用下划线与子前缀    
<table><tr><td>前缀</td><td>Cocoa 框架</td></tr>
<tr><td>NS</td><td>Foundation</td></tr>
<tr><td>NS</td><td>Application Kit</td></tr>
<tr><td>AB</td><td>Address Book</td></tr>
<tr><td>IB</td><td>Interface Builder</td></tr></table>
  * 命名 class，protocol，structure，函数，常量时使用前缀；命名成员方法时不使用前缀，因为方法已经在它所在类的命名空间中；同理，命名结构体字段时也不使用前缀
12. Blocks VS Selectors VS Protocols
  * Protocol做为只作为接口使用，类似C#和Java中的interface，剥离老版本Objective-C中使用Protocol作为委托实现方式的功能。
  * 不建议在protocol中使用@optional，因为如果只将protocol作为接口使用，那么抽象好了，接口自然不需要@optional
  * block使得代码能更好的整合，尽可能的去使用block
  * 有了block，除了编译期无法确定需要使用NSSelectorFromString之外，可能真的不需要Selector了。
  * protocol对象声明使用weak，禁止使用retain。因为retain会导致循环索引导致内存泄露
  * 回调的最佳方式是使用代理模式，而不是使用Notification；
  * block语法

      ```Objective-C
      returnType (^blockName)(parameterTypes) = ^returnType(parameters) {...};
      @property (nonatomic, copy) returnType (^blockName)(parameterTypes);
      - (void)someMethodThatTakesABlock:(returnType (^)(parameterTypes))blockName;
      [someObject someMethodThatTakesABlock:^returnType (parameters) {...}];
      typedef returnType (^TypeName)(parameterTypes);
      TypeName blockName = ^returnType(parameters) {...};
      ```

13. NSException 
  * 同样的如C++中一样，强烈不建议使用异常
  * 如果非要使用，代码格式如下

      ```Objective-C
      @try {
        foo();
      }
      @catch (NSException *ex) {
        bar(ex);
      }
      @finally {
        baz();
      }
      ```
  * 异常由具有如下形式的全局 NSString 对象标识：
    [Prefix] + UniquePartOfName + Exception
    UniquePartOfName 部分是有连续的首字符大写的单词组成。例如：
    NSColorListIOException
    NSColorListNotEditableException
    NSDraggingException
    NSFontUnavailableException
    NSIllegalSelectorException
14. 初始化函数
  * 返回值必须为instancetype，不要再使用id了
  * 避免多次调用父类的初始化函数
  * 使用通用的、约定俗成的alloc和init的方式创建实例，而不是使用new方法
  * 在创建NSString，NSDictionary，NSArray和NSNumber等对象实例时，应使用Literals字面量。需要注意的是，不应将nil传给NSArray和NSDictionary字面量，否则会引起程序崩溃，使用

      ```Objective-C
      NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve"];
      NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal"};
      NSNumber *shouldUseLiterals = @YES;
      NSNumber *buildingZIPCode = @10018;
      ```

      而不是

      ```Objective-C
      NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", nil];
      NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys:@"Kate", @"iPhone", @"Kamal",     @"iPad", nil];
      NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
      NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
      ```
  * 在初始化函数中，访问数据成员应该直接访问实例变量而不是属性

15. `#pragma`
  * 使用#pragma将同一个protocol的方法归类
  * 使用#pragma将同一个父类的重写方法归类
  * 使用#pragma将一组相关联的方法归类
      ```Objective-C
      @implementation ViewController
      - (instancetype)init {
        ...
      }

      #pragma mark - UIViewController

      - (void)viewDidLoad {
        ...
      }

      #pragma mark - IBAction

      - (IBAction)cancel:(id)sender {
        ...
      }

      #pragma mark - UITableViewDataSource

      - (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
        ...
      }

      #pragma mark - UITableViewDelegate

      - (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath {
        ...
      }
      ```

16. nil / Nil / NULL / NSNull
    * Objective-C中默认所有的指针指向nil
    * nil最显著的行为是，它虽然为零，仍然可以有消息发送给它，结果返回0

      ```Objective-C
      // 举个例子，这个表达...
      if (name != nil && [name isEqualToString:@"Steve"]) { ... }
      // …可以被简化为：
      if ([name isEqualToString:@"steve"]) { ... }
      ```

  * 不需要显示初始化为nil，更不要初始化为其它几个关键词
 
  * 需要知道如下

      | Symbol | Value         | Meaning                                    |
      |--------|---------------|--------------------------------------------|
      | NULL   | (void *)0     | literal null value for C pointers          |
      | nil    | (id)0         | literal null value for Objective-C objects |
      | Nil    | (Class)0      | literal null value for Objective-C classes |
      | NSNull | [NSNull null] | singleton object used to represent null    |

17. 注释
  * 不推荐处处都是注释，不推荐写大块注释，好的代码应该自解释。
  * 关键的逻辑、跳转、判断，以及复杂的算法，非常有必要使用注释。
  * 多个注释应尽量左对齐。
  * 所有注释需要保持更新，不需要的注释需要删除
  * 安装VVDocument，使用其格式对方法进行注释
  * 在switch块中，如果一个case不添加break，请明确添加注释

18. 表达式格式
  * 方法大括号和其它大括号（比如for、if、while、switch等等）应在语句的同一行开始，而在新的一行关闭；
 
      ```Objective-C
      if (user.isHappy) {
          // Do something
      } else {
          // Do something else
      }
      ```

  * 不要省略掉花括号，因为即使当前只有一行代码，可能后续其它人会添加代码，如果遗忘补上花括号会产生非预期结果

      ```Objective-C
      // good
      if (!error) {
          return success;
      }
      // bad
      if (!error)
          return success;
      // bad
      if (!error) return success;
      ```

  * 单目运算符与操作数间不要有空格
  * 多目运算符与操作数保持一个空格
  * 三目运算符不要嵌套
  * 强制类型转换、参数之间不要有空格
  
19. Booleans
  * Objective-C用BOOL来编码真值。它是signed char的typedef，并且用宏YES和NO来相应的表示真和假。
  * 在Objective-C中，当遇到处理真值的参数，属性和实例变量时，使用类型BOOL。当分配字面值时，使用宏YES和NO。
  * 如果BOOL属性的名称表示为一个形容词，该属性可以省略“is”字头，但需要为get方法按照常规指定名称

      ```Objective-C
      @property (assign, getter=isEditable) BOOL editable;
      ```

  * 判断真假时不要与字面值比较

      ```Objective-C
      // good
      if (someObject) {
      }
      if (![anotherObject boolValue]) {
      }
      // bad
      if (someObject == nil) {}
      if ([anotherObject boolValue] == NO) {}
      if (isAwesome == YES) {} // Never do this.
      if (isAwesome == true) {} // Never do this.
      ```

  * Objective-C中的所有真值类型和数值：
  
      | Name         | Typedef       | Header           | True Value     | False Value     |
      |--------------|---------------|------------------|----------------|-----------------|
      | BOOL         | signed char   | objc.h           | YES            | NO              |
      | bool         | _Bool (int)   | stdbool.h        | true           | false           |
      | boolean      | unsigned char | MacTypes.h       | TRUE           | FALSE           |
      | NSNumber     | __NSCFBoolean | Foundation.h     | @(YES)         | @(NO)           |
      | CFBooleanRef | struct        | CoreFoundation.h | kCFBooleanTrue | kCFBooleanFalse |
