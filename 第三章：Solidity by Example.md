原文：https://solidity.readthedocs.org/en/latest/solidity-by-example.html 
###################
Solidity by Example Solidity编程实例
###################

.. index:: voting, ballot

.. _voting:

******
Voting 投票
******

The following contract is quite complex, but showcases
a lot of Solidity's features. It implements a voting
contract. Of course, the main problems of electronic
voting is how to assign voting rights to the correct
persons and how to prevent manipulation. We will not
solve all problems here, but at least we will show 
how delegated voting can be done so that vote counting
is **automatic and completely transparent** at the
same time.
接下来的合约非常复杂，但展示了很多Solidity的特性。它实现了一个投票合约。当然，电子选举的主要问题是如何赋予投票权给准确的人，并防止操纵。我们不能解决所有的问题，但至少我们会展示如何委托投票可以同时做到投票统计是**自动和完全透明**。

The idea is to create one contract per ballot,
providing a short name for each option.
Then the creator of the contract who serves as
chairperson will give the right to vote to each
address individually.
思路是为每张选票创建一个合约，每个投票选项提供一个短名称。合约创建者作为主持人将会给每个投票参与人各自的地址投票权。

The persons behind the addresses can then choose
to either vote themselves or to delegate their
vote to a person they trust.
地址后面的人们可以选择自己投票或者委托信任的代表人替他们投票。
At the end of the voting time, `winningProposal()`
will return the proposal with the largest number
of votes.
在投票期间，`winningProposal()`将会返回获得票数最多的提案。
.. Gist: 618560d3f740204d46a5

::

    /// @title Voting with delegation.
   /// @title 授权投票
    contract Ballot
    {
        // This declares a new complex type which will
        // be used for variables later.
        // It will represent a single voter.
       //这里声明了一个后面将会用到的新复合类型，他代表一个独立的投票人。
        struct Voter
        {
            uint weight; // weight is accumulated by delegation 作为代表累积的权重。
            bool voted;  // if true, that person already voted 如果为true，则表示该投票人已经投票。
            address delegate; // person delegated to  委托的投票代表
            uint vote;   // index of the voted proposal 投票选择的提案索引号
        }
        // This is a type for a single proposal.
       // 这是一个独立提案的类型
        struct Proposal
        {
            bytes32 name;   // short name (up to 32 bytes) 短名称（最多32字节）
            uint voteCount; // number of accumulated votes 累计获得的票数
        }

        address public chairperson;
        // This declares a state variable that
        // stores a `Voter` struct for each possible address.
      //这里声明一个状态变量，保存每个独立地址的`Voter` 结构
        mapping(address => Voter) public voters;
        // A dynamically-sized array of `Proposal` structs.
        //一个存储`Proposal`结构的动态数组
        Proposal[] public proposals;

        /// Create a new ballot to choose one of `proposalNames`.
        ///创建一个新的投票用于选出一个提案名`proposalNames`.
        function Ballot(bytes32[] proposalNames)
        {
            chairperson = msg.sender;
            voters[chairperson].weight = 1;
            // For each of the provided proposal names,
            // create a new proposal object and add it
            // to the end of the array.
            //对提供的每一个提案名称，创建一个新的提案
            //对象添加到数组末尾
            for (uint i = 0; i < proposalNames.length; i++)
                // `Proposal({...})` creates a temporary
                // Proposal object and `proposal.push(...)`
                // appends it to the end of `proposals`.
                //`Proposal({...})` 创建了一个临时的提案对象，
                //`proposal.push(...)`添加到了提案数组`proposals`末尾。
                proposals.push(Proposal({
                    name: proposalNames[i],
                    voteCount: 0
                }));
        }

        // Give `voter` the right to vote on this ballot.
        // May only be called by `chairperson`.
         //给投票人`voter`参加投票的投票权，
        //只能由投票主持人`chairperson`调用。
        function giveRightToVote(address voter)
        {
            if (msg.sender != chairperson || voters[voter].voted)
                // `throw` terminates and reverts all changes to
                // the state and to Ether balances. It is often
                // a good idea to use this if functions are
                // called incorrectly. But watch out, this
                // will also consume all provided gas.
                //`throw`会终止和撤销所有的状态和以太改变。
               //如果函数调用无效，这通常是一个好的选择。
               //但是需要注意，这会消耗提供的所有gas。
                throw;
            voters[voter].weight = 1;
        }

        /// Delegate your vote to the voter `to`.
        ///委托你的投票权到一个投票代表 `to`。
        function delegate(address to)
        {
            // assigns reference
            //指定引用
            Voter sender = voters[msg.sender];
            if (sender.voted)
                throw;
            // Forward the delegation as long as
            // `to` also delegated.
            //当投票代表`to`也委托给别人时，寻找到最终的投票代表
            while (voters[to].delegate != address(0) &&
                   voters[to].delegate != msg.sender)
                to = voters[to].delegate;
            // We found a loop in the delegation, not allowed.
    //当最终投票代表等于调用者，是不被允许的循环委托。
            if (to == msg.sender)
                throw;
            // Since `sender` is a reference, this
            // modifies `voters[msg.sender].voted`
            //因为`sender`是一个引用，
            //这里实际修改了`voters[msg.sender].voted`
            sender.voted = true;
            sender.delegate = to;
            Voter delegate = voters[to];
            if (delegate.voted)
                // If the delegate already voted,
                // directly add to the number of votes 
                //如果委托的投票代表已经投票了，直接修改票数
                proposals[delegate.vote].voteCount += sender.weight;
            else
                // If the delegate did not vote yet,
                // add to her weight.
                //如果投票代表还没有投票，则修改其投票权重。
                delegate.weight += sender.weight;
        }

        /// Give your vote (including votes delegated to you)
        /// to proposal `proposals[proposal].name`.
        ///投出你的选票（包括委托给你的选票）
        ///给 `proposals[proposal].name`指定的提案。
        function vote(uint proposal)
        {
            Voter sender = voters[msg.sender];
            if (sender.voted) throw;
            sender.voted = true;
            sender.vote = proposal;
            // If `proposal` is out of the range of the array,
            // this will throw automatically and revert all
            // changes.
            //如果`proposal`索引超出了给定的提案数组范围
            //将会自动抛出异常，并撤销所有的改变。
            proposals[proposal].voteCount += sender.weight;
        }

        /// @dev Computes the winning proposal taking all
        /// previous votes into account.
       ///@dev 根据当前所有的投票计算出当前的胜出提案
        function winningProposal() constant
                returns (uint winningProposal)
        {
            uint winningVoteCount = 0;
            for (uint p = 0; p < proposals.length; p++)
            {
                if (proposals[p].voteCount > winningVoteCount)
                {
                    winningVoteCount = proposals[p].voteCount;
                    winningProposal = p;
                }
            }
        }
    }

