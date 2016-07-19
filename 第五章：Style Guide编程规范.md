原文：[https://solidity.readthedocs.org/en/latest/style-guide.html](https://solidity.readthedocs.org/en/latest/style-guide.html)

Style Guide

编程规范

Introduction

概述

This guide is intended to provide coding conventions for writing solidity code. This guide should be thought of as an evolving document that will change over time as useful conventions are found and old conventions are rendered obsolete.

Many projects will implement their own style guides. In the event of conflicts, project specific style guides take precedence.

The structure and many of the recommendations within this style guide were taken from python’s[pep8 style guide](https://www.python.org/dev/peps/pep-0008/).

The goal of this guide is *not* to be the right way or the best way to write solidity code. The goal of this guide is *consistency*. A quote from python’s [pep8](https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds) captures this concept well.

A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is most important. But most importantly: know when to be inconsistent – sometimes the style guide just doesn’t apply. When in doubt, use your best judgment. Look at other examples and decide what looks best. And don’t hesitate to ask!

本指南用于提供编写Solidity的编码规范，本指南会随着后续需求不断修改演进，可能会增加新的更合适的规范，旧的不适合的规范会被废弃。

当然，很多项目可能有自己的编码规范，如果存在冲突，请参考项目的编码规范。

本指南的结构及规范建议大都来自于python的pep8编码规范（参见[https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds](https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds)）。

本指南不是说必须完全按照指南的要求进行solidity编码，而是提供一个总体的一致性要求，这个和pep8的理念相似（译注：pep8的理念大概是：强制的一致性是非常愚蠢的行为，参见：[https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds](https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds)）。

本指南是为了提供编码风格的一致性，因此一致性这一理念是很重要的，在项目中编码风格的一致性更加重要，而在同一个函数或模块中风格的一致性是最重要的。而最最最重要的是：你要知道什么时候需要保持一致性，什么时候不需要保持一致性，因为有时候本指南不一定适用，你需要根据自己的需要进行权衡。可以参考下边的例子决定哪一种对你来说是最合适的。

Code Layout

代码布局

Indentation

缩进

Use 4 spaces per indentation level.

每行使用4个空格缩进

Tabs or Spaces

tab或空格

Spaces are the preferred indentation method.

Mixing tabs and spaces should be avoided.

空格是首选缩进方式

禁止tab和空格混合使用

Blank Lines

回车（空行）

Surround top level declarations in solidity source with two blank lines.

两个合约之间增加两行空行

Yes:

规范的方式：

**contract** A {

    ...}

**contract** B {

    ...}

**contract** C {

    ...}

No:

不规范的方式：

**contract** A {

    ...}**contract** B {

    ...}

**contract** C {

    ...}

Within a contract surround function declarations with a single blank line.

Blank lines may be omitted between groups of related one-liners (such as stub functions for an abstract contract)

合约内部函数之间需要回车，如果是函数声明和函数实现一起则需要两个回车

Yes:

规范的方式：

**contract** A {

    **function** spam();

    **function** ham();

}

**contract** B is A {

    **function** spam() {

        ...

    }

    **function** ham() {

        ...

    }

}

No:

不规范的方式：

**contract** A {

    **function** spam() {

        ...

    }

    **function** ham() {

        ...

    }}

Source File Encoding

源文件编码方式

UTF-8 or ASCII encoding is preferred.

首选UTF-8或者ASCII编码

Imports

引入

Import statements should always be placed at the top of the file.

一般在代码开始进行引入声明

规范的方式：

Yes:

**import** "owned";

**contract** A {

    ...}

**contract** B is owned {

    ...}

No:

不规范的方式：

**contract** A {

    ...}

**import** "owned";

**contract** B is owned {

    ...}

Whitespace in Expressions

表达式中的空格使用方法

Avoid extraneous whitespace in the following situations:

以下场景避免使用空格

- Immediately inside parenthesis, brackets or braces.


- 括号、中括号，花括号之后避免使用空格

Yes规范的方式: spam(ham[1], Coin({name: “ham”}));

No不规范的方式: spam( ham[ 1 ], Coin( { name: “ham” } ) );

- Immediately before a comma, semicolon:


- 逗号和分号之前避免使用空格

Yes规范的方式: function spam(uint i, Coin coin);

No不规范的方式: function spam(uint i , Coin coin) ;

- More than one space around an assignment or other operator to align with another:


- 赋值符前后避免多个空格

Yes:

规范的方式：

x **=** 1;

y **=** 2;

long_variable **=** 3;

No:

不规范的方式：

x             **=** 1;

y             **=** 2;

long_variable **=** 3;

Control Structures

控制结构

The braces denoting the body of a contract, library, functions and structs should:

合约、库。函数、结构体的花括号使用方法：

- open on the same line as the declaration


- 左花括号和声明同一行


- close on their own line at the same indentation level as the beginning of the declaration.


- 右括号和左括号声明保持相同缩进位置。


- The opening brace should be proceeded by a single space.


- 左括号后应回车

Yes:

规范的方式：

**contract** Coin {

    **struct** Bank {

        **address** owner;

        **uint** balance;

    }

}

No:

不规范的方式：

**contract** Coin

{

    **struct** Bank {

        **address** owner;

        **uint** balance;

    }

}

The same recommendations apply to the control structures if, else, while, and for.

Additionally there should be a single space between the control structures if, while, and for and the parenthetic block representing the conditional, as well as a single space between the conditional parenthetic block and the opening brace.

以上建议也同样适用于if、else、while、for。

此外，if、while、for条件语句之间必须空行

Yes:

规范的方式：

**if** (...) {

    ...

}

**for** (...) {

    ...

}

No:

不规范的方式：

**if** (...)

{

    ...

}

**while**(...)

{

}

**for** (...)

 {

    ...;

}

For control structures who’s body contains a single statement, omitting the braces is ok *if* the statement is contained on a single line.

对于控制结构内部如果只有单条语句可以不需要使用括号。

Yes:

规范的方式：

**if** (x **<** 10)

    x **+=** 1;

No:

不规范的方式：

**if** (x **<** 10)

    someArray.push(Coin({

        name**:** 'spam',

        value**:** 42

    }));

For if blocks which have an else or else if clause, the else should be placed on it’s own line following the previous closing parenthesis. The parenthesis for the else block should follow the same rules as the other conditional control structures.

对于if语句如果包含else或者else if语句，则else语句要新起一行。else和else if的内部规范和if相同。

Yes:

规范的方式：

**if** (x **<** 3) {

    x **+=** 1;

}

**else** {

    x **-=** 1;

}

**if** (x **<** 3)

    x **+=** 1;

**else**

    x **-=** 1;

No:

不规范的方式：

**if** (x **<** 3) {

    x **+=** 1;} 

**else** {

    x **-=** 1;}

Function Declaration

函数声明

For short function declarations, it is recommended for the opening brace of the function body to be kept on the same line as the function declaration.

The closing brace should be at the same indentation level as the function declaration.

The opening brace should be preceeded by a single space.

对于简短函数声明，建议将函数体的左括号和函数名放在同一行。

右括号和函数声明保持相同的缩进。

左括号和函数名之间要增加一个空格。

Yes:

规范的方式：

**function** increment(**uint** x) **returns** (**uint**) {

    **return** x **+** 1;

}

**function** increment(**uint** x) **public** onlyowner **returns** (**uint**) {

    **return** x **+** 1;

}

No:

不规范的方式：

**function** increment(**uint** x) **returns** (**uint**)

{

    **return** x **+** 1;

}

**function** increment(**uint** x) **returns** (**uint**)

{

    **return** x **+** 1;

}

**function** increment(**uint** x) **returns** (**uint**)

 {

    **return** x **+** 1;

}

**function** increment(**uint** x) **returns** (**uint**) 

{

    **return** x **+** 1;

}

The visibility modifiers for a function should come before any custom modifiers.

默认修饰符应该放在其他自定义修饰符之前。

Yes:

规范的方式：

**function** kill() **public** onlyowner {

    selfdestruct(owner);

}

No:

不规范的方式：

**function** kill() onlyowner **public** {

    selfdestruct(owner);

}

For long function declarations, it is recommended to drop each arguent onto it’s own line at the same indentation level as the function body. The closing parenthesis and opening bracket should be placed on their own line as well at the same indentation level as the function declaration.

对于参数较多的函数声明可将所有参数逐行显示，并保持相同的缩进。函数声明的右括号和函数体左括号放在同一行，并和函数声明保持相同的缩进。

Yes:

规范的方式：

**function** thisFunctionHasLotsOfArguments(

    **address** a,

    **address** b,

    **address** c,

    **address** d,

    **address** e,

    **address** f,

) {

    do_something;

}

No:

不规范的方式：

**function** thisFunctionHasLotsOfArguments(**address** a, **address** b, **address** c,

    **address** d, **address** e, **address** f) {

    do_something;

}

**function** thisFunctionHasLotsOfArguments(**address** a,

                                        **address** b,

                                        **address** c,

                                        **address** d,

                                        **address** e,

                                        **address** f) {

    do_something;

}

**function** thisFunctionHasLotsOfArguments(

    **address** a,

    **address** b,

    **address** c,

    **address** d,

    **address** e,

    **address** f) {

    do_something;

}

If a long function declaration has modifiers, then each modifier should be dropped to it’s own line.

如果函数包括多个修饰符，则需要将修饰符分行并逐行缩进显示。函数体左括号也要分行。

Yes:

规范的方式：

**function** thisFunctionNameIsReallyLong(**address** x, **address** y, **address** z)

    **public**

    onlyowner

    priced

    **returns** (**address**)

{

    do_something;

}

**function** thisFunctionNameIsReallyLong(

    **address** x,

    **address** y,

    **address** z,)

    **public**

    onlyowner

    priced

    **returns** (**address**)

{

    do_something;

}

No:

不规范的方式：

**function** thisFunctionNameIsReallyLong(**address** x, **address** y, **address** z)

                                      **public**

                                      onlyowner

                                      priced

                                      **returns** (**address**) {

    do_something;

}

**function** thisFunctionNameIsReallyLong(**address** x, **address** y, **address** z)

    **public** onlyowner priced **returns** (**address**){

    do_something;

}

**function** thisFunctionNameIsReallyLong(**address** x, **address** y, **address** z)

    **public**

    onlyowner

    priced

    **returns** (**address**) {

    do_something;

}

For constructor functions on inherited contracts who’s bases require arguments, it is recommended to drop the base constructors onto new lines in the same manner as modifiers if the function declaration is long or hard to read.

对于需要参数作为构造函数的派生合约，如果函数声明太长或者难于阅读，建议将其构造函数中涉及基类的构造函数分行独立显示。

Yes:

规范的方式：

**contract** A is B, C, D {

    **function** A(**uint** param1, **uint** param2, **uint** param3, **uint** param4, **uint** param5)

        B(param1)

        C(param2, param3)

        D(param4)

    {

        *// do something with param5*

    }

}

No:

不规范的方式：

**contract** A is B, C, D {

    **function** A(**uint** param1, **uint** param2, **uint** param3, **uint** param4, **uint** param5)

    B(param1)

    C(param2, param3)

    D(param4)

    {

        *// do something with param5*

    }

}

**contract** A is B, C, D {

    **function** A(**uint** param1, **uint** param2, **uint** param3, **uint** param4, **uint** param5)

        B(param1)

        C(param2, param3)

        D(param4) {

        *// do something with param5*

    }

}

These guidelines for function declarations are intended to improve readability. Authors should use their best judgement as this guide does not try to cover all possible permutations for function declarations.

对于函数声明的编程规范主要用于提升可读性，本指南不可能囊括所有编程规范，对于不涉及的地方，程序猿可发挥自己的主观能动性。

Mappings

映射

TODO

待完成

Variable Declarations

变量声明

Declarations of array variables should not have a space between the type and the brackets.

对于数组变量声明，类型和数组中括号直接不能有空格。

Yes: uint[] x; No: uint [] x;

规范的方式: uint[] x; 不规范的方式: uint [] x;

Other Recommendations

其他建议

- Surround operators with a single space on either side.


- 赋值运算符两边要有一个空格

Yes:

规范的方式：

x **=** 3;x **=** 100 **/** 10;x **+=** 3 **+** 4;x **|=** y **&&** z;

No:

不规范的方式：

x**=**3;x **=** 100**/**10;x **+=** 3**+**4;x **|=** y**&&**z;

- Operators with a higher priority than others can exclude surrounding whitespace in order to denote precedence. This is meant to allow for improved readability for complex statement. You should always use the same amount of whitespace on either side of an operator:


- 为了显示优先级，优先级运算符和低优先级运算符之间要有空格，这也是为了提升复杂声明的可读性。对于运算符两侧的空格数目必须保持一致。

Yes:

规范的方式：

x **=** 2***\***3 **+** 5;x **=** 2*****y **+** 3*****z;x **=** (a**+**b) ***** (a**-**b);

No:

不规范的方式：

x **=** 2***\*** 3 **+** 5;x **=** y**+**z;x **+=**1;

Naming Conventions

命名规范

Naming conventions are powerful when adopted and used broadly. The use of different conventions can convey significant *meta* information that would otherwise not be immediately available.

The naming recommendations given here are intended to improve the readability, and thus they are not rules, but rather guidelines to try and help convey the most information through the names of things.

Lastly, consistency within a codebase should always supercede any conventions outlined in this document.

命名规范是强大且广泛使用的，使用不同的命名规范可以传递不同的信息。

以下建议是用来提升代码的可读性，因此被规范不是规则而是用于帮助更好的解释相关代码。

最后，编码风格的一致性是最重要的。

Naming Styles

命名方式

To avoid confusion, the following names will be used to refer to different naming styles.

为了防止混淆，以下命名用于说明（描述）不同的命名方式。

- b (single lowercase letter)


- b（单个小写字母）


- B (single uppercase letter)


- B（单个大写字母）


- lowercase


- 小写


- lower_case_with_underscores


- 有下划线的小写


- UPPERCASE


- 大写


- UPPER_CASE_WITH_UNDERSCORES


- 有下划线的大写


- CapitalizedWords (or CapWords)


- CapWords规范（首字母大写）


- mixedCase (differs from CapitalizedWords by initial lowercase character!)


- 混合方式（与CapitalizedWords的不同在于首字母小写!）


- Capitalized_Words_With_Underscores


- 有下划线的首字母大写（译注：对于python来说不建议这种方式）

**Note**

注意

When using abbreviations in CapWords, capitalize all the letters of the abbreviation. Thus HTTPServerError is better than HttpServerError.

当使用CapWords规范（首字母大写）的缩略语时，缩略语全部大写，比如HTTPServerError 比HttpServerError就好理解一点。

Names to Avoid

避免的命名方式

- l - Lowercase letter el  小写的l


- O - Uppercase letter oh 大写的o


- I - Uppercase letter eye 大写的i

Never use any of these for single letter variable names. They are often indistinguishable from the numerals one and zero.

永远不要用字符‘l'(小写字母el(就是读音，下同))，‘O'(大写字母oh)，或‘I'(大写字母eye)作为单字符的变量名。在某些字体中这些字符不能与数字1和0分辨。试着在使用‘l'时用‘L'代替。 

Contract and Library Names

合约及库的命名

Contracts should be named using the CapWords style.

合约应该使用CapWords规范命名（首字母大写）。

Events

事件

Events should be named using the CapWords style.

事件应该使用CapWords规范命名（首字母大写）。

Function Names

函数命名

Functions should use mixedCase.

函数名使用大小写混合

Function Arguments

函数参数命名

When writing library functions that operate on a custom struct, the struct should be the first argument and should always be named self.

当定义一个在自定义结构体上的库函数时，结构体的名称必须具有自解释能力。

Local and State Variables

局部变量命名

Use mixedCase.

大小写混合

Constants

常量命名

Constants should be named with all capital letters with underscores separating words. (for example:MAX_BLOCKS)

常量全部使用大写字母并用下划线分隔。

Modifiers

修饰符命名

Function modifiers should use lowercase words separated by underscores.

功能修饰符使用小写字符并用下划线分隔。

Avoiding Collisions

避免冲突

- single_trailing_underscore_


- 单个下划线结尾

This convention is suggested when the desired name collides with that of a built-in or otherwise reserved name.

当和内置或者保留名称冲突时建议使用本规范。

General Recommendations

通用建议

TODO

待完成
