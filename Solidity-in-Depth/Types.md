Types

Solidity is a statically typed language, which means that the type of each variable (state and local) needs to be specified (or at least known - see [Type Deduction](https://solidity.readthedocs.org/en/latest/types.html#type-deduction) below) at compile-time. Solidity provides several elementary types which can be combined to complex types.

类型

Solidity是一种静态类型语言，意思是每个变量（声明和本地）在编译时刻都要定义 （或者至少要知晓，参看 后面的类型导出 ）。

Solidity提供几个基本类型组合成复杂类型。

Value Types

The following types are also called value types because variables of these types will always be passed by value, i.e. they are always copied when they are used as function arguments or in assignments.

变量类型

以下类型被叫做值类型，因为这些类型的变量总是要被赋值，作为函数参数或者在赋值中，总需要拷贝。

Booleans

bool: The possible values are constants true and false.

Operators:

- ! (logical negation)


- && (logical conjunction, “and”)


- || (logical disjunction, “or”)


- == (equality)


- != (inequality)

The operators || and && apply the common short-circuiting rules. This means that in the expressionf(x) || g(y), if f(x) evaluates to true, g(y) will not be evaluated even if it may have side-effects.

布尔类型

布尔：可能的常量值 是 真或假

操作符：

- ！（逻辑非）


- &&（逻辑与，“and"）


- ||   (逻辑或，”or“)


- ==（相等）


- ！=（不等）


- 操作符||和&&可以应用常规短路规则，即 表达式 f(x) || g(y), 如果f(x) 已是真，g(y）将不用计算，即使它有副作用 （真||任意值 均为真，假&&任意布尔值 均为假）。

Integers

int• / uint•: Signed and unsigned integers of various sizes. Keywords uint8 to uint256 in steps of 8(unsigned of 8 up to 256 bits) and int8 to int256. uint and int are aliases for uint256 and int256, respectively.

Operators:

- Comparisons: <=, <, ==, !=, >=, > (evaluate to bool)


- Bit operators: &, |, ^ (bitwise exclusive or), ~ (bitwise negation)


- Arithmetic operators: +, -, unary -, unary +, *, /, % (remainder), ** (exponentiation)

整型

int• / uint•: 是有符号和无符号的整数，关键字uint8 到 uint256 步长8 (从8到256位的无符号整数 )

 uint 和 int 分别是 uint256 和 int256的别名

操作符：

- 比较 ： <=,<,==,!=,>=,> (计算布尔量)


- 位操作符： &，|，^(位异或)，~（位取反）


- 算术操作符：+，-，一元-，一元+，*，/,%(取余数)，**（幂次方）

Address

address: Holds a 20 byte value (size of an Ethereum address). Address types also have members(see [Functions on addresses](#functions-on-addresses)) and serve as base for all contracts.

Operators:

- <=, <, ==, !=, >= and >

地址

地址： 20字节（一个  Ethereum地址），地址的类型也可以有成员（请看地址功能 (#functions-on-addresses)）  作为所有合约的base

操作符：

- <=, <, ==, !=, >= 和 >

Members of Addresses

- balance and send

It is possible to query the balance of an address using the property balance and to send Ether (in units of wei) to an address using the send function:

**address** x **=** 0x123;

**address** myAddress **=** **this**;

**if** (x.balance **<** 10 **&&** myAddress.balance **>=** 10) x.send(10);

地址成员：

- 账户余额（balance）和发送（send）

若查询到有资产余额的地址，然后发送  Ether(以wei为单位) 到send 函数的地址上

**address** x **=** 0x123;

**address** myAddress **=** **this**;

**if** (x.balance **<** 10 **&&** myAddress.balance **>=** 10) x.send(10);

**Note**

If x is a contract address, its code (more specifically: its fallback function, if present) will be executed together with the send call (this is a limitation of the EVM and cannot be prevented). If that execution runs out of gas or fails in any way, the Ether transfer will be reverted. In this case,send returns false.

注解

如果x是合约地址，它的代码（特别的指出 如果有回退函数，） 将和send 调用一起执行（这是EVM的限制，不能修改） 如果用完了gas或者失败，Ether转移将被回退，这种情况下，send返回false

- call and callcode

Furthermore, to interface with contracts that do not adhere to the ABI, the function call is provided which takes an arbitrary number of arguments of any type. These arguments are padded to 32 bytes and concatenated. One exception is the case where the first argument is encoded to exactly four bytes. In this case, it is not padded to allow the use of function signatures here.

- 调用(call)和调用码( callcode)

另外，和合约的接口不是附在ABI 上，函数调用可以引用任意数量的参数，这些参数要填补成32字节，并被拼接。一个例外的情况是第一个参数被确切的编码成4字节，这种情况下，不用填补，直接使用函数符号

**address** nameReg **=** 0x72ba7d8e73fe8eb666ea66babc8116a41bfb10e2;

nameReg.call("register", "MyName");

nameReg.call(**bytes4**(sha3("fun(uint256)")), a);

call returns a boolean indicating whether the invoked function terminated (true) or caused an EVM exception (false). It is not possible to access the actual data returned (for this we would need to know the encoding and size in advance).

In a similar way, the function callcode can be used: The difference is that only the code of the given address is used, all other aspects (storage, balance, ...) are taken from the current contract. The purpose of callcode is to use library code which is stored in another contract. The user has to ensure that the layout of storage in both contracts is suitable for callcode to be used.

函数调用返回了一个布尔值，表示函数是否是正常调用结束(true)或引起了EVM异常(false)。不可能访问返回实际数据(这个我们需要提前知道编码和大小)。

同样,可以使用函数callcode:不同之处在于,只使用给定地址的编码,所有其他方面(存储、余额…)取自于当前的合约。callcode的目的是使用库代码存储在另一个合同。用户必须确保存储在两个合约的布局适用于callcode。

Both call and callcode are very low-level functions and should only be used as a *last resort* as they break the type-safety of Solidity.

call和callcode是非常低级的函数，它可以作为打破Solidity的类型安全的最后手段。

- call and callcode

**Note**

All contracts inherit the members of address, so it is possible to query the balance of the current contract using this.balance.

请注意

所有合同继承成员地址,所以可以使用this.balance查询当前合约的余额。

Fixed-size byte arrays

bytes1, bytes2, bytes3, ..., bytes32. byte is an alias for bytes1.

Operators:

- Comparisons: <=, <, ==, !=, >=, > (evaluate to bool)


- Bit operators: &, |, ^ (bitwise exclusive or), ~ (bitwise negation)

固定大小的字节数组

bytes1, bytes2, bytes3, ..., bytes32. byte 都是 bytes1的别名.

操作符：

- 比较符 : <=, <, ==, !=, >=, > (布尔的评估）


- 位操作符;: &, |, ^ (按位置异或)，~（按位取反）

Dynamically-sized byte array

**bytes:**Dynamically-sized byte array, see [Arrays](https://solidity.readthedocs.org/en/latest/types.html#arrays). Not a value-type!**string:**Dynamically-sized UTF8-encoded string, see [Arrays](https://solidity.readthedocs.org/en/latest/types.html#arrays). Not a value-type!

As a rule of thumb, use bytes for arbitrary-length raw byte data and string for arbitrary-length string (utf-8) data. If you can limit the length to a certain number of bytes, always use one of bytes1 tobytes32 because they are much cheaper.

动态分配大小字节数组:

bytes:动态分配大小字节数组,参看 Arrays,不是一个值类型!

string:动态大小UTF8编码的字符串,参看Arrays。不是一个值类型!

      作为一个常识, 使用bytes来表示任意长度原始字节数据，使用string来表示特定长度字符串数据(utf - 8编码)。如果你想限定特定数量的字节长度, 就使用bytes1 到 bytes32，  因为这样占用的存储空间更少。

Integer Literals

Integer Literals are arbitrary precision integers until they are used together with a non-literal. In var x = 1 - 2;, for example, the value of 1 - 2 is -1, which is assigned to x and thus x receives the type int8 – the smallest type that contains -1, although the natural types of 1 and 2 are actually uint8.

It is even possible to temporarily exceed the maximum of 256 bits as long as only integer literals are used for the computation: var x = (0xffffffffffffffffffff * 0xffffffffffffffffffff) * 0; Here, x will have the value0 and thus the type uint8.

整型常量

        整型常量是特定精度整数，它们也可以和非常量同时使用。例如, var x = 1 - 2;  1 - 2的值是-1,然后赋值给x, 这时x接收类型为int8——最小的类型,其中包含-1, 虽然1和2的类型实际上是uint8。

        有时最大超过256位的整型常量也可用于计算:var x =(0 xffffffffffffffffffff * 0 xffffffffffffffffffff)* 0;这里,x的值是0，它的类型是uint8类型。

String Literals

String Literals are written with double quotes (“abc”). As with integer literals, their type can vary, but they are implicitly convertible to bytes• if they fit, to bytes and to string.

字符串常量

字符串常量用两个双引号括起来(“abc”)。和整型常量相比,字符串常量有些不同,字符串常量可以隐式转换成bytes •    如果合适,可以是bytes,也可以是string。

\-

Enums

Enums are one way to create a user-defined type in Solidity. They are explicitly convertible to and from all integer types but implicit conversion is not allowed.

**contract** test {

    **enum** ActionChoices { GoLeft, GoRight, GoStraight, SitStill }

    ActionChoices choice;

    ActionChoices **constant** defaultChoice **=** ActionChoices.GoStraight;

    **function** setGoStraight()

    {

        choice **=** ActionChoices.GoStraight;

    }

枚举是一种Solidity中的创建一个用户定义的类型。枚举类型中的枚举值可显式转换，但从整数类型隐式转换是不允许的**。**

**contract** test {

    **enum** ActionChoices { GoLeft, GoRight, GoStraight, SitStill }

    ActionChoices choice;

    ActionChoices **constant** defaultChoice **=** ActionChoices.GoStraight;

    **function** setGoStraight()

    {

        choice **=** ActionChoices.GoStraight;

    }

    *// Since enum types are not part of the ABI, the signature of "getChoice"*

    *// will automatically be changed to "getChoice() returns (uint8)"*

    *// for all matters external to Solidity. The integer type used is just*

    *// large enough to hold all enum values, i.e. if you have more values,*

    *// `uint16` will be used and so on.*

/ /因为枚举类型不是ABI的一部分,“getChoice”的符号

/ /将自动改为“getChoice()返回(uint8)”

/ /从Solidity外部看，使用的整数类型

/ /足够容纳所有枚举值,但如果你有更多的值,

/ /“uint16”将使用。

    **function** getChoice() **returns** (ActionChoices)

    {

        **return** choice;

    }

    **function** getDefaultChoice() **returns** (**uint**)

    {

        **return** **uint**(defaultChoice);

    }}

Reference Types

Complex types, i.e. types which do not always fit into 256 bits have to be handled more carefully than the value-types we have already seen. Since copying them can be quite expensive, we have to think about whether we want them to be stored in **memory** (which is not persisting) or **storage** (where the state variables are held).

引用类型

复杂类型,例如类型并不总是适合256位，比我们已经看到的值类型更复杂的类型，必须更仔细地处理。因为复制拷贝他们可能相当耗费存储和时间，  我们必须考虑把它们存储在内存(这不是持久化)或者存储器(状态变量存放的地方)。

Data location

Every complex type, i.e. *arrays* and *structs*, has an additional annotation, the “data location”, about whether it is stored in memory or in storage. Depending on the context, there is always a default, but it can be overridden by appending either storage or memory to the type. The default for function parameters (including return parameters) is memory, the default for local variables is storage and the location is forced to storage for state variables (obviously).

There is also a third data location, “calldata”, which is a non-modifyable non-persistent area where function arguments are stored. Function parameters (not return parameters) of external functions are forced to “calldata” and it behaves mostly like memory.

Data locations are important because they change how assignments behave: Assignments between storage and memory and also to a state variable (even from other state variables) always create an independent copy. Assignments to local storage variables only assign a reference though, and this reference always points to the state variable even if the latter is changed in the meantime. On the other hand, assignments from a memory stored reference type to another memory-stored reference type does not create a copy.

数据位置

每一个复杂类型,即数组和结构体,有一个额外的注解,“数据位置”,不管它是存储在内存中，还是存储在存储器上。根据上下文,总是有一个默认的,但它可以通过附加存储或内存覆盖类型。函数参数的默认值(包括返回参数)是在内存上,局部变量的默认存储位置是在存储器上。存储器上存有状态变量(很明显)。

（除了内存，存储器这两个位置之外），还有第三个数据位置,“calldata”,这是一个 无法改变的，非持久的 存储函数参数的地方。外部函数的函数参数(不返回参数)“calldata”，其在形式上象内存。

数据位置很重要,因为它们改变赋值方式:在存储和内存以及状态变量之间赋值(甚至从其他状态变量)总是创建一个独立的副本。赋值只分配一个本地存储变量引用,这总是指向状态变量的引用,后者同时改变。另一方面,从一个内存存储引用类型, 赋值到另一个内存存储引用类型，（这时）并不创建一个副本。

**contract** c {

  **uint**[] x; *// the data location of x is storage*

  *// the data location of memoryArray is memory*

  **function** f(**uint**[] memoryArray) {

    x **=** memoryArray; *// works, copies the whole array to storage*

    **var** y **=** x; *// works, assigns a pointer, data location of y is storage*

    y[7]; *// fine, returns the 8th element*

    y.length **=** 2; *// fine, modifies x through y*

    **delete** x; *// fine, clears the array, also modifies y*

    *// The following does not work; it would need to create a new temporary /*

    *// unnamed array in storage, but storage is "statically" allocated:*

    *// y = memoryArray;*

    *// This does not work either, since it would "reset" the pointer, but there*

    *// is no sensible location it could point to.*

    *// delete y;*

    g(x); *// calls g, handing over a reference to x*

    h(x); *// calls h and creates an independent, temporary copy in memory*

  }

  **function** g(**uint**[] **storage** storageArray) **internal** {}

  **function** h(**uint**[] memoryArray) {}}

**contract** c {

  **uint**[] x; *// the data location of x is storage    **x的数据位置是存储器*

  *// the data location of memoryArray is memory  **memoryArray的数据位置是内存*

  **function** f(**uint**[] memoryArray) {

    x **=** memoryArray; *// works, copies the whole array to storage ** 运行，拷贝整个数组到存储器*

    **var** y **=** x; *// works, assigns a pointer, data location of y is storage **运行，赋值到一个指针，y的数据位置是存储器*

    y[7]; *// fine, returns the 8th element **好了，返回第8个元素*

    y.length **=** 2; *// fine, modifies x through y    **好了，通过y改变x*

    **delete** x; *// fine, clears the array, also modifies y  **好了，清除数组，也改变y*

    *// The following does not work; it would need to create a new temporary / ** 以下代码不起作用， 它是在存储中创立一个临时的未命名的数组，但存储器是“静态”分配的*

    *// unnamed array in storage, but storage is "statically" allocated:*

    *// y = memoryArray;*

    *// This does not work either, since it would "reset" the pointer, but there **这个也不起作用，因为 它重置了指针， 但已经没有相应的位置可以指向*

    *// is no sensible location it could point to.*

    *// delete y;*

    g(x); *// calls g, handing over a reference to x   **调用g(x)  将x作为引用*

    h(x); *// calls h and creates an independent, temporary copy in memory **调用h(x). 在内存中创立了一个独立的，暂时的拷贝*

  }

  **function** g(**uint**[] **storage** storageArray) **internal** {}

  **function** h(**uint**[] memoryArray) {}}

Summary

**Forced data location:**

- parameters (not return) of external functions: calldata


- state variables: storage

**Default data location:**

- parameters (also return) of functions: memory


- all other local variables: storage

总结

强制数据位置:

- 外部函数的参数(不返回):calldata


- 状态变量:存储器

默认数据位置:

- 函数（有返回）的参数:内存


- 其他所有局部变量:存储器

Arrays

Arrays can have a compile-time fixed size or they can be dynamic. For storage arrays, the element type can be arbitrary (i.e. also other arrays, mappings or structs). For memory arrays, it cannot be a mapping and has to be an ABI type if it is an argument of a publicly-visible function.

An array of fixed size k and element type T is written as T[k], an array of dynamic size as T[]. As an example, an array of 5 dynamic arrays of uint is uint[][5] (note that the notation is reversed when compared to some other languages). To access the second uint in the third dynamic array, you use x[2][1] (indices are zero-based and access works in the opposite way of the declaration, i.e. x[2] shaves off one level in the type from the right).

Variables of type bytes and string are special arrays. A bytes is similar to byte[], but it is packed tightly in calldata. string is equal to bytes but does not allow length or index access (for now).

So bytes should always be preferred over byte[] because it is cheaper.

数组

      数组是可以在编译时固定大小的，也可以是动态的。对于存储器数组来说，成员类型可以是任意的(也可以是其他数组，映射或结构)。对于内存数组来说 ,成员类型不能是一个映射；如果是公开可见的函数参数，成员类型是必须是ABI类型的。

        固定大小k的数组和基本类型T，可以写成T[k],  动态数组写成 T[ ] 。例如, 有5个基本类型为uint 的动态数组的数组 可以写成uint[ ][5]  ( 注意,和一些其他语言相比，这里的符号表示次序是反过来的)。为了访问第三动态数组中的第二个uint, 必须使用x[2][1](下标是从零开始的，访问模式和声明模式正好相反, 即x[2]是从右边剔除了一阶)。

*        bytes*和 string 是特殊类型的数组。 bytes 类似于byte[ ],但它是紧凑排列在calldata里的。string 等于 bytes ， 但不允许用长度或所以索引访问(现在情况是这样的)。

所以bytes应该优先于byte[ ],因为它效率更高。

**Note**

If you want to access the byte-representation of a string s, use bytes(s).length / bytes(s)[7] = ‘x’;. Keep in mind that you are accessing the low-level bytes of the utf-8 representation, and not the individual characters!

Members

**length:**Arrays have a length member to hold their number of elements. Dynamic arrays can be resized in storage (not in memory) by changing the .length member. This does not happen automatically when attempting to access elements outside the current length. The size of memory arrays is fixed (but dynamic, i.e. it can depend on runtime parameters) once they are created.

**push:**Dynamic storage arrays and bytes (not string) have a member function called push that can be used to append an element at the end of the array. The function returns the new length.

请注意

如果你想访问字符串s的某个字节,  要使用 bytes(s).length/bytes(s)[7]= ' x ';。记住,你正在访问的低级utf - 8字节表示,而不是单个字符!

成员（函数）：

**length**: 总有 一个称作length的成员（函数）来存放元素的数量。动态数组可以通过改变.length成员（函数），在存储器里来调整大小（不是在内存中）。当试图访问现有长度之外的成员时，这并不是自动被许可的。（数组）一旦创建，内存里的数组大小是固定的(如果是动态的数组,则取决于运行时参数)。

**push** :动态存储数组arrays和字节bytes(不是字符串string)有一个成员函数称作push,可在数组的尾部添加一个元素。函数返回新的长度。

**Warning**

It is not yet possible to use arrays of arrays in external functions.

**Warning**

Due to limitations of the EVM, it is not possible to return dynamic content from external function calls. The function f in contract C { function f() returns (uint[]) { ... } } will return something if called from web3.js, but not if called from Solidity.

The only workaround for now is to use large statically-sized arrays.

警告

到目前为止，还不可以在外部函数中使用数组的数组。

警告

由于EVM的局限,不可能从外部函数调用返回的动态内容。合约函数f contract C { function f() returns (uint[]) { ... } }  使用web3.js调用,将有返回值,  但使用Solidity调用，就没有返回值。

现在唯一的解决方法是使用较大的静态尺寸大小的数组。

**contract** ArrayContract {

  **uint**[2***\***20] m_aLotOfIntegers;

  *// Note that the following is not a pair of arrays but an array of pairs.*

  **bool**[2][] m_pairsOfFlags;

  *// newPairs is stored in memory - the default for function arguments*

  **function** setAllFlagPairs(**bool**[2][] newPairs) {

    *// assignment to a storage array replaces the complete array*

    m_pairsOfFlags **=** newPairs;

  }

  **function** setFlagPair(**uint** index, **bool** flagA, **bool** flagB) {

    *// access to a non-existing index will throw an exception*

    m_pairsOfFlags[index][0] **=** flagA;

    m_pairsOfFlags[index][1] **=** flagB;

  }

  **function** changeFlagArraySize(**uint** newSize) {

    *// if the new size is smaller, removed array elements will be cleared*

    m_pairsOfFlags.length **=** newSize;

  }

  **function** clear() {

    *// these clear the arrays completely*

    **delete** m_pairsOfFlags;

    **delete** m_aLotOfIntegers;

    *// identical effect here*

    m_pairsOfFlags.length **=** 0;

  }

  **bytes** m_byteData;

  **function** byteArrays(**bytes** data) {

    *// byte arrays ("bytes") are different as they are stored without padding,*

    *// but can be treated identical to "uint8[]"*

    m_byteData **=** data;

    m_byteData.length **+=** 7;

    m_byteData[3] **=** 8;

    **delete** m_byteData[2];

  }

  **function** addFlag(**bool**[2] flag) **returns** (**uint**) {

    **return** m_pairsOfFlags.push(flag);

  }

  **function** createMemoryArray(**uint** size) **returns** (**bytes**) {

    *// Dynamic memory arrays are created using `new`:*

    **uint**[2][] **memory** arrayOfPairs **=** **new** **uint**[2][](size);

    *// Create a dynamic byte array:*

    **bytes** **memory** b **=** **new** **bytes**(200);

    **for** (**uint** i **=** 0; i **<** b.length; i**++**)

      b[i] **=** **byte**(i);

    **return** b;

  }}

**contract** ArrayContract {

  **uint**[2***\***20] m_aLotOfIntegers;

  *// Note that the following is not a pair of arrays but an array of pairs.** **注意下面不是两个数组，而是一个数组，该数组的成员是一对值*

  **bool**[2][] m_pairsOfFlags;

  *// newPairs is stored in memory - the default for function arguments ** **newPairs在内存中存储-这是函数参数的缺省方式*

  **function** setAllFlagPairs(**bool**[2][] newPairs) {

    *// assignment to a storage array replaces the complete array **赋值到一个存储器数组里以替换整个数组*

    m_pairsOfFlags **=** newPairs;

  }

  **function** setFlagPair(**uint** index, **bool** flagA, **bool** flagB) {

    *// access to a non-existing index will throw an exception*

    m_pairsOfFlags[index][0] **=** flagA;

    m_pairsOfFlags[index][1] **=** flagB;

  }

  **function** changeFlagArraySize(**uint** newSize) {

    *// if the new size is smaller, removed array elements will be cleared  **如果新的尺寸太小，则已经移除的元素将被清除*

    m_pairsOfFlags.length **=** newSize;

  }

  **function** clear() {

    *// these clear the arrays completely*

    **delete** m_pairsOfFlags;

    **delete** m_aLotOfIntegers;

    *// identical effect here*

    m_pairsOfFlags.length **=** 0;

  }

  **bytes** m_byteData;

  **function** byteArrays(**bytes** data) {

    *// byte arrays ("bytes") are different as they are stored without padding,  **如果没有填充的话，字节数组（"bytes"）和存储时是不同的*

    *// but can be treated identical to "uint8[]"  **但可以转换成 "uint8[]"*

    m_byteData **=** data;

    m_byteData.length **+=** 7;

    m_byteData[3] **=** 8;

    **delete** m_byteData[2];

  }

  **function** addFlag(**bool**[2] flag) **returns** (**uint**) {

    **return** m_pairsOfFlags.push(flag);

  }

  **function** createMemoryArray(**uint** size) **returns** (**bytes**) {

    *// Dynamic memory arrays are created using `new`:*** ***使用`new`创立动态内存数组*

    **uint**[2][] **memory** arrayOfPairs **=** **new** **uint**[2][](size);

    *// Create a dynamic byte array:  **创立动态 byte 数组*

    **bytes** **memory** b **=** **new** **bytes**(200);

    **for** (**uint** i **=** 0; i **<** b.length; i**++**)

      b[i] **=** **byte**(i);

    **return** b;

  }}

Structs

Solidity provides a way to define new types in the form of structs, which is shown in the following example:

结构体

Solidity 提供了一种方法来定义新类型的形式结构,如下面的例子所示:

**contract** CrowdFunding {

  *// Defines a new type with two fields.*

  **struct** Funder {

    **address** addr;

    **uint** amount;

  }

  **struct** Campaign {

    **address** beneficiary;

    **uint** fundingGoal;

    **uint** numFunders;

    **uint** amount;

    **mapping** (**uint** **=>** Funder) funders;

  }

  **uint** numCampaigns;

  **mapping** (**uint** **=>** Campaign) campaigns;

  **function** newCampaign(**address** beneficiary, **uint** goal) **returns** (**uint** campaignID) {

    campaignID **=** numCampaigns**++**; *// campaignID is return variable*

    *// Creates new struct and saves in storage. We leave out the mapping type.*

    campaigns[campaignID] **=** Campaign(beneficiary, goal, 0, 0);

  }

  **function** contribute(**uint** campaignID) {

    Campaign c **=** campaigns[campaignID];

        *// Creates a new temporary memory struct, initialised with the given values*

        *// and copies it over to storage.*

        *// Note that you can also use Funder(msg.sender, msg.value) to initialise.*

    c.funders[c.numFunders**++**] **=** Funder({addr**:** msg.sender, amount**:** msg.value});

    c.amount **+=** msg.value;

  }

  **function** checkGoalReached(**uint** campaignID) **returns** (**bool** reached) {

    Campaign c **=** campaigns[campaignID];

    **if** (c.amount **<** c.fundingGoal)

      **return** **false**;

    c.beneficiary.send(c.amount);

    c.amount **=** 0;

    **return** **true**;

  }}

**contract** CrowdFunding {

  *// Defines a new type with two fields. **定义了两个域的新类型*

  **struct** Funder {

    **address** addr;

    **uint** amount;

  }

  **struct** Campaign {

    **address** beneficiary;

    **uint** fundingGoal;

    **uint** numFunders;

    **uint** amount;

    **mapping** (**uint** **=>** Funder) funders;

  }

  **uint** numCampaigns;

  **mapping** (**uint** **=>** Campaign) campaigns;

  **function** newCampaign(**address** beneficiary, **uint** goal) **returns** (**uint** campaignID) {

    campaignID **=** numCampaigns**++**; *// campaignID is return variable ** campaignID是返回的变量*

    *// Creates new struct and saves in storage. We leave out the mapping type.** 创建一个新的结构体，保存在存储器里， 保留了映射类型*

    campaigns[campaignID] **=** Campaign(beneficiary, goal, 0, 0);

  }

  **function** contribute(**uint** campaignID) {

    Campaign c **=** campaigns[campaignID];

        *// Creates a new temporary memory struct, initialised with the given values **创建了一个新的临时内存结构体，用给定的值进行初始化*

        *// and copies it over to storage. **拷贝到存储器上*

        *// Note that you can also use Funder(msg.sender, msg.value) to initialise.** 注意你可以使用 Funder(msg.sender, msg.value)来初始化*

    c.funders[c.numFunders**++**] **=** Funder({addr**:** msg.sender, amount**:** msg.value});

    c.amount **+=** msg.value;

  }

  **function** checkGoalReached(**uint** campaignID) **returns** (**bool** reached) {

    Campaign c **=** campaigns[campaignID];

    **if** (c.amount **<** c.fundingGoal)

      **return** **false**;

    c.beneficiary.send(c.amount);

    c.amount **=** 0;

    **return** **true**;

  }}

The contract does not provide the full functionality of a crowdfunding contract, but it contains the basic concepts necessary to understand structs. Struct types can be used inside mappings and arrays and they can itself contain mappings and arrays.

It is not possible for a struct to contain a member of its own type, although the struct itself can be the value type of a mapping member. This restriction is necessary, as the size of the struct has to be finite.

Note how in all the functions, a struct type is assigned to a local variable (of the default storage data location). This does not copy the struct but only stores a reference so that assignments to members of the local variable actually write to the state.

Of course, you can also directly access the members of the struct without assigning it to a local variable, as in campaigns[campaignID].amount = 0.

       此（例子）合约没有提供众筹合约的完整功能,  但它包含了必要的基本概念，以便（让我们更好地）理解结构体。结构体类型可以是内部映射或者是数组，他们本身也可以包含映射和数组。

       通常这是不可能,即一个结构体包含一个自身类型的成员, 虽然结构体本身可以是一个映射的值类型成员。这个限制是必要的, 原因是结构体的大小是有限的。

       注意所有的函数中, 结构类型是赋值给一个局部变量(默认存储数据的位置)。这并不复制结构体，仅仅保存了一个引用， 本地变量的赋值最终还是以写进了状态中。

         当然,您也可以直接访问结构体的成员变量，而不用赋值到一个局部变量,如campaigns[campaignID].amount = 0.

Mappings

Mapping types are declared as mapping _KeyType => _ValueType, where _KeyType can be almost any type except for a mapping and _ValueType can actually be any type, including mappings.

Mappings can be seen as hashtables which are virtually initialized such that every possible key exists and is mapped to a value whose byte-representation is all zeros. The similarity ends here, though: The key data is not actually stored in a mapping, only its sha3 hash used to look up the value.

Because of this, mappings do not have a length or a concept of a key or value being “set”.

Mappings are only allowed for state variables (or as storage reference types in internal functions).

映射

       映射类型被声明为 mapping _KeyType => _ValueType,  _KeyType可以是除了映射以外的其他任何类型，_ValueType可以是任何类型,包括映射。

       映射可以被视为初始化的散列表,这样每一个键值都存在, 这些键值在字节表示上是全零。相似性到此为止,尽管:key数据实际上并不是存储在一个映射中,它只有在使用sha3哈希查找值使用。

       因此,映射没有长度，也没有一个键或值的被“set”的概念。

映射是只允许为状态变量(在内部函数中作为一个存储引用类型)。

Operators Involving LValues

If a is an LValue (i.e. a variable or something that can be assigned to), the following operators are available as shorthands:

a += e is equivalent to a = a + e. The operators -=, *=, /=, %=, a |=, &= and ^= are defined accordingly.a++ and a– are equivalent to a += 1 / a -= 1 but the expression itself still has the previous value of a. In contrast, --a and ++a have the same effect on a but return the value after the change.

包括左值操作的操作符

如果是一个左值操作(即一个可以赋值给它的变量),可以使用以下的操作符:

a += e相当于 a = a + e。操作符- = * =,/ = % = | = & = ^ = 都有相应的定义。a++和a--相当于a+ = 1 /a - = 1,但是表达式本身还有一个操作前的值。相比之下, --a和++a有相同的影响但返回值改变。

delete

delete a assigns the initial value for the type to a. I.e. for integers it is equivalent to a = 0, but it can also be used on arrays, where it assigns a dynamic array of length zero or a static array of the same length with all elements reset. For structs, it assigns a struct with all members reset.

delete has no effect on whole mappings (as the keys of mappings may be arbitrary and are generally unknown). So if you delete a struct, it will reset all members that are not mappings and also recurse into the members unless they are mappings. However, individual keys and what they map to can be deleted.

It is important to note that delete a really behaves like an assignment to a, i.e. it stores a new object in a.

删除

删除一个指定类型的初始值为整数,即相当于a= 0,但是它也可以用于数组,它分配一个动态数组的长度为零或一个静态数组长度相同的所有元素重置。对于结构体,它分配一个struct,重置所有成员。

删除没有影响整体映射(如映射的键可能是任意的,通常是未知的)。如果你删除一个结构,它将重置没有映射的所有成员，也可以是递归的成员,除非它们映射。然而,个别键和他们的映射是可以删除。

重要的是要注意,删除一个a的赋值,  即它存储在一个新的对象。

**contract** DeleteExample {

  **uint** data;

  **uint**[] dataArray;

  **function** f() {

    **uint** x **=** data;

    **delete** x; *// sets x to 0, does not affect data*

    **delete** data; *// sets data to 0, does not affect x which still holds a copy*

    **uint**[] y **=** dataArray;

    **delete** dataArray; *// this sets dataArray.length to zero, but as uint[] is a complex object, also*

    *// y is affected which is an alias to the storage object*

    *// On the other hand: "delete y" is not valid, as assignments to local variables*

    *// referencing storage objects can only be made from existing storage objects.*

  }}

**contract** DeleteExample {

  **uint** data;

  **uint**[] dataArray;

  **function** f() {

    **uint** x **=** data;

    **delete** x; *// sets x to 0, does not affect data  设置x为0, 不影响data*

    **delete** data; *// sets data to 0, does not affect x which still holds a copy  设置data为0,x不受影响，x仍然有一个拷贝*

    **uint**[] y **=** dataArray;

    **delete** dataArray; *// this sets dataArray.length to zero, but as uint[] is a complex object, also **dataArray.length长度是0。但是uint[ ]是一个复杂对象， y受影响，其是存储对象的别名*

    *// y is affected which is an alias to the storage object*

    *// On the other hand: "delete y" is not valid, as assignments to local variables **另外， "delete y"是非法的，因为y是赋值到本地变量*

    *// referencing storage objects can only be made from existing storage objects.**引用存储对象仅仅来自于现有的存储对象*

  }}

Conversions between Elementary Types

Implicit Conversions

If an operator is applied to different types, the compiler tries to implicitly convert one of the operands to the type of the other (the same is true for assignments). In general, an implicit conversion between value-types is possible if it makes sense semantically and no information is lost: uint8 is convertible to uint16 and int128 to int256, but int8 is not convertible to uint256 (because uint256 cannot hold e.g.-1). Furthermore, unsigned integers can be converted to bytes of the same or larger size, but not vice-versa. Any type that can be converted to uint160 can also be converted to address.

基本类型之间的转换

隐式转换

如果一个操作符应用于不同类型,  编译器(就会)试图隐式把操作数的类型，从一种类型转换到其他类型(赋值也是如此)。一般来说,一个隐式的值类型之间的转换是可能的,如果是语义敏感的话，信息不会丢失:unt8可转换成uint16， int128， int256,  但int8不能转换成uint256(因为uint256放不下  如 -1)。此外,无符号整数可以转换成相同或更大的尺寸的bytes ,  但反过来，不行 。任何类型都可以转化为uint160，也可以转换为地址。

Explicit Conversions

If the compiler does not allow implicit conversion but you know what you are doing, an explicit type conversion is sometimes possible:

**int8** y **=** **-**3;

**uint** x **=** **uint**(y);

At the end of this code snippet, x will have the value 0xfffff..fd (64 hex characters), which is -3 in two’s complement representation of 256 bits.

If a type is explicitly converted to a smaller type, higher-order bits are cut off:

**uint32** a **=** 0x12345678;

**uint16** b **=** **uint16**(a); *// b will be 0x5678 now*

显式转换

如果编译器不允许隐式转换，但你知道你在做什么,一个显式的类型转换有时是可能的:

int8 y = 3;

uint x =uint(y);

   

   这个代码片段结尾 ， x的值是 0xfffff . .fd(64个十六进制字符),-3在256位的二进制补码表示。

   如果一个类型是显式地转换为一个更小的类型,高阶位将被移除。

uint32 = 0x12345678;

uint16 b = uint16(a);/ / *b will be 0x5678 now*  *b现在变成了0x5678，（少了1234）*

Type Deduction

For convenience, it is not always necessary to explicitly specify the type of a variable, the compiler automatically infers it from the type of the first expression that is assigned to the variable:

uint20 x **=** 0x123;

**var** y **=** x;

Here, the type of y will be uint20. Using var is not possible for function parameters or return parameters.

**Warning**

The type is only deduced from the first assignment, so the loop in the following snippet is infinite, as i will have the type uint8 and any value of this type is smaller than 2000. for (var i = 0; i < 2000; i++) { ... }

类型推导

为方便起见,它并不总是必须显式地指定一个变量的类型,编译器会自动从第一个赋值表达式的变量类型里推断出新变量的类型:

uint20 x = 0 x123;

var y = x;

在这里,y的类型是uint20。在函数参数或返回参数是不可能使用var（这个关键字）的。

警告

这个类型仅仅是从第一次赋值推导得出的,所以以下代码片段的循环是无限的,  因为 i 的类型是uint8， 这种类型的任何值都小于2000。for (var i = 0;< 2000;i+ +){…}

[Next ](https://solidity.readthedocs.org/en/latest/units-and-global-variables.html) Previous

© Copyright 2015, Ethereum. Revision 9b9d10b4.

Built with [Sphinx](http://sphinx-doc.org/) using a [theme](https://github.com/snide/sphinx_rtd_theme) provided by [Read the Docs](https://readthedocs.org/).

 Read the Docsv: latest 