Possible Improvements 可能的改进
=====================

Currently, many transactions are needed to assign the rights
to vote to all participants. Can you think of a better way?
现在，指派投票权到所有的投票参加者需要许多的交易，你能想出更好的方法么？
.. index:: auction;blind, auction;open, blind auction, open auction

*************
Blind Auction 盲拍
*************

In this section, we will show how easy it is to create a
completely blind auction contract on Ethereum.
We will start with an open auction where everyone
can see the bids that are made and then extend this
contract into a blind auction where it is not
possible to see the actual bid until the bidding
period ends.
这一节，我们将展示在以太上创建一个完整的盲拍合约是多么简单。我们从一个所有人都能看到出价的公开拍卖开始，接着扩展合约成为一个在拍卖结束以前不能看到实际出价的盲拍。
Simple Open Auction 简单的公开拍卖
===================

The general idea of the following simple auction contract
is that everyone can send their bids during
a bidding period. The bids already include sending
money / ether in order to bind the bidders to their
bid. If the highest bid is raised, the previously
highest bidder gets her money back.
After the end of the bidding period, the
contract has to be called manually for the
beneficiary to receive his money - contracts cannot
activate themselves.
通常简单的公开拍卖合约，是每个人可以在拍卖期间发送他们的竞拍出价。竞拍订单绑定到其竞拍人并包含需要发送的钱和以太。如果产生了新的最高竞拍价，前一个最高价竞拍人将会拿回他的钱。在竞拍阶段结束后，拍卖人需要手动调用合约收取他的钱 — — 合约不会激活自己。

.. {% include open_link gist="48cd2b65ff83bd04f7af" %}

