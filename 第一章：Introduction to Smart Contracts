原文：[https://solidity.readthedocs.org/en/latest/introduction-to-smart-contracts.html](https://solidity.readthedocs.org/en/latest/introduction-to-smart-contracts.html)

Introduction to Smart Contracts

A Simple Smart Contract

一个简单的智能合约

先从一个非常基础的例子开始，不用担心你现在还一点都不了解，我们将逐步了解到更多的细节。

Let us begin with the most basic example. It is fine if you do not understand everything right now, we will go into more detail later.

Storage

**contract** SimpleStorage {

    **uint** storedData;

    **function** set(**uint** x) {

        storedData **=** x;

    }

    **function** get() **constant** **returns** (**uint** retVal) {

        **return** storedData;

    }}

A contract in the sense of Solidity is a collection of code (its functions) and data (its *state*) that resides at a specific address on the Ethereum blockchain. The line uint storedData; declares a state variable called storedData of type uint (unsigned integer of 256 bits). You can think of it as a single slot in a database that can be queried and altered by calling functions of the code that manages the database. In the case of Ethereum, this is always the owning contract. And in this case, the functionsset and get can be used to modify or retrieve the value of the variable.

在Solidity中，一个合约由一组代码（合约的函数）和数据（合约的状态）组成。合约位于以太坊区块链上的一个特殊地址。*uint storedData**;* 这行代码声明了一个状态变量，变量名为*storedData*，类型为 *uint* （256bits无符号整数）。你可以认为它就像数据库里面的一个存储单元，跟管理数据库一样，可以通过调用函数查询和修改它。在以太坊中，通常只有合约的拥有者才能这样做。在这个例子中，函数 *set* 和 *get* 分别用于修改和查询变量的值。

To access a state variable, you do not need the prefix this. as is common in other languages.

跟很多其他语言一样，访问状态变量时，不需要在前面增加 this. 这样的前缀。

This contract does not yet do much apart from (due to the infrastructure built by Ethereum) allowing anyone to store a single number that is accessible by anyone in the world without (feasible) a way to prevent you from publishing this number. Of course, anyone could just call set again with a different value and overwrite your number, but the number will still be stored in the history of the blockchain. Later, we will see how you can impose access restrictions so that only you can alter the number.

这个合约还无法做很多事情（受限于以太坊的基础设施），仅仅是允许任何人储存一个数字。而且世界上任何一个人都可以来存取这个数字，缺少一个（可靠的）方式来保护你发布的数字。任何人都可以调用set方法设置一个不同的数字覆盖你发布的数字。但是你的数字将会留存在区块链的历史上。稍后我们会学习如何增加一个存取限制，使得只有你才能修改这个数字。

Subcurrency Example

代币的例子

The following contract will implement the simplest form of a cryptocurrency. It is possible to generate coins out of thin air, but only the person that created the contract will be able to do that (it is trivial to implement a different issuance scheme). Furthermore, anyone can send coins to each other without any need for registering with username and password - all you need is an Ethereum keypair.

接下来的合约将实现一个形式最简单的加密货币。空中取币不再是一个魔术，当然只有创建合约的人才能做这件事情（想用其他货币发行模式也很简单，只是实现细节上的差异）。而且任何人都可以发送货币给其他人，不需要注册用户名和密码，只要有一对以太坊的公私钥即可。

**Note**

This is not a nice example for browser-solidity. If you use [browser-solidity](https://chriseth.github.io/browser-solidity) to try this example, you cannot change the address where you call functions from. So you will always be the “minter”, you can mint coins and send them somewhere, but you cannot impersonate someone else. This might change in the future.

对于在线solidity环境来说，这不是一个好的例子。如果你使用[在线solidity环境](http://chriseth.github.io/browser-solidity/) 来尝试这个例子。调用函数时，将无法改变from的地址。所以你只能扮演铸币者的角色，可以铸造货币并发送给其他人，而无法扮演其他人的角色。这点在线solidity环境将来会做改进。

**contract** Coin {

    *// The keyword "public" makes those variables*

    *// readable from outside.*

//关键字“public”使变量能从合约外部访问。

    **address** **public** minter;

    **mapping** (**address** **=>** **uint**) **public** balances;

    *// Events allow light clients to react on*

    *// changes efficiently.*

//事件让轻客户端能高效的对变化做出反应。

    **event** Sent(**address** from, **address** to, **uint** amount);

    *// This is the constructor whose code is*

    *// run only when the contract is created.*

//这个构造函数的代码仅仅只在合约创建的时候被运行。

    **function** Coin() {

        minter **=** msg.sender;

    }

    **function** mint(**address** receiver, **uint** amount) {

        **if** (msg.sender **!=** minter) **return**;

        balances[receiver] **+=** amount;

    }

    **function** send(**address** receiver, **uint** amount) {

        **if** (balances[msg.sender] **<** amount) **return**;

        balances[msg.sender] **-=** amount;

        balances[receiver] **+=** amount;

        Sent(msg.sender, receiver, amount);

    }}

This contract introduces some new concepts, let us go through them one by one.

这个合约引入了一些新的概念，让我们一个一个来看一下。

The line address public minter; declares a state variable of type address that is publicly accessible. The address type is a 160 bit value that does not allow any arithmetic operations. It is suitable for storing addresses of contracts or keypairs belonging to external persons. The keyword publicautomatically generates a function that allows you to access the current value of the state variable. Without this keyword, other contracts have no way to access the variable and only the code of this contract can write to it. The function will look something like this:

address public minter;  这行代码声明了一个可公开访问的状态变量，类型为address。address类型的值大小为160 bits，不支持任何算术操作。适用于存储合约的地址或其他人的公私钥。public关键字会自动为其修饰的状态变量生成访问函数。没有public关键字的变量将无法被其他合约访问。另外只有本合约内的代码才能写入。自动生成的函数如下：

**function** minter() **returns** (**address**) { **return** minter; }

Of course, adding a function exactly like that will not work because we would have a function and a state variable with the same name, but hopefully, you get the idea - the compiler figures that out for you.

当然我们自己增加一个这样的访问函数是行不通的。编译器会报错，指出这个函数与一个状态变量重名。

The next line, mapping (address => uint) public balances; also creates a public state variable, but it of a more complex datatype. The type maps addresses to unsigned integers. Mappings can be seen as hashtables which are virtually initialized such that every possible key exists and is mapped to a value whose byte-representation is all zeros. This analogy does not go too far, though, as it is neither possible to obtain a list of all keys of a mapping, nor a list of all values. So either keep in mind (or better, keep a list or use a more advanced data type) what you added to the mapping or use it in a context where this is not needed, like this one. The accessor function created by the public keyword is a bit more complex in this case. It roughly looks like the following:

下一行代码 mapping (address => uint) public balances; 创建了一个public的状态变量，但是其类型更加的复杂。该类型将一些address映射到无符号整数。mapping可以被认为是一个哈希表，每一个可能的key对应的value被虚拟的初始化为全0.这个类比不是很严谨，对于一个mapping，无法获取一个包含其所有key或者value的链表。所以我们得自己记着添加了哪些东西到mapping中。更好的方式是维护一个这样的链表，或者使用其他更高级的数据类型。或者只在不受这个缺陷影响的场景中使用mapping，就像这个例子。在这个例子中由public关键字生成的访问函数将会更加复杂，其代码大致如下：

**function** balances(**address** _account) **returns** (**uint** balance) {

    **return** balances[_account];}

As you see, you can use this function to easily query the balance of a single account.

我们可以很方便的通过这个函数查询某个特定账号的余额。

The line event Sent(address from, address to, uint value); declares a so-called “event” which is fired in the last line of the function send. User interfaces (as well as server appliances of course) can listen for those events being fired on the blockchain without much cost. As soon as it is fired, the listener will also receive the arguments from, to and value, which makes it easy to track transactions. In order to listen for this event, you would use

event Sent(address from, address to, uint value); 这行代码声明了一个“事件”。由send函数的最后一行代码触发。客户端（服务端应用也适用）可以以很低的开销来监听这些由区块链触发的事件。事件触发时，监听者会同时接收到from，to，value这些参数值，可以方便的用于跟踪交易。为了监听这个事件，你可以使用如下代码：

Coin.Sent().watch({}, '', **function**(error, result) {

    **if** (**!**error) {

        console.log("Coin transfer: " **+** result.args.amount **+**

            " coins were sent from " **+** result.args.from **+**

            " to " **+** result.args.to **+** ".");

        console.log("Balances now:\n" **+**

            "Sender: " **+** Coin.balances.call(result.args.from) **+**

            "Receiver: " **+** Coin.balances.call(result.args.to));

    }}

Note how the automatically generated function balances is called from the user interface.

注意在客户端中是如何调用自动生成的 balances 函数的。

The special function Coin is the constructor which is run during creation of the contract and cannot be called afterwards. It permanently stores the address of the person creating the contract: msg(together with tx and block) is a magic global variable that contains some properties which allow access to the blockchain. msg.sender is always the address where the current (external) function call came from.

这里有个比较特殊的函数 Coin。它是一个构造函数，会在合约创建的时候运行，之后就无法被调用。它会永久得存储合约创建者的地址。msg（以及tx和block）是一个神奇的全局变量，它包含了一些可以被合约代码访问的属于区块链的属性。msg.sender 总是存放着当前函数的外部调用者的地址。

Finally, the functions that will actually end up with the contract and can be called by users and contracts alike are mint and send. If mint is called by anyone except the account that created the contract, nothing will happen. On the other hand, send can be used by anyone (who already has some of these coins) to send coins to anyone else. Note that if you use this contract to send coins to an address, you will not see anything when you look at that address on a blockchain explorer, because the fact that you sent coins and the changed balances are only stored in the data storage of this particular coin contract. By the use of events it is relatively easy to create a “blockchain explorer” that tracks transactions and balances of your new coin.

最后，真正被用户或者其他合约调用，用来完成本合约功能的函数是mint和send。如果合约创建者之外的其他人调用mint，什么都不会发生。而send可以被任何人（拥有一定数量的代币）调用，发送一些币给其他人。注意，当你通过该合约发送一些代币到某个地址，在区块链浏览器中查询该地址将什么也看不到。因为发送代币导致的余额变化只存储在该代币合约的数据存储中。通过事件我们可以很容易创建一个可以追踪你的新币交易和余额的“区块链浏览器”。

Blockchain Basics

区块链基础

Blockchains as a concept are not too hard to understand for programmers. The reason is that most of the complications (mining, hashing, elliptic-curve cryptography, peer-to-peer networks, ...) are just there to provide a certain set of features and promises. Once you accept these features as given, you do not have to worry about the underlying technology - or do you have to know how Amazon’s AWS works internally in order to use it?

对于程序员来说，区块链这个概念其实不难理解。因为最难懂的一些东西（挖矿，哈希，椭圆曲线加密，点对点网络等等）只是为了提供一系列的特性和保障。你只需要接受这些既有的特性，不需要关心其底层的技术。就像你如果仅仅是为了使用亚马逊的AWS，并不需要了解其内部工作原理。

Transactions

交易/事务

A blockchain is a globally shared, transactional database. This means that everyone can read entries in the database just by participating in the network. If you want to change something in the database, you have to create a so-called transaction which has to be accepted by all others. The word transaction implies that the change you want to make (assume you want to change two values at the same time) is either not done at all or completely applied. 

区块链是一个全局共享的，事务性的数据库。这意味着参与这个网络的每一个人都可以读取其中的记录。如果你想修改这个数据库中的东西，就必须创建一个事务，并得到其他所有人的确认。事务这个词意味着你要做的修改（假如你想同时修改两个值）只能被完完全全的实施或者一点都没有进行。

Furthermore, while your transaction is applied to the database, no other transaction can alter it.

此外，当你的事务被应用到这个数据库的时候，其他事务不能修改该数据库。

As an example, imagine a table that lists the balances of all accounts in an electronic currency. If a transfer from one account to another is requested, the transactional nature of the database ensures that if the amount is subtracted from one account, it is always added to the other account. If due to whatever reason, adding the amount to the target account is not possible, the source account is also not modified.

举个例子，想象一张表，里面列出了某个电子货币所有账号的余额。当从一个账户到另外一个账户的转账请求发生时，这个数据库的事务特性确保从一个账户中减掉的金额会被加到另一个账户上。如果因为某种原因，往目标账户上增加金额无法进行，那么源账户的金额也不会发生任何变化。

Furthermore, a transaction is always cryptographically signed by the sender (creator). This makes it straightforward to guard access to specific modifications of the database. In the example of the electronic currency, a simple check ensures that only the person holding the keys to the account can transfer money from it.

此外，一个事务会被发送者（创建者）进行密码学签名。这项措施非常直观的为数据库的特定修改增加了访问保护。在电子货币的例子中，一个简单的检查就可以确保只有持有账户密钥的人，才能从该账户向外转账。

Blocks

区块

One major obstacle to overcome is what in bitcoin terms is called “double-spend attack”: What happens if two transactions exist in the network that both want to empty an account, a so-called conflict?

区块链要解决的一个主要难题，在比特币中被称为“双花攻击”。当网络上出现了两笔交易，都要花光一个账户中的钱时，会发生什么？一个冲突？

The abstract answer to this is that you do not have to care. An order of the transactions will be selected for you, the transactions will be bundled into what is called a “block” and then they will be executed and distributed among all participating nodes. If two transactions contradict each other, the one that ends up being second will be rejected and not become part of the block.

简单的回答是你不需要关心这个问题。这些交易会被排序并打包成“区块”，然后被所有参与的节点执行和分发。如果两笔交易相互冲突，排序靠后的交易会被拒绝并剔除出区块。

These blocks form a linear sequence in time and that is where the word “blockchain” derives from. Blocks are added to the chain in rather regular intervals - for Ethereum this is roughly every 17 seconds.

这些区块按时间排成一个线性序列。这也正是“区块链”这个词的由来。区块以一个相当规律的时间间隔加入到链上。对于以太坊，这个间隔大致是17秒。

As part of the “order selection mechanism” (which is called “mining”) it may happen that blocks are reverted from time to time, but only at the “tip” of the chain. The more blocks are reverted the less likely it is. So it might be that your transactions are reverted and even removed from the blockchain, but the longer you wait, the less likely it will be.

作为“顺序选择机制”（通常称为“挖矿”）的一部分，一段区块链可能会时不时被回滚。但这种情况只会发生在整条链的末端。回滚涉及的区块越多，其发生的概率越小。所以你的交易可能会被回滚，甚至会被从区块链中删除。但是你等待的越久，这种情况发生的概率就越小。

The Ethereum Virtual Machine

以太坊虚拟机

Overview

总览

The Ethereum Virtual Machine or EVM is the runtime environment for smart contracts in Ethereum. It is not only sandboxed but actually completely isolated, which means that code running inside the EVM has no access to network, filesystem or other processes. Smart contracts even have limited access to other smart contracts.

以太坊虚拟机（EVM）是以太坊中智能合约的运行环境。它不仅被沙箱封装起来，事实上它被完全隔离，也就是说运行在EVM内部的代码不能接触到网络、文件系统或者其它进程。甚至智能合约与其它智能合约只有有限的接触。

Accounts

账户

There are two kinds of accounts in Ethereum which share the same address space: **External accounts** that are controlled by public-private key pairs (i.e. humans) and **contract accounts** which are controlled by the code stored together with the account.

以太坊中有两类账户，它们共用同一个地址空间。外部账户，该类账户被公钥-私钥对控制（人类）。合约账户，该类账户被存储在账户中的代码控制。

The address of an external account is determined from the public key while the address of a contract is determined at the time the contract is created (it is derived from the creator address and the number of transactions sent from that address, the so-called “nonce”).

外部账户的地址是由公钥决定的，合约账户的地址是在创建该合约时确定的（这个地址由合约创建者的地址和该地址发出过的交易数量计算得到，地址发出过的交易数量也被称作"nonce"）

Apart from the fact whether an account stores code or not, the EVM treats the two types equally, though.

合约账户存储了代码，外部账户则没有，除了这点以外，这两类账户对于EVM来说是一样的。

Every account has a persistent key-value store mapping 256 bit words to 256 bit words called**storage**.

每个账户有一个key-value形式的持久化存储。其中key和value的长度都是256比特，名字叫做storage.

Furthermore, every account has a **balance** in Ether (in “Wei” to be exact) which can be modified by sending transactions that include Ether.

另外，每个账户都有一个以太币余额（单位是“Wei"），该账户余额可以通过向它发送带有以太币的交易来改变。

Transactions

交易

A transaction is a message that is sent from one account to another account (which might be the same or the special zero-account, see below). It can include binary data (its payload) and Ether.

一笔交易是一条消息，从一个账户发送到另一个账户（可能是相同的账户或者零账户，见下文）。交易可以包含二进制数据（payload）和以太币。

If the target account contains code, that code is executed and the payload is provided as input data.

如果目标账户包含代码，该代码会执行，payload就是输入数据。

If the target account is the zero-account (the account with the address 0), the transaction creates a **new contract**. As already mentioned, the address of that contract is not the zero address but an address derived from the sender and its number of transaction sent (the “nonce”). The payload of such a contract creation transaction is taken to be EVM bytecode and executed. The output of this execution is permanently stored as the code of the contract. This means that in order to create a contract, you do not send the actual code of the contract, but in fact code that returns that code.

如果目标账户是零账户（账户地址是0），交易将创建一个新合约。正如上文所讲，这个合约地址不是零地址，而是由合约创建者的地址和该地址发出过的交易数量（被称为nonce）计算得到。创建合约交易的payload被当作EVM字节码执行。执行的输出做为合约代码被永久存储。这意味着，为了创建一个合约，你不需要向合约发送真正的合约代码，而是发送能够返回真正代码的代码。

Gas

Gas

Upon creation, each transaction is charged with a certain amount of **gas**, whose purpose is to limit the amount of work that is needed to execute the transaction and to pay for this execution. While the EVM executes the transaction, the gas is gradually depleted according to specific rules.

以太坊上的每笔交易都会被收取一定数量的gas，gas的目的是限制执行交易所需的工作量，同时为执行支付费用。当EVM执行交易时，gas将按照特定规则被逐渐消耗。

The **gas price** is a value set by the creator of the transaction, who has to pay gas_price * gas up front from the sending account. If some gas is left after the execution, it is refunded in the same way.

gas price（以太币计）是由交易创建者设置的，发送账户需要预付的交易费用 = gas price * gas amount。 如果执行结束还有gas剩余，这些gas将被返还给发送账户。

If the gas is used up at any point (i.e. it is negative), an out-of-gas exception is triggered, which reverts all modifications made to the state in the current call frame.

无论执行到什么位置，一旦gas被耗尽（比如降为负值），将会触发一个out-of-gas异常。当前调用帧所做的所有状态修改都将被回滚。

Storage, Memory and the Stack

存储，主存和栈

Each account has a persistent memory area which is called **storage**. Storage is a key-value store that maps 256 bit words to 256 bit words. It is not possible to enumerate storage from within a contract and it is comparatively costly to read and even more so, to modify storage. A contract can neither read nor write to any storage apart from its own.

每个账户有一块持久化内存区域被称为存储。其形式为key-value，key和value的长度均为256比特。在合约里，不能遍历账户的存储。相对于另外两种，存储的读操作相对来说开销较大，修改存储更甚。一个合约只能对它自己的存储进行读写。

The second memory area is called **memory**, of which a contract obtains a freshly cleared instance for each message call. Memory can be addressed at byte level, but read and written to in 32 byte (256 bit) chunks. Memory is more costly the larger it grows (it scales quadratically).

第二个内存区被称为主存。合约执行每次消息调用时，都有一块新的，被清除过的主存。主存可以以字节粒度寻址，但是读写粒度为32字节（256比特）。操作主存的开销随着其增长而变大（平方级别）。

The EVM is not a register machine but a stack machine, so all computations are performed on an area called the **stack**. It has a maximum size of 1024 elements and contains words of 256 bits. Access to the stack is limited to the top end in the following way: It is possible to copy one of the topmost 16 elements to the top of the stack or swap the topmost element with one of the 16 elements below it. All other operations take the topmost two (or one, or more, depending on the operation) elements from the stack and push the result onto the stack. Of course it is possible to move stack elements to storage or memory, but it is not possible to just access arbitrary elements deeper in the stack without first removing the top of the stack.

EVM不是基于寄存器，而是基于栈的虚拟机。因此所有的计算都在一个被称为栈的区域执行。栈最大有1024个元素，每个元素256比特。对栈的访问只限于其顶端，方式为：允许拷贝最顶端的16个元素中的一个到栈顶，或者是交换栈顶元素和下面16个元素中的一个。所有其他操作都只能取最顶的两个（或一个，或更多，取决于具体的操作）元素，并把结果压在栈顶。当然可以把栈上的元素放到存储或者主存中。但是无法只访问栈上指定深度的那个元素，在那之前必须要把指定深度之上的所有元素都从栈中移除才行。

Instruction Set

指令集

The instruction set of the EVM is kept minimal in order to avoid incorrect implementations which could cause consensus problems. All instructions operate on the basic data type, 256 bit words. The usual arithmetic, bit, logical and comparison operations are present. Conditional and unconditional jumps are possible. Furthermore, contracts can access relevant properties of the current block like its number and timestamp.

EVM的指令集被刻意保持在最小规模，以尽可能避免可能导致共识问题的错误实现。所有的指令都是针对256比特这个基本的数据类型的操作。具备常用的算术，位，逻辑和比较操作。也可以做到条件和无条件跳转。此外，合约可以访问当前区块的相关属性，比如它的编号和时间戳。

Message Calls

消息调用

Contracts can call other contracts or send Ether to non-contract accounts by the means of message calls. Message calls are similar to transactions, in that they have a source, a target, data payload, Ether, gas and return data. In fact, every transaction consists of a top-level message call which in turn can create further message calls.

合约可以通过消息调用的方式来调用其它合约或者发送以太币到非合约账户。消息调用和交易非常类似，它们都有一个源，一个目标，数据负载，以太币，gas和返回数据。事实上每个交易都可以被认为是一个顶层消息调用，这个消息调用会依次产生更多的消息调用。

A contract can decide how much of its remaining **gas** should be sent with the inner message call and how much it wants to retain. If an out-of-gas exception happens in the inner call (or any other exception), this will be signalled by an error value put onto the stack. In this case, only the gas sent together with the call is used up. In Solidity, the calling contract causes a manual exception by default in such situations, so that exceptions “bubble up” the call stack.

As already said, the called contract (which can be the same as the caller) will receive a freshly cleared instance of memory and has access to the call payload - which will be provided in a separate area called the **calldata**. After it finished execution, it can return data which will be stored at a location in the caller’s memory preallocated by the caller.

一个合约可以决定剩余gas的分配。比如内部消息调用时使用多少gas，或者期望保留多少gas。如果在内部消息调用时发生了out-of-gas异常（或者其他异常），合约将会得到通知，一个错误码被压在栈上。这种情况只是内部消息调用的gas耗尽。在solidity中，这种情况下发起调用的合约默认会触发一个人工异常。这个异常会打印出调用栈。就像之前说过的，被调用的合约（发起调用的合约也一样）会拥有崭新的主存并能够访问调用的负载。调用负载被存储在一个单独的被称为calldata的区域。调用执行结束后，返回数据将被存放在调用方预先分配好的一块内存中。

Calls are **limited** to a depth of 1024, which means that for more complex operations, loops should be preferred over recursive calls.

调用层数被限制为1024，因此对于更加复杂的操作，我们应该使用循环而不是递归。

Callcode and Libraries

代码调用和库

There exists a special variant of a message call, named **callcode** which is identical to a message call apart from the fact that the code at the target address is executed in the context of the calling contract.

存在一种特殊类型的消息调用，被称为callcode。它跟消息调用几乎完全一样，只是加载自目标地址的代码将在发起调用的合约上下文中运行。

This means that a contract can dynamically load code from a different address at runtime. Storage, current address and balance still refer to the calling contract, only the code is taken from the called address.

这意味着一个合约可以在运行时从另外一个地址动态加载代码。存储，当前地址和余额都指向发起调用的合约，只有代码是从被调用地址获取的。

This makes it possible to implement the “library” feature in Solidity: Reusable library code that can be applied to a contract’s storage in order to e.g. implement a complex data structure.

这使得Solidity可以实现”库“。可复用的库代码可以应用在一个合约的存储上，可以用来实现复杂的数据结构。

Logs

日志

It is possible to store data in a specially indexed data structure that maps all they way up to the block level. This feature called **logs** is used by Solidity in order to implement **events**. Contracts cannot access log data after it has been created, but they can be efficiently accessed from outside the blockchain. Since some part of the log data is stored in bloom filters, it is possible to search for this data in an efficient and cryptographically secure way, so network peers that do not download the whole blockchain (“light clients”) can still find these logs.

在区块层面，可以用一种特殊的可索引的数据结构来存储数据。这个特性被称为日志，Solidity用它来实现事件。合约创建之后就无法访问日志数据，但是这些数据可以从区块链外高效的访问。因为部分日志数据被存储在布隆过滤器（Bloom filter) 中，我们可以高效并且安全的搜索日志，所以那些没有下载整个区块链的网络节点（轻客户端）也可以找到这些日志。

Create

创建

Contracts can even create other contracts using a special opcode (i.e. they do not simply call the zero address). The only difference between these **create calls** and normal message calls is that the payload data is executed and the result stored as code and the caller / creator receives the address of the new contract on the stack.

合约甚至可以通过一个特殊的指令来创建其他合约（不是简单的向零地址发起调用）。创建合约的调用跟普通的消息调用的区别在于，负载数据执行的结果被当作代码，调用者/创建者在栈上得到新合约的地址。

Selfdestruct

自毁

The only possibility that code is removed from the blockchain is when a contract at that address performs the SELFDESTRUCT operation. The remaining Ether stored at that address is sent to a designated target and then the storage and code is removed.

只有在某个地址上的合约执行自毁操作时，合约代码才会从区块链上移除。合约地址上剩余的以太币会发送给指定的目标，然后其存储和代码被移除。

Note that even if a contract’s code does not contain the SELFDESTRUCT opcode, it can still perform that operation using callcode.

注意，即使一个合约的代码不包含自毁指令，依然可以通过代码调用(callcode)来执行这个操作。
