原文：

[https://solidity.readthedocs.org/en/latest/control-structures.html](https://solidity.readthedocs.org/en/latest/control-structures.html)

Expressions and Control Structures

**表达式和控制结构 **

Control Structures

Most of the control structures from C/JavaScript are available in Solidity except for switch and goto. So there is: if, else, while, for, break, continue, return, ? :, with the usual semantics known from C / JavaScript.

Parentheses can *not* be omitted for conditionals, but curly brances can be omitted around single-statement bodies.

Note that there is no type conversion from non-boolean to boolean types as there is in C and JavaScript, so if (1) { ... } is *not* valid Solidity.

控制结构

除了 switch和goto,solidity的绝大多数控制结构均来自于C / JavaScript，if, else, while, for, break, continue, return, ? :, 的语义均和C / JavaScript一样。

条件语句中的括号不能省略,但在单条语句前后的花括号可以省略。

注意,（Solidity中 ）没有象C和JavaScrip那样 ，从非布尔类型类型到布尔类型的转换,  所以

if (1){…}不是合法的Solidity（语句）。

Function Calls

Internal Function Calls

Functions of the current contract can be called directly (“internally”), also recursively, as seen in this nonsensical example:

**contract** c {

  **function** g(**uint** a) **returns** (**uint** ret) { **return** f(); }

  **function** f() **returns** (**uint** ret) { **return** g(7) **+** f(); }}

These function calls are translated into simple jumps inside the EVM. This has the effect that the current memory is not cleared, i.e. passing memory references to internally-called functions is very efficient. Only functions of the same contract can be called internally.

函数调用

内部函数调用

当前合约和函数可以直接调用(“内部”),也可以递归, 所以你会看到在这个“荒谬”的例子:

**contract** c {

  **function** g(**uint** a) **returns** (**uint** ret) { **return** f(); }

  **function** f() **returns** (**uint** ret) { **return** g(7) **+** f(); }}

这些函数调用在EMV里翻译成简单的jumps 语句。当前内存没有被清除,  即通过内存引用的函数是非常高效的。仅仅同一个合约的函数可在内部被调用。

External Function Calls

The expression this.g(8); is also a valid function call, but this time, the function will be called “externally”, via a message call and not directly via jumps. Functions of other contracts have to be called externally. For an external call, all function arguments have to be copied to memory.

When calling functions of other contracts, the amount of Wei sent with the call and the gas can be specified:

**contract** InfoFeed {

  **function** info() **returns** (**uint** ret) { **return** 42; }}

**contract** Consumer {

  InfoFeed feed;

  **function** setFeed(**address** addr) { feed **=** InfoFeed(addr); }

  **function** callFeed() { feed.info.value(10).gas(800)(); }}

Note that the expression InfoFeed(addr) performs an explicit type conversion stating that “we know that the type of the contract at the given address is InfoFeed” and this does not execute a constructor. We could also have used function setFeed(InfoFeed _feed) { feed = _feed; } directly. Be careful about the fact that feed.info.value(10).gas(800) only (locally) sets the value and amount of gas sent with the function call and only the parentheses at the end perform the actual call.

外部函数调用

表达式this.g(8);也是一个合法的函数调用,  但是这一次,这个函数将被称为“外部”的, 通过消息调用,而不是直接通过jumps调用。其他合约的函数也是外部调用。对于外部调用,所有函数参数必须被复制到内存中。

当调用其他合约的函数时, 这个调用的wei的数量被发送和gas被定义:

**contract** InfoFeed {

  **function** info() **returns** (**uint** ret) { **return** 42; }

}

**contract** Consumer {

  InfoFeed feed;

  **function** setFeed(**address** addr) { feed **=** InfoFeed(addr); }

  **function** callFeed() { feed.info.value(10).gas(800)(); }

}

注意：表达式InfoFeed(addr)执行显式的类型转换, 意思是“我们知道给定的地址的合约类型是InfoFeed”, 这并不执行构造函数。 我们也可以直接使用函数setFeed(InfoFeed _feed) { feed = _feed; }。 注意： feed.info.value(10).gas(800)是(本地)设置值和函数调用发送的gas数量, 只有最后的括号结束后才完成实际的调用。

Named Calls and Anonymous Function Parameters

Function call arguments can also be given by name, in any order, and the names of unused parameters (especially return parameters) can be omitted.

**contract** c {

  **function** f(**uint** key, **uint** value) { ... }

  **function** g() {

    *// named arguments*

    f({value**:** 2, key**:** 3});

  }

  *// omitted parameters*

  **function** func(**uint** k, **uint**) **returns**(**uint**) {

    **return** k;

  }

}

具名调用和匿名函数参数

有参数的函数调用可以有名字,不使用参数的名字(特别是返回参数)可以省略。

**contract** c {

  **function** f(**uint** key, **uint** value) { ... }

  **function** g() {

    *// named arguments   **具名参数*

    f({value**:** 2, key**:** 3});

  }

  *// omitted parameters **省略名字的参数*

  **function** func(**uint** k, **uint**) **returns**(**uint**) {

    **return** k;

  }

}

Order of Evaluation of Expressions

The evaluation order of expressions is not specified (more formally, the order in which the children of one node in the expression tree are evaluated is not specified, but they are of course evaluated before the node itself). It is only guaranteed that statements are executed in order and short-circuiting for boolean expressions is done.

表达式计算的次序

表达式的计算顺序是不确定的（准确地说是, 顺序表达式树中的子节点表达式计算顺序是不确定的的, 但他们对节点本身，计算表达式顺序当然是确定的)。只保证语句执行顺序，以及布尔表达式的短路规则。

