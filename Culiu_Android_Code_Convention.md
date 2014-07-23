# 醋溜科技Android项目编码规范

## 1 概述


#### 1.1 目的

编码规范对于程序员而言尤为重要，有以下几个原因：

* 一个软件的生命周期中，80%的花费在于维护
* 几乎没有任何一个软件，在其整个生命周期中，均由最初的开发人员来维护
* 编码规范可以改善软件的可读性，可以让程序员尽快而彻底地理解新的代码
* 如果你将源码作为产品发布，就需要确任它是否被很好的打包并且清晰无误，一如你已构建的其它任何产品

本文为规范软件开发人员的代码编写提供参考依据和统一标准。

#### 1.2 参考
* [Google Java Coding Style](http://google-styleguide.googlecode.com/svn/trunk/javaguide.html)
* [Code Style Guidelines for Contributors](http://source.android.com/source/code-style.html)
* [Oracle Code Conventions for the Java TM Programming Language](http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html)
* [Google Java编程风格指南中文版](http://www.cnblogs.com/lanxuezaipiao/p/3534447.html)


## 2 源代码文件

#### 2.1 文件名
源文件以其最顶层的类名来命名，大小写敏感，文件扩展名为.java。

#### 2.2 文件编码：UTF-8
源文件编码格式为UTF-8。设置编码参考：[Eclipse中设置文件编码的方法](http://ryxxlong.iteye.com/blog/788469)

#### 2.3 特殊字符
* 空白字符: 除了行结束符序列，ASCII水平空格字符(0x20，即空格)是源文件中唯一允许出现的空白字符。有其它字符串中的空白字符都要进行转义。制表符不用于缩进。
* 特殊转义序列：对于具有特殊转义序列的任何字符(\b, \t, \n, \f, \r, \“, \‘及\)，我们使用它的转义序列，而不是相应的八进制(比如\012)或Unicode(比如\u000a)转义。
* 非ASCII字符: 对于剩余的非ASCII字符，是使用实际的Unicode字符(比如∞)，还是使用等价的Unicode转义符(比如\u221e)，取决于哪个能让代码更易于阅读和理解。


```
提示：在使用Unicode转义符或是一些实际的Unicode字符时，建议做些注释给出解释，这有助于别人阅读和理解。
```
**例子**：

语句                                                    | 说明
------------------------------------------------------ | -------------
String unitAbbrev = "μs";                              | 好，即使没有注释也非常清晰
String unitAbbrev = "\u03bcs"; // "μs"                 | 允许，但没有理由要这样做
String unitAbbrev = "\u03bcs"; // Greek letter mu, "s" | 允许，但这样做显得笨拙还容易出错
String unitAbbrev = "\u03bcs";                         | 不好，读者根本看不出这是什么
return '\ufeff' + content; // byte order mark          | 好，对于非打印字符，使用转义，并在必要时写上注释


## 3 源代码文件结构
<br/>
一个源文件按顺序包含以下部分：

* 许可证或版权信息(如有需要)
* 包声明
* import语句
* 一个顶级类(只有一个)

以上每个部分之间用一个空行隔开。

#### 3.1 许可证或版权信息
如果需要声明lincense或copyright信息，应该在文件开始时声明。


#### 3.2 包声明
注意package语句不管有多长，都不要换行。

#### 3.3 import语句
* 不应该使用通配符import，即不要出现类似这样的import语句：import java.util.*;
* import语句不能换行。
* 使用Eclipse快捷键整理即可。Windows: Ctrl+Shift+O  Mac: Command+Shift+O

#### 3.4 类声明
* **类成员顺序：**类的成员顺序对易学性有很大的影响，但这也不存在唯一的通用法则。不同的类对成员的排序可能是不同的。 最重要的一点，每个类应该以某种逻辑去排序它的成员，维护者应该要能解释这种排序逻辑。比如， 新的方法不能总是习惯性地添加到类的结尾，因为这样就是按时间顺序而非某种逻辑来排序的。
* **重载不分离原则：**当一个类有多个构造函数，或是多个同名方法，这些函数/方法应该按顺序出现在一起，中间不要放进其它函数/方法。


## 4 代码格式

#### 4.1 大括号

##### 4.1.1 必须使用大括号，即使是可选的情况
在  if、 else、 for、 do 以及 while 这些语句中的代码，就算是空的，或是仅有一行，也都要使用大括号。

例子：

代码 | 评价
------------ | -------------
if (yesOrNo) func(); | 不好
if (yesOrNo)<br/>&nbsp; &nbsp; &nbsp; &nbsp; func(); | 不好
if (yesOrNo) {<br/>&nbsp; &nbsp; func();<br/>} | 好

##### 4.1.2 非空代码块使用 K&R Style
对于非空块和块状结构，大括号遵循[Kernighan和Ritchie风格](http://en.wikipedia.org/wiki/Indent_style)。

例子：

```
return new MyClass() {
  @Override
  public void method() {
    if (condition()) {
      try {
        something();
      } catch (ProblemException e) {
        recover();
      }
    }
  }
};
```

##### 4.1.3 空块：可以用简洁版本
一个空的块状结构里什么也不包含，大括号可以简洁地写成{}，不需要换行。例外：如果它是一个多块语句的一部分(if/else 或 try/catch/finally) ，即使大括号内没内容，右大括号也要换行。

```
void doNothing() {}
```
#### 4.2 缩进4个空格
缩进要使用空格，不要使用制表符。如何设置缩进空格个数参考：[Eclipse中设置缩进的方法](http://zwkufo.blog.163.com/blog/static/2588251201343123235394/)。

#### 4.3 换行

##### 4.3.1 类内连续的成员之间使用换行
类内连续的成员之间：字段，构造函数，方法，嵌套类，静态初始化块，实例初始化块。在函数体内，语句的逻辑分组间使用换行。例外：两个连续字段之间的换行是可选的，用于字段的换行主要用来对字段进行逻辑分组。

##### 4.3.2 逻辑上紧密相关的代码块应该用一个空行分开。
```
// Create a new identity matrix
Matrix4x4 matrix = new Matrix4x4(); 

// Precompute angles for efficiency 
double cosAngle = Math.cos(angle); 
double sinAngle = Math.sin(angle); 

// Specify matrix as a rotation transformation
matrix.setElement(1, 1, cosAngle); 
matrix.setElement(1, 2, sinAngle); 
matrix.setElement(2, 1, -sinAngle); 
matrix.setElement(2, 2, cosAngle); 

// Apply rotation
transformation.multiply(matrix);
```

#### 4.4 列限制：80或100
一个项目可以选择一行80个字符或100个字符的列限制，除了下述例外，任何一行如果超过这个字符数限制，必须自动换行。

例外：

1. 不可能满足列限制的行(例如，Javadoc中的一个长URL，或是一个长的JSNI方法参考)。
2. package和import语句
3. 注释中那些可能被剪切并粘贴到shell中的命令行。

如果使用Eclipse可以设置一条标线标志80或100列的位置，生动换行，方法参考：[eclipse 设置超过80字自动换行](http://hi.baidu.com/nicelinda/item/941bd11538fc774be75e06a9)

#### 4.5 空格的使用

1. 运算符两边应该各有一个空格。
2. Java保留字后面应跟随一个空格。
3. 逗号后面应跟随一个空格。
4. 冒号两个应各有一个空格。
5. 分号后面应跟随一个空格。

```
a = (b + c) * d;            // 错误：a=(b+c)*d
while (true) {              // 错误：while(true){
doSomething(a, b, c, d);    // 错误：doSomething(a,b,c,d);
case 100 :                  // 错误：case 100:
for (i = 0; i < 10; i++) {  // 错误：for(i=0;i<10;i++){
```

#### 4.6 推荐用小括号来限定组

除非作者和reviewer都认为去掉小括号也不会使代码被误解，或是去掉小括号能让代码更易于阅读，否则我们不应该去掉小括号。 我们没有理由假设读者能记住整个Java运算符优先级表。

#### 4.7 变量声明

1. 每次只声明一个变量。不要使用组合声明，比如int a, b;
2. 需要时才声明，声明之后要立刻初始化。
3. 数组声明要使用非C语言风格。中括号是类型的一部分：String[] args， 而非String args[]。

#### 4.8 switch语句

1. default的情况要写出来。每个switch语句都包含一个default语句组，即使它什么代码也不包含。
2. Fall-through：在一个switch块内，每个语句组要么通过break, continue, return或抛出异常来终止，要么通过一条注释来说明程序将继续执行到下一个语句组， 任何能表达这个意思的注释都是OK的(典型的是用// fall through)。这个特殊的注释并不需要在最后一个语句组(一般是default)中出现。示例：

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

#### 4.9 注解(Annotations)

注解紧跟在文档块后面，应用于类、方法和构造函数，一个注解独占一行。这些换行不属于自动换行(第4.5节，自动换行)，因此缩进级别不变。例如：

```
@Override
@Nullable
public String getNameIfPresent() { ... }
```

例外：单个的注解可以和签名的第一行出现在同一行。例如：

```
@Override public int hashCode() { ... }
```

应用于字段的注解紧随文档块出现，应用于字段的多个注解允许与字段出现在同一行。例如：

```
@Partial @Mock DataLoader loader;
```

参数和局部变量注解没有特定规则。

#### 4.10 Modifiers

类和成员的modifiers如果存在，则按Java语言规范中推荐的顺序出现。

```
public protected private abstract static final transient volatile synchronized native strictfp
```

## 5 命名约定

#### 5.1 对所有标识符都通用的规则
标识符只能使用ASCII字母和数字，因此每个有效的标识符名称都能匹配正则表达式\w+。

#### 5.2 标识符类型的规则

##### 5.2.1 包名
包名全部小写，连续的单词只是简单地连接起来，不使用下划线。

##### 5.2.2 类名
类名都以[UpperCamelCase](http://zh.wikipedia.org/zh/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB)风格编写。

类名通常是名词或名词短语，接口名称有时可能是形容词或形容词短语。现在还没有特定的规则或行之有效的约定来命名注解类型。

测试类的命名以它要测试的类的名称开始，以Test结束。例如，HashTest或HashIntegrationTest。

##### 5.2.3 方法名

方法名都以[lowerCamelCase](http://zh.wikipedia.org/zh/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB)风格编写。

方法名通常是动词或动词短语。

下划线可能出现在JUnit测试方法名称中用以分隔名称的逻辑组件。一个典型的模式是：test<MethodUnderTest>_<state>，例如testPop_emptyStack。 并不存在唯一正确的方式来命名测试方法。

##### 5.2.4 常量名

常量名命名模式为CONSTANT_CASE，全部字母大写，用下划线分隔单词。那，到底什么算是一个常量？

每个常量都是一个静态final字段，但不是所有静态final字段都是常量。在决定一个字段是否是一个常量时， 考虑它是否真的感觉像是一个常量。例如，如果任何一个该实例的观测状态是可变的，则它几乎肯定不会是一个常量。 只是永远不打算改变对象一般是不够的，它要真的一直不变才能将它示为常量。

```
// Constants
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final Joiner COMMA_JOINER = Joiner.on(',');  // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```
这些名字通常是名词或名词短语。

##### 5.2.5 非常量字段名

非常量字段名以[lowerCamelCase](http://zh.wikipedia.org/zh/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB)风格编写。

这些名字通常是名词或名词短语。

##### 5.2.6 参数名

参数名以[lowerCamelCase](http://zh.wikipedia.org/zh/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB)风格编写。

参数应该避免用单个字符命名。

##### 5.2.7 局部变量名

局部变量名以[lowerCamelCase](http://zh.wikipedia.org/zh/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB)风格编写，比起其它类型的名称，局部变量名可以有更为宽松的缩写。

虽然缩写更宽松，但还是要避免用单字符进行命名，除了临时变量和循环变量。

即使局部变量是final和不可改变的，也不应该把它示为常量，自然也不能用常量的规则去命名它。

##### 5.2.8 类型变量名

类型变量可用以下两种风格之一进行命名：

    单个的大写字母，后面可以跟一个数字(如：E, T, X, T2)。
    以类命名方式(5.2.2节)，后面加个大写的T(如：RequestT, FooBarT)。
