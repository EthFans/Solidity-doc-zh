原文链接

[https://solidity.readthedocs.org/en/latest/units-and-global-variables.html#units-and-globally-available-variables](https://solidity.readthedocs.org/en/latest/units-and-global-variables.html#units-and-globally-available-variables)

Units and Globally Available Variables

**单位和全局可用变量**

Ether Units

A literal number can take a suffix of wei, finney, szabo or ether to convert between the subdenominations of Ether, where Ether currency numbers without a postfix are assumed to be “wei”, e.g. 2 ether == 2000 finney evaluates to true.

以太单位

数词后面可以有一个后缀, wei, finney, szabo 或 ether 和 ether 相关量词 之间的转换,在以太币数量后若没有跟后缀，则缺省单位是“wei“,   如  2 ether  == 2000 finney   （这个表达式）计算结果为true。 

Time Units

Suffixes of seconds, minutes, hours, days, weeks and years after literal numbers can be used to convert between units of time where seconds are the base unit and units are considered naively in the following way:

- 1 == 1 second


- 1 minutes == 60 seconds


- 1 hours == 60 minutes


- 1 days == 24 hours


- 1 weeks = 7 days


- 1 years = 365 days

Take care if you perform calendar calculations using these units, because not every year equals 365 days and not even every day has 24 hours because of [leap seconds](https://en.wikipedia.org/wiki/Leap_second). Due to the fact that leap seconds cannot be predicted, an exact calendar library has to be updated by an external oracle.

These suffixes cannot be applied to variables. If you want to interpret some input variable in e.g. days, you can do it in the following way:

**function** f(**uint** start, **uint** daysAfter) {

  **if** (now **>=** start **+** daysAfter ***** 1 days) { ... }}

时间单位

后缀的秒,分,小时,天,周,年，  数量词的时间单位之间可以用来转换,秒是基本单位。下面是常识：

1 = 1秒              （原文使用了两个==，可能有误 --译者注）

1分钟 = 60秒     （原文使用了两个==，可能有误 --译者注）

1小时 = 60分钟 （原文使用了两个==，可能有误 --译者注）

1天=24小时       （原文使用了两个==，可能有误 --译者注）

1周= 7天

1年= 365天

如果你使用这些单位执行日历计算，要注意以下问题。   因为闰秒，所以每年不总是等于365天,甚至每天也不是都有24小时,。由于无法预测闰秒,一个精确的日历库必须由外部oracle更新。

这些后缀不能用于变量。如果你想解释一些输入变量, 如天,你可以用以下方式:

**function** f(**uint** start, **uint** daysAfter) {

  **if** (now **>=** start **+** daysAfter ***** 1 days) { ... }}

Special Variables and Functions

There are special variables and functions which always exist in the global namespace and are mainly used to provide information about the blockchain.

特殊的变量和函数

有特殊的变量和函数总是存在于全局命名空间,主要用于提供关于blockchain的信息。

Block and Transaction Properties

- block.coinbase (address): current block miner’s address


- block.difficulty (uint): current block difficulty


- block.gaslimit (uint): current block gaslimit


- block.number (uint): current block number


- block.blockhash (function(uint) returns (bytes32)): hash of the given block - only for 256 most recent blocks


- block.timestamp (uint): current block timestamp


- msg.data (bytes): complete calldata


- msg.gas (uint): remaining gas


- msg.sender (address): sender of the message (current call)


- msg.sig (bytes4): first four bytes of the calldata (i.e. function identifier)


- msg.value (uint): number of wei sent with the message


- now (uint): current block timestamp (alias for block.timestamp)


- tx.gasprice (uint): gas price of the transaction


- tx.origin (address): sender of the transaction (full call chain)

块和交易属性

- block.coinbase (address): :当前块的矿工的地址


- block.difficulty (uint):当前块的难度系数


- block.gaslimit (uint):当前块汽油限量


- block.number (uint):当前块编号


- block.blockhash (function(uint) returns (bytes32)):指定块的哈希值——最新的256个块的哈希值


- block.timestamp (uint):当前块的时间戳


- msg.data (bytes):完整的calldata


- msg.gas (uint):剩余的汽油


- msg.sender (address):消息的发送方(当前调用)


- msg.sig (bytes4):calldata的前四个字节(即函数标识符)


- msg.value (uint):所发送的消息中wei的数量


- now (uint):当前块时间戳(block.timestamp的别名)


- tx.gasprice (uint):交易的汽油价格


- tx.origin (address):交易发送方(完整的调用链)

**Note**

The values of all members of msg, including msg.sender and msg.value can change for every**external** function call. This includes calls to library functions.

If you want to implement access restrictions in library functions using msg.sender, you have to manually supply the value of msg.sender as an argument.

**Note**

The block hashes are not available for all blocks for scalability reasons. You can only access the hashes of the most recent 256 blocks, all other values will be zero.

请注意

msg的所有成员的值,包括msg.sender和msg.value可以在每个 external函数调用中改变。这包括调用库函数。

如果你想在库函数实现访问限制使用msg.sender, 你必须手动设置msg.sender作为参数。

请注意

由于所有块可伸缩性的原因，（所有）块的Hash值就拿不到。你只能访问最近的256块的Hash值,其他值为零。

Mathematical and Cryptographic Functions

数学和加密功能

**addmod(uint x, uint y, uint k) returns (uint)****:**

compute (x + y) % k where the addition is performed with arbitrary precision and does not wrap around at 2**256.

计算 (x + y) % k   （按指定的精度，不能超过2**256）

**mulmod(uint x, uint y, uint k) returns (uint):**

compute (x * y) % k where the multiplication is performed with arbitrary precision and does not wrap around at 2**256. （按指定的精度，不能超过2**256）

计算compute (x * y) % k 

**sha3(...) returns (bytes32):**

compute the Ethereum-SHA-3 hash of the (tightly packed) arguments

计算（紧凑排列的）参数的Ethereum-SHA-3 的Hash值值

**sha256(...) returns (bytes32):**

compute the SHA-256 hash of the (tightly packed) arguments

计算（紧凑排列的）参数的SHA-256 的Hash值

**ripemd160(...) returns (bytes20):**

compute RIPEMD-160 hash of the (tightly packed) arguments

计算（紧凑排列的）参数的 RIPEMD-160 的Hash值

**ecrecover(bytes32, byte, bytes32, bytes32) returns (address):**

recover public key from elliptic curve signature - arguments are (data, v, r, s)

恢复椭圆曲线特征的公钥-参数为(data, v, r, s)

In the above, “tightly packed” means that the arguments are concatenated without padding. This means that the following are all identical:

在上述中，“紧凑排列”，意思是没有填充的参数的连续排列，也就是下面表达式是没有区别的

sha3("ab", "c")

sha3("abc")

sha3(0x616263)

sha3(6382179)

sha3(97, 98, 99)

If padding is needed, explicit type conversions can be used: sha3(“x00x12”) is the same as sha3(uint16(0x12)).

如果需要填充，要用显示的形式表示： sha3(“x00x12”) 和 sha3(uint16(0x12))是相同的。

It might be that you run into Out-of-Gas for sha256, ripemd160 or ecrecover on a *private blockchain*. The reason for this is that those are implemented as so-called precompiled contracts and these contracts only really exist after they received the first message (although their contract code is hardcoded). Messages to non-existing contracts are more expensive and thus the execution runs into an Out-of-Gas error. A workaround for this problem is to first send e.g. 1 Wei to each of the contracts before you use them in your actual contracts. This is not an issue on the official or test net.

在一个私有的blockchain里，你可能（在使用）sha256, ripemd160 或 ecrecover (的时候) 碰到"Out-of-Gas"（的问题）  。原因在于这个仅仅是预编译的合约，合约要在他们接到的第一个消息以后才真正的生成（虽然他们的合约代码是硬编码的）。对于没有真正生成的合约的消息是非常昂贵的，这时就会碰到“Out-of-Gas”的问题。 这一问题的解决方法是事先把1wei 发送到各个你当前使用的各个合约上。这不是官方或测试网的问题。

Contract Related

**this (current contract’s type):**

the current contract, explicitly convertible to address

**selfdestruct(address):**destroy the current contract, sending its funds to the given address

Furthermore, all functions of the current contract are callable directly including the current function.

合约相关的

**this (current contract’s type)**:

当前的合约,显示可转换地址

**selfdestruct(address):**:

销毁当前合约,其资金发送给指定的地址

此外,当前合同的所有函数均可以被直接调用（包括当前函数）。

© Copyright 2015, Ethereum. Revision 37381072.

Built with [Sphinx](http://sphinx-doc.org/) using a [theme](https://github.com/snide/sphinx_rtd_theme) provided by [Read the Docs](https://readthedocs.org/).