Assignment

Destructuring Assignments and Returning Multiple Values

Solidity internally allows tuple types, i.e. a list of objects of potentially different types whose size is a constant at compile-time. Those tuples can be used to return multiple values at the same time and also assign them to multiple variables (or LValues in general) at the same time:

赋值

析构赋值并返回多个值

Solidity内部允许元组类型,即一系列的不同类型的对象的大小在编译时是一个常量。这些元组可以用来同时返回多个值，并且同时将它们分配给多个变量(或左值运算)

**contract** C {

  **uint**[] data;

  **function** f() **returns** (**uint**, **bool**, **uint**) {

    **return** (7, **true**, 2);

  }

  **function** g() {

    *// Declares and assigns the variables. Specifying the type explicitly is not possible.*

    **var** (x, b, y) **=** f();

    *// Assigns to a pre-existing variable.*

    (x, y) **=** (2, 7);

    *// Common trick to swap values -- does not work for non-value storage types.*

    (x, y) **=** (y, x);

    *// Components can be left out (also for variable declarations).*

    *// If the tuple ends in an empty component,*

    *// the rest of the values are discarded.*

    (data.length,) **=** f(); *// Sets the length to 7*

    *// The same can be done on the left side.*

    (,data[3]) **=** f(); *// Sets data[3] to 2*

    *// Components can only be left out at the left-hand-side of assignments, with*

    *// one exception:*

    (x,) **=** (1,);

    *// (1,) is the only way to specify a 1-component tuple, because (1) is*

    *// equivalent to 1.*

  }

}

**contract** C {

  **uint**[] data;

  **function** f() **returns** (**uint**, **bool**, **uint**) {

    **return** (7, **true**, 2);

  }

  **function** g() {

    *// Declares and assigns the variables. Specifying the type explicitly is not possible. **声明和赋值变量，不必显示定义类型*

    **var** (x, b, y) **=** f();

    *// Assigns to a pre-existing variable.** 赋值给已经存在的变量*

    (x, y) **=** (2, 7);

    *// Common trick to swap values -- does not work for non-value storage types. **交换值的技巧-对非值存储类型不起作用*

    (x, y) **=** (y, x);

    *// Components can be left out (also for variable declarations). **元素可排除（对变量声明也适用）*

    *// If the tuple ends in an empty component, **如果元组是以空元素为结尾*

    *// the rest of the values are discarded.  **值的其余部分被丢弃*

    (data.length,) **=** f(); *// Sets the length to 7 **设定长度为7*

    *// The same can be done on the left side. **同样可以在左侧做*

    (,data[3]) **=** f(); *// Sets data[3] to 2 ** 将data[3] 设为2*

    *// Components can only be left out at the left-hand-side of assignments, with*

    *// one exception:    **组件只能在赋值的左边被排除，有一个例外*

    (x,) **=** (1,);

    *// (1,) is the only way to specify a 1-component tuple, because (1) is     **(1,)是定义一个元素的元组，（1）是等于1*

    *// equivalent to 1.*

  }}

