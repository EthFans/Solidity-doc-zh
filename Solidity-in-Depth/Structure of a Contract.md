原文 [https://solidity.readthedocs.org/en/latest/structure-of-a-contract.html](https://solidity.readthedocs.org/en/latest/structure-of-a-contract.html)

Structure of a Contract

合约的结构

Contracts in Solidity are similar to classes in object-oriented languages. Each contract can contain declarations of **state variables**, **functions**, **function modifiers**, **events**, **structs types**and **enum types**. Furthermore, contracts can inherit from other contracts.

Solidity的合约和面向对象语言中的类的定义相似。每个合约包括了 状态变量，函数，函数修饰符，事件，结构类型 和枚举类型。另外，合约也可以从其他合约中继承 。

- State variables are values which are permanently stored in contract storage.


- Functions are the executable units of code within a contract.


- Function modifiers can be used to amend the semantics of functions in a declarative way.


- Events are convenience interfaces with the EVM logging facilities.


- Structs are custom defined types that can group several variables.


- Enums can be used to create custom types with a finite set of values.


- 状态变量是在合约存贮器中永久存贮的值


- 函数是合约中可执行单位的代码 


- 函数修饰符可以在声明的方式中补充函数的语义


- 事件是和EVM（以太虚拟机）日志设施的方便的接口


- 结构是一组用户定义的变量


- 枚举是用来创建一个特定值的集合的类型