::

    contract SimpleAuction {
        // Parameters of the auction. Times are either
        // absolute unix timestamps (seconds since 1970-01-01)
        // ore time periods in seconds.
        //拍卖的参数。
       //时间要么为unix绝对时间戳（自1970-01-01以来的秒数），
       //或者是以秒为单位的出块时间
        address public beneficiary;
        uint public auctionStart;
        uint public biddingTime;

        // Current state of the auction.
        //当前的拍卖状态
        address public highestBidder;
        uint public highestBid;

        // Set to true at the end, disallows any change
       //在结束时设置为true来拒绝任何改变
        bool ended;

        // Events that will be fired on changes.
       //当改变时将会触发的事件
        event HighestBidIncreased(address bidder, uint amount);
        event AuctionEnded(address winner, uint amount);

        // The following is a so-called natspec comment,
        // recognizable by the three slashes.
        // It will be shown when the user is asked to
        // confirm a transaction.
        //下面是一个叫做natspec的特殊注释，
        //由3个连续的斜杠标记，当询问用户确认交易事务时将显示。

        /// Create a simple auction with `_biddingTime`
        /// seconds bidding time on behalf of the
        /// beneficiary address `_beneficiary`.
        ///创建一个简单的合约使用`_biddingTime`表示的竞拍时间，
       /// 地址`_beneficiary`.代表实际的拍卖人
        function SimpleAuction(uint _biddingTime,
                               address _beneficiary) {
            beneficiary = _beneficiary;
            auctionStart = now;
            biddingTime = _biddingTime;
        }

        /// Bid on the auction with the value sent
        /// together with this transaction.
        /// The value will only be refunded if the
        /// auction is not won.
        ///对拍卖的竞拍保证金会随着交易事务一起发送，
       ///只有在竞拍失败的时候才会退回
        function bid() {
            // No arguments are necessary, all
            // information is already part of
            // the transaction.
           //不需要任何参数，所有的信息已经是交易事务的一部分
            if (now > auctionStart + biddingTime)
                // Revert the call if the bidding
                // period is over.
               //当竞拍结束时撤销此调用
                throw;
            if (msg.value <= highestBid)
                // If the bid is not higher, send the
                // money back.
               //如果出价不是最高的，发回竞拍保证金。
                throw;
            if (highestBidder != 0)
                highestBidder.send(highestBid);
            highestBidder = msg.sender;
            highestBid = msg.value;
            HighestBidIncreased(msg.sender, msg.value);
        }

        /// End the auction and send the highest bid
        /// to the beneficiary.
       ///拍卖结束后发送最高的竞价到拍卖人
        function auctionEnd() {
            if (now <= auctionStart + biddingTime)
                throw; // auction did not yet end
                          //拍卖还没有结束
            if (ended)
                throw; // this function has already been called
         //这个收款函数已经被调用了
            AuctionEnded(highestBidder, highestBid);
            // We send all the money we have, because some
            // of the refunds might have failed.
            //发送合约拥有所有的钱，因为有一些保证金可能退回失败了。

            beneficiary.send(this.balance);
            ended = true;
        }

        function () {
            // This function gets executed if a
            // transaction with invalid data is sent to
            // the contract or just ether without data.
            // We revert the send so that no-one
            // accidentally loses money when using the
            // contract.
            //这个函数将会在发送到合约的交易事务包含无效数据
            //或无数据的时执行，这里撤销所有的发送，
            //所以没有人会在使用合约时因为意外而丢钱。
            throw;
        }
    }

Blind Auction 盲拍
================