Complications for Arrays and Structs

The semantics of assignment are a bit more complicated for non-value types like arrays and structs. Assigning *to* a state variable always creates an independent copy. On the other hand, assigning to a local variable creates an independent copy only for elementary types, i.e. static types that fit into 32 bytes. If structs or arrays (including bytes and string) are assigned from a state variable to a local variable, the local variable holds a reference to the original state variable. A second assignment to the local variable does not modify the state but only changes the reference. Assignments to members (or elements) of the local variable *do* change the state.

数组和结构体的组合

对于象数组和结构体这样的非值类型，赋值的语义更复杂些。赋值到一个状态变量总是需要创建一个独立的副本。另一方面,对基本类型来说，赋值到一个局部变量需要创建一个独立的副本, 即32字节的静态类型。如果结构体或数组(包括字节和字符串)从状态变量被赋值到一个局部变量,  局部变量则保存原始状态变量的引用。第二次赋值到局部变量不修改状态，只改变引用。赋值到局部变量的成员(或元素)将改变状态。

Exceptions

There are some cases where exceptions are thrown automatically (see below). You can use the throwinstruction to throw an exception manually. The effect of an exception is that the currently executing call is stopped and reverted (i.e. all changes to the state and balances are undone) and the exception is also “bubbled up” through Solidity function calls (exceptions are send and the low-level functions call and callcode, those return false in case of an exception).

Catching exceptions is not yet possible.

In the following example, we show how throw can be used to easily revert an Ether transfer and also how to check the return value of send:

异常

有一些自动抛出异常的情况(见下文)。您可以使用throw 指令手动抛出一个异常。异常的影响是当前执行的调用被停止和恢复(即所有状态和余额的变化均没有发生)。另外， 异常也可以通过Solidity 函数 “冒出来”, (一旦“异常”发生, 就send "exceptions", call和callcode底层函数就返回false)。

捕获异常是不可能的。

在接下来的例子中,我们将演示如何轻松恢复一个Ether转移,以及如何检查send的返回值:

**contract** Sharer {

    **function** sendHalf(**address** addr) **returns** (**uint** balance) {

        **if** (**!**addr.send(msg.value**/**2))

            **throw**; *// also reverts the transfer to Sharer*

        **return** **this**.balance;

    }

}

**contract** Sharer {

    **function** sendHalf(**address** addr) **returns** (**uint** balance) {

        **if** (**!**addr.send(msg.value**/**2))

            **throw**; *// also reverts the transfer to Sharer  **也恢复Sharer的转移*

        **return** **this**.balance;

    }

}

Currently, there are three situations, where exceptions happen automatically in Solidity:

1. If you access an array beyond its length (i.e. x[i] where i >= x.length)


1. If a function called via a message call does not finish properly (i.e. it runs out of gas or throws an exception itself).


1. If a non-existent function on a library is called or Ether is sent to a library.

Internally, Solidity performs an “invalid jump” when an exception is thrown and thus causes the EVM to revert all changes made to the state. The reason for this is that there is no safe way to continue execution, because an expected effect did not occur. Because we want to retain the atomicity of transactions, the safest thing to do is to revert all changes and make the whole transaction (or at least call) without effect.

目前,Solidity异常自动发生,有三种情况, :

1. 如果你访问数组超出其长度 (即x[i] where i >= x.length)


1. 如果一个通过消息调用的函数没有正确的执行结束(即gas用完，或本身抛出异常)。


1. 如果一个库里不存在的函数被调用，或Ether被发送到一个函数库里。

在内部,当抛出异常时 ,Solidity就执行“非法jump”, 从而导致EMV(Ether虚拟机)恢复所有更改状态。这个原因是没有安全的方法可以继续执行,   预期的结果没有发生。由于我们想保留事务的原子性,（所以）最安全的做法是恢复所有更改，并使整个事务(或者至少调用)没有受影响。

© Copyright 2015, Ethereum. Revision 37381072.

Built with [Sphinx](http://sphinx-doc.org/) using a [theme](https://github.com/snide/sphinx_rtd_theme) provided by [Read the Docs](https://readthedocs.org/).

 Read the Docsv: latest 
