# 基本概念

设计程序时使用的语言称为程序设计语言。

程序员必须使用与各程序设计语言相匹配的软件来执行由该语言写成的程序，这种软件通常称为**语言处理器**。

## 机器语言与汇编语言

操作系统将机器语言书写的程序从磁盘文件中载入内存，之后可以使用硬件直接解释执行。

- 机器语言：一串位数很长的二进制数字。
- 汇编语言：一些关键字表示一串二进制数字，然后简化编程的语言。

汇编程序（assembler），将汇编语言转化为机器语言，一种最基本的语言处理器。

## 解释器与编译器

语言处理器：解释器与编译器。

- 解释器：解释器根据程序中的算法执行运算。它是一种用于执行程序的软件，如果执行的程序由虚拟机器语言或类似于机器语言的程序设计语言写成，这种软件也称为虚拟机。
- 编译器：编译器能够将某种语言写成的程序转换为另一种语言的程序，通常它会将源程序转换为机器语言。编译器转换程序的行为称为编译，转换前的程序称为源代码或源程序。如果编译器没有把源代码直接转换为机器语言，一般称为源代码转换器或源码转换器（source code translator）。

传统的狭义编译器将会以文件形式保存转换后的程序，因此，只要源程序没有变更，编译仅需执行一次，执行时间也会缩短。

## 语言处理器的结构

无论是解释器还是编译器，语言处理器前半部分的程序结构都大同小异。

1. 源代码首先将进行词法分析，由一长串字符串细分为多个更小的字符串单元。分割后的字符串称为单词。
2. 处理器执行语法分析处理，把单词的排列转换为抽象语法树。
3. 编译器会把抽象语法树转换为其他语言，而解释器将会一边分析抽象语法树一边执行运算。

<img src="images/language processor.png" alt="language processor" style="zoom:50%;" />



# 设计程序设计语言

如果想要从零开始设计一种新颖实用的语言，结果往往是半途而废。即使设计成功，也可能由于过于复杂难以实现等原因而最终不了了之。

因此，可以先设计一种极为简单的语言，并开发相应的语言处理器，确保程序能够正常运行。之后再慢慢向其中添加诸如面向对象等一些复杂的语言功能。即，先设计一个简化的成品，再逐步改良。

## 麻雀虽小、五脏俱全的程序设计语言

所需功能：

- 整数四则运算
- 字符串处理
- 对变量提供支持
- if语句及while语句等基本控制语句

Cloris语言是一种脚本语言，因此**不需要指定静态数据类型**，用户在使用时也**不必事先声明变量**，这样它的语法能较为简洁。

Cloris语言既不需要在使用变量前事先声明，也不需要指定变量的类型。用户可以将变量任意赋值为整数或字符串。只是这样一来，如果程序中出现字符串变量相减的语句，就会引起运行错误并终止。

Cloris语言的句末需要使用分号（;），如果在句末换行的话，分号可以省略。例如，下面的代码是合法的：

```java
sum = 0
i = 1
while i < 10 {
  sum = sum + i
  i = i + 1
}
sum
```

Cloris语言不支持类似于Java语言中`return`语句的功能，最后一条语句的计算结果就是整个程序的运行结果。

if语句的程序示例：

```java
even = 0
odd = 0
i = 1
while i < 10 {
    if i % 2 == 0 {
        even = even + i
    }else {
        odd = odd +i
    }
    i = i + 1
}
even + odd
```

## 句尾的分号

Cloris语言为了简化语法，**省去了`if`语句和`while`语句的条件表达式两侧的括号**，并允许用户**省略可以省略的句尾分号**。

此外，`{}`括起来的代码块中最后一条语句的句尾分号能够省略，即，若句尾直接跟着`}`，就不必使用分号。

```java
{x = 1; y = 2}
```

分号并不是一句语句结束的标识，而是代码块中语句之间的分隔符。因此，下面的代码块中含有3条语句，而不是2条：

```java
{x = 1; y = 2;}
```

其中第3条应该被视为1条空语句（没有内容的语句）。

空行也应被视为一条空语句，只不过省略了句尾的分号。