The previous open auction is extended to a blind auction
in the following. The advantage of a blind auction is
that there is no time pressure towards the end of
the bidding period. Creating a blind auction on a
transparent computing platform might sound like a
contradiction, but cryptography comes to the rescue.
接下来扩展前面的公开拍卖成为一个盲拍。盲拍的特点是拍卖结束以前没有时间压力。在一个透明的计算平台上创建盲拍系统听起来可能有些矛盾，但是加密算法能让你脱离困境。
During the **bidding period**, a bidder does not
actually send her bid, but only a hashed version of it.
Since it is currently considered practically impossible
to find two (sufficiently long) values whose hash
values are equal, the bidder commits to the bid by that.
After the end of the bidding period, the bidders have
to reveal their bids: They send their values
unencrypted and the contract checks that the hash value
is the same as the one provided during the bidding period.
在**拍卖阶段**, 竞拍人不需要发送实际的出价，仅仅只需要发送一个散列版本。因为目前几乎不可能找到两个值（足够长）的散列值相等，竞拍者提交他们的出价散列值。在拍卖结束后，竞拍人重新发送未加密的竞拍出价，合约将检查其散列值是否和拍卖阶段发送的一样。
Another challenge is how to make the auction
**binding and blind** at the same time: The only way to
prevent the bidder from just not sending the money
after he won the auction is to make her send it
together with the bid. Since value transfers cannot
be blinded in Ethereum, anyone can see the value.
另一个挑战是如何让拍卖同时实现**绑定和致盲** ：防止竞拍人竞拍成功后不付钱的唯一的办法是，在竞拍出价的同时发送保证金。但是在Ethereum上发送保证金是无法致盲，所有人都能看到保证金。
The following contract solves this problem by
accepting any value that is at least as large as
the bid. Since this can of course only be checked during
the reveal phase, some bids might be **invalid**, and
this is on purpose (it even provides an explicit
flag to place invalid bids with high value transfers):
Bidders can confuse competition by placing several
high or low invalid bids.
下面的合约通过接受任何尽量大的出价来解决这个问题。当然这可以在最后的揭拍阶段进行复核，一些竞拍出价可能是**无效的**，这样做的目的是（它提供一个显式的标志指出是无效的竞拍，同时包含高额保证金）：竞拍人可以通过放置几个无效的高价和低价竞拍来混淆竞争对手。

.. {% include open_link gist="70528429c2cd867dd1d6" %}

::

    contract BlindAuction
    {
        struct Bid
        {
            bytes32 blindedBid;
            uint deposit;
        }
        address public beneficiary;
        uint public auctionStart;
        uint public biddingEnd;
        uint public revealEnd;
        bool public ended;

        mapping(address => Bid[]) public bids;

        address public highestBidder;
        uint public highestBid;

        event AuctionEnded(address winner, uint highestBid);

        /// Modifiers are a convenient way to validate inputs to
        /// functions. `onlyBefore` is applied to `bid` below:
        /// The new function body is the modifier's body where
        /// `_` is replaced by the old function body.
       ///修饰器（Modifier）是一个简便的途径用来验证函数输入的有效性。
       ///`onlyBefore` 应用于下面的 `bid`函数，其旧的函数体替换修饰器主体中 `_`后就是其新的函数体
        modifier onlyBefore(uint _time) { if (now >= _time) throw; _ }
        modifier onlyAfter(uint _time) { if (now <= _time) throw; _ }

        function BlindAuction(uint _biddingTime,
                                uint _revealTime,
                                address _beneficiary)
        {
            beneficiary = _beneficiary;
            auctionStart = now;
            biddingEnd = now + _biddingTime;
            revealEnd = biddingEnd + _revealTime;
        }

        /// Place a blinded bid with `_blindedBid` = sha3(value,
        /// fake, secret).
        /// The sent ether is only refunded if the bid is correctly
        /// revealed in the revealing phase. The bid is valid if the
        /// ether sent together with the bid is at least "value" and
        /// "fake" is not true. Setting "fake" to true and sending
        /// not the exact amount are ways to hide the real bid but
        /// still make the required deposit. The same address can
        /// place multiple bids.
       ///放置一个盲拍出价使用`_blindedBid`=sha3(value,fake,secret).
       ///仅仅在竞拍结束正常揭拍后退还发送的以太。当随同发送的以太至少
       ///等于 "value"指定的保证金并且 "fake"不为true的时候才是有效的竞拍
       ///出价。设置 "fake"为true或发送不合适的金额将会掩没真正的竞拍出
       ///价，但是仍然需要抵押保证金。同一个地址可以放置多个竞拍。
        function bid(bytes32 _blindedBid)
            onlyBefore(biddingEnd)
        {
            bids[msg.sender].push(Bid({
                blindedBid: _blindedBid,
                deposit: msg.value
            }));
        }

        /// Reveal your blinded bids. You will get a refund for all
        /// correctly blinded invalid bids and for all bids except for
        /// the totally highest.
       ///揭开你的盲拍竞价。你将会拿回除了最高出价外的所有竞拍保证金
       ///以及正常的无效盲拍保证金。
        function reveal(uint[] _values, bool[] _fake,
                        bytes32[] _secret)
            onlyAfter(biddingEnd)
            onlyBefore(revealEnd)
        {
            uint length = bids[msg.sender].length;
            if (_values.length != length || _fake.length != length ||
                        _secret.length != length)
                throw;
            uint refund;
            for (uint i = 0; i < length; i++)
            {
                var bid = bids[msg.sender][i];
                var (value, fake, secret) =
                        (_values[i], _fake[i], _secret[i]);
                if (bid.blindedBid != sha3(value, fake, secret))
                    // Bid was not actually revealed.
                    // Do not refund deposit.
                    //出价未被正常揭拍，不能取回保证金。
                    continue;
                refund += bid.deposit;
                if (!fake && bid.deposit >= value)
                    if (placeBid(msg.sender, value))
                        refund -= value;
                // Make it impossible for the sender to re-claim
                // the same deposit.
                //保证发送者绝不可能重复取回保证金
                bid.blindedBid = 0;
            }
            msg.sender.send(refund);
        }

        // This is an "internal" function which means that it
        // can only be called from the contract itself (or from
        // derived contracts).
        //这是一个内部 (internal)函数，
       //意味着仅仅只有合约（或者从其继承的合约）可以调用
        function placeBid(address bidder, uint value) internal
                returns (bool success)
        {
            if (value <= highestBid)
                return false;
            if (highestBidder != 0)
                // Refund the previously highest bidder.
                //退还前一个最高竞拍出价
                highestBidder.send(highestBid);
            highestBid = value;
            highestBidder = bidder;
            return true;
        }

        /// End the auction and send the highest bid
        /// to the beneficiary.
       ///竞拍结束后发送最高出价到拍卖人
        function auctionEnd()
            onlyAfter(revealEnd)
        {
            if (ended) throw;
            AuctionEnded(highestBidder, highestBid);
            // We send all the money we have, because some
            // of the refunds might have failed.
            //发送合约拥有所有的钱，因为有一些保证金退回可能失败了。
            beneficiary.send(this.balance);
            ended = true;
        }

        function () { throw; }
    }

