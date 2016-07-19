原文[https://solidity.readthedocs.org/en/latest/layout-of-source-files.html](https://solidity.readthedocs.org/en/latest/layout-of-source-files.html)

Layout of a Solidity Source File

Solidity **源文件的布局**

Source files can contain an arbitrary number of contract definitions and include directives.

源文件包括任意数量的合约定义和include指令

Importing other Source Files

Syntax and Semantics

Solidity supports import statements that are very similar to those available in JavaScript (from ES6 on), although Solidity does not know the concept of a “default export”.

引入其他源文件

语法和语义

Solidity支持 import语句，非常类似于JavaScript（ES6）,虽然Solidity不知道“缺省导出”的概念。

At a global level, you can use import statements of the following form:

import “filename”; will import all global symbols from “filename” (and symbols imported there) into the current global scope (different than in ES6 but backwards-compatible for Solidity).

import * as symbolName from “filename”; creates a new global symbol symbolName whose members are all the global symbols from “filename”.

在全局层次上，你可以用下列形式使用import语句

import "filename"; 

将会从"filename"导入所有的全局符号(和当前导入的符号)到当前的全局范围里（不同于ES6，但是Solidity保持向后兼容）

**import** ***** as symbolName from "filename";

创立了一个全局的符号名 **symbolName**,其中的成员就来自“filename”的所有符号

import {symbol1 as alias, symbol2} from “filename”; creates new global symbols alias and symbol2 which reference symbol1 and symbal2 from “filename”, respectively.

Another syntax is not part of ES6, but probably convenient:

import “filename” as symbolName; is equivalent to import * as symbolName from “filename”;.

import {symbol1 as alias, symbol2} from “filename”; 

 将创立一个新的全局变量别名：alias 和 symbol2，  它将分别从"filename" 引入symbol1 和 symbol2

另外，Solidity语法不是ES6的子集，但可能（使用）更便利

import “filename” as symbolName; 

  等价于 import * as symbolName from “filename”;.

Paths

In the above, filename is always treated as a path with / as directory separator, . as the current and .. as the parent directory. Path names that do not start with . are treated as absolute paths.

To import a file x from the same directory as the current file, use import ”./x” as x;. If you use import “x” as x; instead, a different file could be referenced (in a global “include directory”).

It depends on the compiler (see below) how to actually resolve the paths. In general, the directory hierarchy does not need to strictly map onto your local filesystem, it can also map to resources discovered via e.g. ipfs, http or git.

路径

-       在上面，文件名总是用/作为目录分割符，. 是当前的目录，.. 是父目录，路径名称不用.开头的都将视为绝对路径。


-      从同一个目录下import 一个文件 x 作为当前文件，用  import ”./x” as x;  如果使用 


-  import “x” as x;    是不同的文件引用（在全局中使用"include directory"）,。


-      它将依赖于编译器（见后）来解析路径。通常，目录层次不必严格限定映射到你的本地文件系统，它也可以映射到ipfs,http或git上的其他资源

Use in actual Compilers

When the compiler is invoked, it is not only possible to specify how to discover the first element of a path, but it is possible to specify path prefix remappings so that e.g. github.com/ethereum/dapp-bin/library is remapped to /usr/local/dapp-bin/library and the compiler will read the files from there. If remapping keys are prefixes of each other, the longest is tried first. This allows for a “fallback-remapping” with e.g. “” maps to “/usr/local/include/solidity”.

使用真正的编译器

     当编译器启动时，不仅可以定义如何找到第一个元素的路径，也可能定义前缀重映射的路径，如 github.com/ethereum/dapp-bin/library将重映射到 /usr/local/dapp-bin/library ，编译器将从这个路径下读取文件。  如果重映射的keys是前缀， （编译器将尝试）最长的路径。允许回退并且映射到“/usr/local/include/solidity”。

**solc**:

For solc (the commandline compiler), these remappings are provided as key=value arguments, where the =value part is optional (and defaults to key in that case). All remapping values that are regular files are compiled (including their dependencies). This mechanism is completely backwards-compatible (as long as no filename contains a =) and thus not a breaking change.

So as an example, if you clone github.com/ethereum/dapp-bin/ locally to /usr/local/dapp-bin, you can use the following in your source file:

import “github.com/ethereum/dapp-bin/library/iterable_mapping.sol” as it_mapping;

and then run the compiler as

solc github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ source.sol

Note that solc only allows you to include files from certain directories: They have to be in the directory (or subdirectory) of one of the explicitly specified source files or in the directory (or subdirectory) of a remapping target. If you want to allow direct absolute includes, just add the remapping =/.

If there are multiple remappings that lead to a valid file, the remapping with the longest common prefix is chosen.

solc:

solc(行命令编译器)，重映射将提供  key=值参数,  =值 部分是可选的（缺省就是key ） 。 

所有重映射的常规文件都将被编译（包括他们的依赖文件），这个机制完全向后兼容（只要没有文件名包含 a=） ，这不是一个很大的变化，

    比如 ，如果你从 github.com/ethereum/dapp-bin/ 克隆到本地 /usr/local/dapp-bin, 你可以用下列源文件

import “github.com/ethereum/dapp-bin/library/iterable_mapping.sol” as it_mapping;

运行编译器时如下

solc github.com/ethereum/dapp-bin/=/usr/local/dapp-bin/ source.sol

注意： solc仅仅允许你从特定的目录下include文件，他们必须是一个显式定义的，包含目录或子目录的源文件， 或者是重映射目标的目录（子目录）。如果你允许直接include, 要增加remapping =/. 

如果有多个重映射，就要做一个合法文件，文件中选择最长的公共前缀

**browser-solidity**:

The [browser-based compiler](https://chriseth.github.io/browser-solidity) provides an automatic remapping for github and will also automatically retrieve the file over the network: You can import the iterable mapping by e.g. import “github.com/ethereum/dapp-bin/library/iterable_mapping.sol” as it_mapping;.

Other source code providers may be added in the future.

基于浏览器的solidity

基于浏览器的编译器提供了从github上的自动重映射,并且自动检索网络上的文件，你可以import 迭代映射  如

import “github.com/ethereum/dapp-bin/library/iterable_mapping.sol” as it_mapping;.

其他源代码提供者可以以后增加进来。

Comments

Single-line comments (//) and multi-line comments (/*...*/) are possible.

There are special types of comments called natspec comments (documentation yet to be written). These are introduced by triple-slash comments (///) or using double asterisks (/** ... */). Right in front of function declarations or statements, you can use doxygen-style tags inside them to document functions, annotate conditions for formal verification and provide a **confirmation text** that is shown to users if they want to invoke a function.

 注释

可用单行注释  (//) 和多行注释  (/*...*/)

有种特别的注释 叫做 “natspec ” （文档以后写出来），在函数声明或定义的右边用三个斜杠（///）或者用 两个星号  (/** ... */). 。**如果想要调用一个函数，可以使用doxygen-style标签里面文档功能,形式验证,并提供一个确认条件的文本注释显示给用户。**