由于Cloris语言的句尾分号能够省略，换行与否将会大有不同。和Java等语言不同，此时换行符不会被简单地当做空白符处理。因此，Cloris语言的表达式和语句不能中途换行。只有语句的句尾，或`if`、`while`等语句的语句体之前的`{`后能够换行。`}`与`else`之间，或`else`与`{`之间不能换行。

错误写法：

```java
if i % 2 == 0  // error
{
    even = even + i
}			// error
else		// error
{
    odd = odd + i
}
```

第1行的换行出现在`{`之前，这是不允许的。

第4行没有将`}else{`写在一起，同样是错误的。

正确写法：

```java
if i % 2 == 0 {
    elen = even + i
} else {
    odd = odd +i
}
```

这里可能`else`部分的换行规则，不能符合所有人的喜好。

上述限制尽管增加了代码书写的难度，但如果允许代码在各种情况下换行，语言处理器的实现就会变得非常复杂。

## 含糊不得的语言

在设计一种语言时，设计者必须多加注意，确保语言中不出现模棱两可的歧义语法。

例如，Cloris语言中`while`语句体必须由大括号`{}`包围。`if`语句也是如此，条件表达式不一定非要用括号括起，但这两个语句体两侧必须使用括号。

如果像Java语言那样，语句体内仅含一条语句时可以不使用括号，就会出现下面这样的歧义：

```java
if 0 < x - y - z

// 这句 if 语句能有两种解读方式
if 0 < x - y {-z}
if 0 < x {-y - z}
```

前者的条件表达式是`0 < x - y`，如果 为真，则结果为`-z`；

后者的条件表达式是`0 < x`，如果为真，则结果为`-y - z`；

如果这种语言的语法明确规定了如何解释这种情况，语言处理器也做出了相应的实现，自然没有问题，否则这种语法就是模棱两可的。

要明确如何判断并不是一件容易的事，因此Stone语言的语句体必须用`{}`括起来，避免出现这个问题。

`if`语句的**dangling-else问题**是一个著名的二义语法，例如，Java语言允许下面这样的`if`语句：

```java
if(x > 0)
    if(y > 0)
        return x + y;
    else
        return -x;
```

由于语句体中只有一条语句，因此无需使用大括号。

这段代码的问题在于判断`else`应当对应哪一个`if`。在Java中，`else`与最近的一个`if`对应，因此不存在歧义问题。

如果在设计语言时欠考虑，就很容易出现这类dangling-else问题，使语言变得模棱两可，为此，设计者必须万分小心。



# 分割单词

语言处理器的第一个组成部分是**词法分析器（lexical analyzer、lexer或scanner）**。

程序的源代码最初只是一长串字符串，从内部来看，源代码中的换行也能用专门的（不可见）换行符表示，因此整个源代码是一种相连的长字符串。

这种长字符串很难处理，语言处理器通常会首先将字符串中的字符以单词为单位分组，切割成多个子字符串，这就是词法分析。

## Token对象

程序设计领域中的单词包含`+`或`==`之类的符号。例如，下面是某个程序中的一行代码：

```java
while i < 10 {
```

词法分析器会把它拆分为下面这样的字符串：

```java
"while" "i" "<" "10" "{"
```

这句代码被分割为了5个字符串，其中`while`是一个词语，但要把`<`与`{`也称作词语，的确有些不自然，因此，人们通常把词法分析的结果称为**单词（Token）**。

词法分析将**筛选出程序的解释与执行必需的成分**。单词之间的**空白或注释都会在这一阶段被去除**。例如：

```java
i = i + 1  // increment
i=i+1
```

这两行代码词法分析的结果相同，都是5个单词：

```java
"i" "=" "i" "+" "1"
```

经过词法分析之后，程序员便无需在处理代码的注释，也不用考虑单词之间是否含有空白符。

Token对象除了记录该单词对应的字符串，还会保存单词的类型、单词所处位置的行号等信息。

实际的单词是Token类的子类的对象。Token类根据单词的类型，又定义了不同的子类。

Cloris语言含有标识符、整型字面量和字符串字面量这三种类型的单词，每种单词都定义了对应的Token类的子类。每种子类都覆盖了Token类的`isIdentifier`（如果是标识符则为真）、`isNumber`（如果是整型字面量则为真）及`isString`（如果是字符串字面量则为真）方法，并根据具体类型返回相应的值。