.. index:: purchase, remote purchase, escrow

********************
Safe Remote Purchase 安全的远程购物
********************

.. {% include open_link gist="b16e8e76a423b7671e99" %}

::

    contract Purchase
    {
        uint public value;
        address public seller;
        address public buyer;
        enum State { Created, Locked, Inactive }
        State public state;
        function Purchase()
        {
            seller = msg.sender;
            value = msg.value / 2;
            if (2 * value != msg.value) throw;
        }
        modifier require(bool _condition)
        {
            if (!_condition) throw;
            _
        }
        modifier onlyBuyer()
        {
            if (msg.sender != buyer) throw;
            _
        }
        modifier onlySeller()
        {
            if (msg.sender != seller) throw;
            _
        }
        modifier inState(State _state)
        {
            if (state != _state) throw;
            _
        }
        event aborted();
        event purchaseConfirmed();
        event itemReceived();

        /// Abort the purchase and reclaim the ether.
        /// Can only be called by the seller before
        /// the contract is locked.
       ///终止购物并收回以太。仅仅可以在合约未锁定时被卖家调用。
        function abort()
            onlySeller
            inState(State.Created)
        {
            aborted();
            seller.send(this.balance);
            state = State.Inactive;
        }
        /// Confirm the purchase as buyer.
        /// Transaction has to include `2 * value` ether.
        /// The ether will be locked until confirmReceived
        /// is called.
       ///买家确认购买。交易事务包含两倍价值的（`2 * value`）以太。
       ///这些以太会一直锁定到收货确认(confirmReceived)被调用。
        function confirmPurchase()
            inState(State.Created)
            require(msg.value == 2 * value)
        {
            purchaseConfirmed();
            buyer = msg.sender;
            state = State.Locked;
        }
        /// Confirm that you (the buyer) received the item.
        /// This will release the locked ether.
        ///确认你（买家）收到了货物，这将释放锁定的以太。
        function confirmReceived()
            onlyBuyer
            inState(State.Locked)
        {
            itemReceived();
            buyer.send(value); // We ignore the return value on purpose
           //我们有意忽略了返回值。
            seller.send(this.balance);
            state = State.Inactive;
        }
        function() { throw; }
    }

********************
Micropayment Channel 小额支付通道
********************

To be written.
待补


