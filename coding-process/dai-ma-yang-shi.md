介绍
====
本文绝大部分遵循了google java style，[原文链接](https://google.github.io/styleguide/javaguide.html)  
[xml for idea](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)  
[xml for eclipse](https://github.com/google/styleguide/blob/gh-pages/eclipse-java-google-style.xml)  
本文介绍了代码样式与风格，主要涉及代码格式工程上的美学标准，并不是一个完整的编码手册。

源码文件
========

文件名
------

源文件名由它所包含的顶级类的区分大小写的名称和 **.java** 扩展名组成。

文件编码
--------

项目中所有文件均以 **UTF-8** 编码

特殊字符
--------

### 空格

使用ASCII的水平空格字符（0x20）作为源码文件中的唯一（除了换行符之外）空格符。即：  
1.  所有字符串和字面量中的空格符均要转义处理。  
2.  缩进时不使用Tab符号。
### 特殊转义序列

字符的特殊转义序列包括：（\\b， \\t， \\n， \\f， \\r， \\"， \\' 及\\\\）。  
在代码中直接使用转义序列，而不使用他们的octal（\\012）或者Unicode（\\u000a）转义。

### 非ASCII字符

对于需要使用非ASCII字符的地方，可以选择Unicode字符（如：∞）或者Unicode转义字符（如：\\u221e）。请结合实际情况选择可读性更高的方式。  
同时强烈不建议在代码字符串及注释之外的地方使用Unicode转义字符。  
使用Unicode等非直观编码格式的时候请加上额外的注释。  
下表是详细说明：
  ---------------------------------------------------------------------- --------------------------------------------------------
| **示例**|**说明**|
|---|---|
|String unitAbbrev = "μs";|最佳方式：此时不需要任何注释。|
|String unitAbbrev = "\\u03bcs"; // "μs"|允许，不推荐。|
|String unitAbbrev = "\\u03bcs"; // Greek letter mu, "s"|允许，不直观，强烈不推荐。|
|String unitAbbrev = "\\u03bcs";|不允许：完全不能看出什么意思。|
|return '\\[]{#OLE_LINK1 .anchor}ufeff' + content; // byte order mark|推荐：使用转义来表达不可打印的字符，并加上恰当的注释。|
  ---------------------------------------------------------------------- --------------------------------------------------------

不要因为担心程序不能正确处理非ASCII字符妥协降低代码的可读性。如果出现这种情况意味着代码本身出了问题，需要从修复代码入手。

源码文件结构
============

源码文件格式按顺序规定如下：
1.  License或者copyright信息。
2.  包声明。
3.  Import语句
4.  仅有的一个顶级类  

上面四部分应该各由一个空行分隔开。

License或copyright
------------------

由 /\*\*/ 注释组成。  
**正例** ：  
```
/*
 * Copyright © 2018-2022 平安国际智慧城市科技股份有限公司。All rights reserved.
 */
```
**反例** ：
```
/**
 * Copyright © 2018-2022 平安国际智慧城市科技股份有限公司。All rights reserved.
 */
```

包声明
------

包声明不允许换行。代码行的长度限制不适用于包声明语句。

Import语句
----------

### 不允许出现通配符

**正例**：
```
import java.util.Date;
import java.util.List;
import java.sql.Connection;
```
**反例**：
```
import java.util.*;
import java.sql.*;
```

### 不允许换行

代码行的长度限制不适用于import语句。

### 顺序

1.  所有的static import在一个块中

2.  所有的非static import在一个块中  

如果代码同时有static和非static导入，需要用一个空行来分隔。在各自块中不需要额外的空行。  
各块中的导入语句以名称的ASCII顺序排列。

### 对于class不使用静态导入

对于各种嵌套类不使用static导入，应该使用普通的import。

类声明
------

### 一个顶级类

一个源码文件中有且仅有一个顶级类。

### 类代码顺序

类field和初始化器的顺序对可读性影响很大。但不强行指定唯一正确的方法，不同的类可以选择不同的排序方式。  
这里重点要求类的排序应当遵循某种逻辑，注意类的维护者必须可以给出合乎逻辑的解释。  
如：新的方法并不能习惯性的添加到类的尾部，因为这样就是按照方法添加的时间排序，而不是某种逻辑顺序。

#### 不要分隔开重载的代码

当一个类有多个构造函数或者相同名称的方法时，它们应当按顺序出现，中间不包含其他代码或者私有成员变量。

代码格式化
==========

大括号
------

### 在可选的地方强制使用大括号

if，else，for，do 以及 while 之后均应当紧跟大括号，即使是仅有一行代码甚至空块。

### 非空代码块：K&R 风格

对于非空代码块或者其他结构体：

1.  左大括号之前不换行

2.  左大括号之后换行

3.  右大括号之前不换行

4.  如果右大括号是一个语句、函数体或类的结束，则右大括号后换行;
    否则不换行。例如，如果右大括号后面是else或逗号，则不换行。

下面是一个具体的例子：
```
    return () -> {
      while (condition()) {
        method();
      }
    };
    
    return new MyClass() {
      @Override
      public void method() {
        if (condition()) {
          try {
            something();
          } catch (ProblemException e) {
            recover();
          }
        } else if (otherCondition()) {
          somethingElse();
        } else {
          lastThing();
        }
      }
    };
  }
```
对于枚举类，请见枚举类章节。

### 空代码块的大括号可以连写

如果一个空的块状结构里什么也不包含，大括号可以简洁地写成{}，不需要换行。  
例外：如果它是一个多块语句的一部分（if/else 或 try/catch/finally），即使大括号内没内容，右大括号也要换行。  
**正例**：
```
void doNothing() {}

void doNothing() {
}
```
反例：
```
try {
  doSomething();
} catch (Exception e) {} // 多块语句，这里需要换行
```
缩进：+2空格
------------

每个新的块开始，必须比上一行缩进两个空格。当块结束时，恢复为块前的缩进级别。  
缩进级别对块中的代码/注释均有效。

一行仅一条语句
--------------

每一条语句之后必须换行。

列的宽度限制：100
-----------------

一行java代码最多包含100个字符。一个  *字符*  指任意Unicode字符码。除了下面的例外情况，任意代码到达此字数限制必须换行。  
无论它的展示宽度是大是小，任意的Unicode字符码均视为一个字符。当使用全角字符时可以提前换行。  
例外：
1.  无法遵守列宽度限制的特殊地方（如javadoc中的URL，或者超长的JSNI方法引用）。
2.  package 和 import 语句。
3.  注释中的命令行代码，因为它们可能被复制粘贴到shell中使用。

换行
----

注意提取方法或者局部变量可能会有效缩短行的长度。

### 何时换行

首要原则是在更高的语法级别处中断。
1.  在非赋值操作符之前换行。  
这点适用于：
    1. “.”分隔符
    2. 双冒号方法引用 “::”
    3. 类型限定中“&”（&lt;T extends Foo & Bar&gt;）
    4. catch块中的“|”（catch （FooException | BarException e））
2.  当赋值运算符需要换行时，**建议**在操作符之后换行。  
这点同样适用于 for（“foreach”）中的赋值语句  
3.  方法或者构造函数和其后的左括号（“(”）在同一行
4.  逗号（“,”）和前面的语句在同一行
5.  在lambda表达式箭头符号前后不换行。除非此lambda表达式仅包含无大括号的一行语句：  

**正例** ：
```
    MyLambda<String, Long, Object> lambda =
        (String label, Long value, Object obj) -> {
          ...
        };

    Predicate<String> predicate = str ->
        longExpressionInvolving(str);
```

换行的原则是使得代码更清晰，而不是尽力缩短代码行数。

### 换行时至少缩进4个空格

当使用换行（一个连续行换为多行）时，后续每行至少缩进4个空格。  
当有多个连续的换行时，缩进可以按需调整为多于4个缩进。  
通常而言，两个连续换行使用同样的缩进场景是当且仅当它们处于同样的语法级别。  
我们不鼓励使用可变数目的空格来对齐前面的行，详见4.6.3水平对齐章节。

空白
----

### 垂直空白（空行）

一个空行出现在如下地方：

1.  类的连续成员或初始化器之间：field，构造器，方法，嵌套类，静态初始化器，实例初始化器。  
上面的情况有两个例外：
    1. 两个连续的fields之间的空行是可选的。这时空行应当用作逻辑上将fields分组。
    2. enum常量间的空行在4.8.1中描述。
1.  源码文件中其它要求换行的地方。  

此外只要能够增加代码可读性，可以在代码任意地方增加空行。    
在第一个初始化器之前、最后一个初始化器之后的做法不做强制要求。

### 水平空白（缩进）

除了上述缩进，以及字面量、注释和Javadoc需要用到，单个ASCII空格仅出现在以下几个地方：
1.  用于分隔保留字，如 if，for或者 catch 和字后的左括号（“(”）
2.  用于分隔保留字，如else或者catch以及之前的右大括号（“}”）
3.  用于左大括号（“{”）之前，这里有两个例外：
    1. 注解中的左花括号左边不用空格：@SomeAnnotation({a, b})
    2. 多维数组初始化时的多个花括号之间不需要空格：String\[\]\[\] array = {{“foo”}}
4.  在二元或者三元运算符两边加空格。对于下列情况也同样适用：
    1. 类型限定中的 “&”，如：&lt;T extends Foo & Bar&gt;
    2. catch中多个异常类型中建的中竖线 “|”：catch (FooException | BarException e)
    3. foreach 循环中的 “:”
    4. lambda表达式中的 “->”
5.  “,”，“::”或者强转结束的“)”后面
6.  行尾注释的“//”两侧。这里允许多个连续空格，不做强制要求
7.  声明的类型和变量之间：List<String> list
8.  数组初始化时两边大括号的内侧的空格是可选的，即：
    new int\[\] {5, 6} 和 new int\[\] { 5, 6 } 均合法
9.  Java8的type annotation和“\[\]”或者“...”之间，如：
```
  @Target({ElementType.TYPE_USE, ElementType.TYPE_PARAMETER})
  public @interface MyAnnotation {
  }

  private char @MyAnnotation [] array1; // char数组，char[]
  private char[] @MyAnnotation [] array2; // char二维数组
  
```
注意，对于行开始和结束处的空格不做额外要求，这里仅适用于行“内部”的空格。

### 不要求水平对齐

不要使用纵向对齐。  
**正例** ：
```
private int x;
private Color color;
```
**反例** ：
```
private  int       x;
private  Color   color;
```
对于历史遗留的对齐代码，在修改时请将它们改为不对齐格式。  
格式对齐将在代码重构时带来不必要的麻烦，合并代码时也会有很多不必要的冲突。

推荐分组括号
------------

可选的分组小括号只有省略它们能让代码更容易阅读时才能省略。  
假设每个读者都记住整个java操作符优先级表是不合理的。  
**正例**：
```
--y || (++x && ++z);
```
**反例**：
```
--y || ++x && ++z;
```
特殊构造器
----------

### 枚举类

在每个枚举常量“,”之后的换行是可选的。也可以允许多个空行：
```
  private enum Answer {
    YES {
      @Override
      public String toString() {
        return "yes";
      }
    },
    
    NO,
    MAYBE
  }
```
一个没有自定义方法、没有文档的枚举类，可以简略的写为一行：
```
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```
### 变量声明

#### 单个变量声明

每个变量（包括类变量及本地变量）应该单独声明。
**正例**：
```
int a;
int b;
```
**反例**：
```
int a, b;
```
**例外**：在for循环中的声明可以不受此限制。

#### 使用前声明

不应该在方法或者代码块开始处声明局部变量。  
局部变量应该在近使用处声明，有效缩小其范围。  
局部变量在声明时应该带初始化器，或者在声明之后马上进行初始化。

### 数组

#### 数组块格式初始化

数组初始化可以使用类似块的结构。下列四种数组初始化声明均有效：
```
    int[] a =
        new int[]{
            0, 1, 2, 3
        };
    int[] b =
        new int[]
            {
                0,
                1,
                2,
                3,
            };
    int[] c =
        new int[]
            {
                0, 1,
                2, 3
            };
    int[] d =
        new int[]
            {
                0, 1, 2, 3
            };
```

#### 不要使用类似C的数组初始化声明

方括号是类型的一部分，而不是变量的一部分。  
**正例** ：
```
String[] args
```
反例：
```
String args[]
```
### switch表达式

#### 缩进

switch块缩进+2空格。  
switch后的左大括号“{”之后应该换行，后文缩进级别为+2空格。switch块结束之后缩进级别恢复。

#### Fall-through注释

在一个switch块内，每个语句组要么通过break，continue，return或抛出异常来终止，要么通过一条注释来说明程序将继续执行到下一个分支块，任何能表达这个意思的注释都是OK的(典型的是用//
fall through)。最后一个分支不需要此注释。示例如下：
```
    switch (input) {
      case 1:
      case 2:
        prepareOneOrTwo();
        // fall through
      case 3:
        handleOneTwoOrThree();
        break;
      default:
        handleLargeNumber(input);
    }
```
**注意**：在case 1中没有注释，注释只需要出现在（case内部存在的）代码语句之后。

#### 强制default分支

即使default为空，也需要添加空的default分支。  
**例外**：对于枚举类来说，如果case已经包含了所有可能的枚举值，此时可以省略default分支。现代的IDE或者静态代码检查工具可以发现缺失的选项。

### 注解

用于类、方法或者构造器的注解可以紧跟着documentation块之后，一行只写一个注解。多个注解换行不需要缩进。
下面是一个正确的例子：
```
  @Override
  @Nullable
  public String getNameIfPresent() { ...}
```
例外：如果仅有一个无参注解，可以和它签名的代码在同一行：
```
@Override public int hashCode() { ... }
```
类属性的注解应当紧跟document块，但是这里的多个注解（包括有参的）可以在同一行：
```
@Partial @Mock DataLoader loader;
```
参数、局部变量或类型的注释没有特定规则。

### 注释

这里的注释仅指代码注释，而不包括Javadoc。  
注释之后的换行符之前允许有若干空格。

#### 块注释样式

块注释和上下文代码使用同样等级的缩进。不强制使用“/\* ...
\*/”还是“//”风格。  
/\*\*/注释跨行时，后续行必须从\*开始，并且与前一行的\*对齐。  
下面是三种注释风格都是正确的：
1：
```
  /*
   * This is
   * okay.
   */
```
2：
```
  // And so 
  // is this. 
```
3：
```
  /* Or you can
   * even do this. */
```

注意：当注释跨行时，现代的IDE可以自动格式化“/\*... \*/”类型的注释，但是 “//”格式一般不能自动格式化。

### 限定符

类和成员的限定符的书写顺序应当按照Java 语言规范排序：  

public protected private abstract default static final transient volatile synchronized native strictfp

命名
====

标识符通用准则
--------------

修饰符应当仅由ASCII字符和数字组成，下面说明了少数可以用到下划线的场景。  
这意味着所有合法的修饰符都能够被 \\w+ 正则匹配到。

不推荐特殊的前缀或者后缀。如：
name\_，mName，s\_name以及kName

标识符分类准则
--------------

### 包名

包名由小写字母组成，如果有多个单词可以简短地写在一起。  
**正例**：
```
com.example.deepspace
```
反例：
```
com.example.deepSpace
com.example.deep_space
```
### 类名
本节参考了阿里的类命名规范。
1.  类名使用 UpperCamelCase 风格，遵从驼峰形式，接
    .java扩展名。但以下情形例外：DO / BO /DTO / VO / AO

    **正例**：MarcoPolo / UserDO / XmlService / TcpUdpDeal / TaPromotion
    **反例**：macroPolo / UserDo / XMLService / TCPUDPDeal / TAPromotion

1.  类名严禁使用拼音与英文混合的方式，不允许直接使用中文的方式。
    说明：正确的英文拼写和语法可以让阅读者易于理解，避免歧义。注意，即使纯拼音命名方式也要避免采用。

    **正例**：alibaba / taobao / youku / hangzhou 等国际通用的名称，可视同英文

类名通常情况下是名词或者名词短语。如，“Character”或“ImmutableList”。  
接口名可以为形容词或副词短语，如“Readable”。

对于注解名无规定。  
测试类的命名由被测试的类名加上“Test”组成。如：“HashMapTest”或者“HashIntegrationTest”。

### 方法名

方法名格式为首字母小写的驼峰。  
方法名通常情况下是动词或者动词短语。如：“sendMessage”或者“stop”。  
JUnit测试类中允许用下划线来逻辑区分测试组件，但名称的各部分也需要遵循驼峰。  
常见的格式为：&lt;methodUnderTest&gt;\_&lt;state&gt;，具体举例如：pop\_emptyStack。  
**注意**：对测试方法名的要求比非测试方法要求低，允许一定自由度。

### 常量名

常量名全大写，单词间用下划线隔开：CONSTANCE\_CASE。

我们这里说的常量，是指 **static final** 关键字修饰的属性。  
定义的常量应该是深度不可变的，并且它们的方法应该没有side effect。  
具体来说可以包括：基本数据类型、字符串、不可变类，只包括不可变类型的可变集合。  
如果一个实例含有可以被观测到的变化，那么它就不是常量。

**正例**：
```
    static final int NUMBER = 5;
    static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
    static final ImmutableMap<String, String> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
    static final Joiner COMMA_JOINER = Joiner.on(',');  // Joiner 是不可变类
    static final SomeMutableType[] EMPTY_ARRAY = {};
    enum SomeEnum { ENUM_CONSTANT }
```
**反例**：
```
    static String nonFinal = "non-final";
    final String nonStatic = "non-static";
    static final Set<String> mutableCollection = new HashSet<String>();
    static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
    static final ImmutableMap<String, SomeMutableType> mutableValues =
        ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
    static final Logger logger = Logger.getLogger(MyClass.getName());
    static final String[] nonEmptyArray = {"these", "can", "change"};
```
这些名字通常由名词或者名词短语组成。

### 非常量的field

static或者其他修饰符的非常量的域应该是小写字母开头的驼峰。  
它们也通常是名词或者名词短语。如：computeValues 或者 index。

### 参数名

参数名是小写字母开头的驼峰。  
不要在public方法中使用单字符的参数名。

### 本地变量名

本地变量遵循小写字母开头的驼峰。  
即使是final和constant，局部变量也不是常量，它们不应该被设计成常量。

### 类型变量

类型变量有两种命名样式：
-   一个大写字母，其后可以跟一个数字，如：E，T，X，T2。
-   遵循类名命名样式，后接大写T，如：RequestT，FooBarT。

关于驼峰的详细说明
------------------

将英语转为驼峰经常有多种方式，英语中也有如IPv6和iOS这些容易引起混淆的单词。现在将驼峰做详细规定：

1.  将短语转换为纯ASCII并删除任何撇号。例如，将“Müller's algorithm”转为“Muellers algorithm”。
2.  将上面的结果划分成单词、空格和其他标点符号。
    -   如果一个单词已经是驼峰形式的，就直接按驼峰划分。注意iOS这类单词本身并不是驼峰式的，因此不适用本条。
3.  将所有字母全变为小写：
    -   大驼峰，每个单词的首字母大写。用于类命名。
    -   小驼峰，除了第一个单词，每个单词的首字母大写，用于方法命名。
4.  组合它们。  

下表是一些例子：

  |**原始含义**|**正例**|**反例**|
  |------|------|------|
  |"XML HTTP request" |XmlHttpRequest|XMLHTTPRequest|
  |"new customer ID"|newCustomerId   |    newCustomerID|
  |"inner stopwatch"    |     innerStopwatch  |    innerStopWatch|
  |"supports IPv6 on iOS?"  | supportsIpv6OnIos |  supportsIPv6OnIOS|
  |"YouTube importer"   |     YouTubeImporter|
  
**注意**：因为连字符号的原因，英语中有些单词可能有多种表示方法。  
如：nonempty或者non-empty都正确，因此方法名checkNonempty和checkNonEmpty也都是正确的。

编程实践
========

始终使用 @Override
------------------

在覆盖超类、接口的方法时应尽可能地使用@Override注解。  
唯一的例外是父类的方法带有@Deprecated时。

不忽略异常
----------

通常对异常的处理包括记录日志，或者如果判断此异常不可能出现，可以抛出AssertionError。  
如果此异常确实不需要任何处理，必须在catch中用注释写明其合理原因：
```
    try {
      int i = Integer.parseInt(response);
      return handleNumericResponse(i);
    } catch (NumberFormatException ok) {
      // response不是数字，这是合理的，无需特殊处理。
    }
    return handleTextResponse(response);
```
在单元测试代码中允许例外情况，即如果捕获的异常的名称是expected或以expected开头，则可以忽略它而不加注释。下面是一个非常常见的习惯用法，用于确保测试中的代码确实抛出了预期类型的异常，因此这里不需要注释：

```$xslt
    try {
      emptyStack.pop();
      fail();
    } catch (NoSuchElementException expected) {
    }
```

使用类直接调用静态成员
----------------------

当引用静态的类成员时，不要使用类的实例来调用。
**正例**：
```
Foo.aStaticMethod(); // good
```
**反例**：
```
aFoo.aStaticMethod(); // bad
somethingThatYieldsAFoo().aStaticMethod(); // very bad
```
不要使用Finalizers
------------------

请牢记不要覆盖Object.finalize方法。

Javadoc
=======

格式化
------

### 常规样式

基本样式如下例：

```
  /**
   * Multiple lines of Javadoc text are written here,
   * wrapped normally...
   */
  public int method(String p1) { ... }
```

简单的方法可以单行Javadoc：
```
/** An especially short bit of Javadoc. */
```
上边的基本样式总是没问题的。单行仅适用于一行能写清楚的时候，而且没有类似@return的标记时。

### 段落

只包含最左侧“\*”的行，代表空行。空行出现在段落之间和Javadoc标记（@XXX）之前。  
除了第一个段落，每个段落第一个单词前都有标签&lt;p&gt;，并且它和第一个单词间没有空格。

### 标签

标签应当按照如下顺序出现：@param、@return、@throws、@deprecated。  
如果使用了这四个标签，则后面不允许空描述。  
当单个标签的内容不止一行时，后续行应对照“@”位置缩进4个空格。

摘要片段
--------

每个Javadoc都从一个简短的摘要开始。因为在展示类或者方法的索引时，它们经常是唯一出现的文字。  
一个摘要片段应该是：一个名字短语或者动词短语，不需要是一个完整的句子；  
它不应当以*A {@code Foo} is a ... *或者*This method returns*开头；  
也不应该是一个完整的祈使句如Save the record。

何时使用Javadoc
---------------

至少在每个public类及它的每个public和protected成员处使用Javadoc，以下是一些例外情况：

### 例外1：自解释的方法

对于像getFoo这样的简单方法可以省略掉Javadoc。它的含义就像方法名那样简单，仅返回一个Foo没有任何其他操作。

但是如果有一些相关信息是需要读者了解的，那么以上的例外不应作为忽视这些信息的理由。  
例如，对于方法名*getCanonicalName*，就不应该忽视文档说明，因为读者很可能不知道canonical name指的是什么。

### overrides方法

子类覆盖父类的方法可以不用Javadoc。

### 可选方法

对于包外不可见的类和方法，如有需要，也是要使用Javadoc的。  
如果一个注释是用来定义一个类、方法、字段的整体目的或行为，那么这个注释应该写成Javadoc（“/\*\* 格式”）。  
对于可选的Javadoc来说并不强制要求遵循上述样式要求。