Cloris语言还定义了一个特别的单词Token.EOF（end of file），用于表示程序结束。

类似的还有Token.EOL（end of line），用于表示换行符，不过它是一个String对象，即一个单纯的字符串。

## 通过正则表达式定义单词

要设计词法分析器，首先要**考虑每一种类型的单词的定义**，规定**怎样的字符串才能构成一个单词**。

这里最重要的不能有歧义，某个特定的字符串只能是某种特定类型的单词。

例如，字符串`123h`如果既能被解释为标识符，又能被解释为整型字面量，之后的处理就会相当麻烦。

这种单词的定义方式是不可取的。

```java
// 代码清单 Token.java
package cloris;

public abstract class Token {
    public static final Token EOF = new Token(-1){};	// end of file
    public static final String EOL = "\\n";	  // end of line
    private int lineNumber;
    
    protected Token(int line){
        lineNumber = line;
    }
		
  	public int getLineNumber() {return lineNumber;}
    public boolean isIdentifier() {return false;}
    public boolean isNumber() {return false;}
    public boolean isString() {return false;}
    public int getNumber() {throw new ClorisException("not number token");}
    public String getText() {return "";}
}
```

```java
// 代码清单 ClorisException.java
package cloris;
import cloris.ast.ASTree;

public class ClorisException extends RuntimeException {
		public ClorisException(String m) {super(m);}
    public ClorisException(String m, ASTree t) {
        super(m + " " + t.location());  
    }
}
```

Cloris语言支持三种类型的单词，即**标识符**、**整型字面量**及**字符串字面量**。

- **标识符（identifier）**指的是变量名、函数名或类名等名称。此外，`+`或`-`等运算符及括号等标点符号也属于标识符。有时在其他语言中，**标点符号**与**保留字**有时也会被归为另一种类型的单词，不过在Cloris语言中没有对它们加以区分，都作为标识符处理。
- **整型字面量（integer literal）**指的是127互殴2014等字符序列。如果仅适用整型这样的名称，可会被把它与程序执行过程中赋值给变量的整数值混同，因此这里使用整型字面量的名称，用于指代整数值的字符序列。
    - 例如，Java语言支持`0x1f`这样的16进制数表示。这种4个字符组成的字符串也是整型字面量，用一个整数值来表示的话，为31。
- **字符串字面量（string literal）**是一串用于表示字符串的字符序列。与Java等语言一样，被双引号`""`括起来的字符序列就是一个字符串字面量。双引号及其中的字符构成了一个字符串字面量，表示的是某一字符擦混类型的值，该值即为双引号内包含的字符序列。例如，字符串字面量“Java”表示的是字符串值Java。
    - 双引号之间可以使用`\n`、`\"`与`\\`这三种类型的转义字符。它们分别表示换行符、双引号和反斜杠。因此，尽管`x\n`这以字符串字面量含有5个字符，但它表示的是一个由2个字符组成的字符串值，其中第一个字符是`x`，第二个是换行符。

定义单词时使用正则表达式，能够借助**正则表达式（regular expression）**库简单地实现词法分析器。

正则表达式使用一些特殊的记号（元字符），在不同的正则表达式实现方式中，允许使用的元字符有所不同。

正则表达式元字符：

|    符号    |                含义                |
| :--------: | :--------------------------------: |
|  .(句点)   |           与任意字符匹配           |
|   [0-9]    |       与0至9中的某个数字匹配       |
|   [^0-9]   | 与0至9这些字符之外的某一个字符匹配 |
|    pat*    |       模式pat至少重复出现0次       |
|    pat+    |       模式pat至少重复出现1次       |
|    pat?    |        模式pat出现0次或1次         |
| pat1\|pat2 |         与模式1或模式2匹配         |
|     ()     |     将括号内视为一个完整的模式     |
|     \c     |  与单个字符c（元字符*或.等）匹配   |

借助正则表达式定义Cloris语言的单词。

正则表达式的写法遵循Java正则表达式库`java.util.regex`的规定。

整型字面量（从$0$到$9$中取出$1$个或以上的数字）：`[0-9]+`

























