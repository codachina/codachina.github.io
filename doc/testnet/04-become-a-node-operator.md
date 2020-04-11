---
title: 成为节点
toc: true
---

原文：https://codaprotocol.com/docs/

翻译：Star.LI (star@trapdoortech.com)

危险提示

节点操作命令仍处于稳定状态，因此这些命令可能会更改。 请尝试并提交任何修复。

现在我们已经设置了Coda节点并发送了第一笔交易，让我们将注意力转移到其他可以与Coda网络进行交互的方法上，即参与共识，并通过生成zk-SNARK来帮助压缩数据。 通过运行有助于保护网络安全的节点，您将获得Coda作为奖励。

## 参与共识

Coda网络采用[POS共识](https://codaprotocol.com/docs/glossary/#proof-of-stake) 。 采用POS共识算法，您不需要像比特币采矿那样拥有复杂的计算机设备。 通过简单地将Coda放入我们的钱包中，我们可以选择将其委托给自己或者其他节点。 首先让我们看看如何委托Coda：

## 抵押Coda

通过[发送交易](https://codaprotocol.com/docs/my-first-transaction)，我们获取了一些Coda，因此我们现在可以使用`-propose-key`选项在启动守护进程时抵押Coda。 先停止当前的守护进程，并使用以下命令重新启动它，设置存储私钥的路径（我们之前在`keys/my-wallet`中创建了密钥对）：

```
coda daemon \
    -discovery-port 8303 \
    -peer /dns4/seed-one.genesis.o1test.net/tcp/10002/ipfs/12D3KooWP7fTKbyiUcYJGajQDpCFo2rDexgTHFJTxCH8jvcL1eAH \
    -peer /dns4/seed-two.genesis.o1test.net/tcp/10002/ipfs/12D3KooWL9ywbiXNfMBqnUKHSB1Q1BaHFNUzppu6JLMVn9TTPFSA \
    -propose-key keys/my-wallet
```

注意

你可以提供多个私钥存储路径，同时完成多个抵押。

可以通过`coda client status` 命令查看抵押情况：

```
coda client status

Coda Daemon Status 
-----------------------------------

Global Number of Accounts:                     18
The Total Number of Blocks in the Blockchain:  1
Local Uptime:                                  36s
Ledger Merkle Root:                            ...
Staged-ledger Hash:                            ...
Staged Hash:                                   ...
GIT SHA1:                                      ...
Configuration Directory:                       ...
Peers:                                         ...
User_commands Sent:                            0
Snark Worker Running:                          false
Sync Status:                                   Offline
Proposers Running:                             Total: 1 (8QnLUNW7sUxnApau4SLShwr25koiSKrECxtveu89PPmQW5pEyy3xK8YgRpZQkZEanc)
Best Tip Consensus Time:                       0:0
Consensus Time Now:                            11542:461
Consensus Mechanism:                           proof_of_stake
...
```

其中，`Proposers Running`指定抵押的账户信息。

警告

请记住，如果您抵押给自己，则需要始终保持与网络的连接，因为你的节点会被选为出块节点。 如果您需要经常下线，最好抵押给其他节点。

## 委托 coda

委派Coda是直接抵押的另外一个选择，其好处是不必始终保持与网络的连接。 但是，请记住：

- 更改委托，您将需要支付少量交易费用，因为更改委托也是一种交易
- 您委托的节点可以选择向您收取佣金

委托需要执行如下的命令：

```
coda client delegate-stake \
    -delegate <delegate-public-key> \
    -privkey-path <file> \
    -fee <fee>
```

命令中的参数为：

- `delegate`，委托节点的公钥信息
- `privkey-path` ，是自己的存储私钥的路径
- `fee` ，交易费用

在如下情况下，可以进行委托：

- 从“冷钱包”中，给自己的节点进行委托
- 委托给“抵押池”，获取周期性的收益

注意

此更改生效之前有一天的等待期，以防止滥用网络。

## 数据压缩

Coda协议的独特之处在于，它不需要节点像其他加密货币协议一样维护区块链的完整历史记录。 通过递归证明，Coda协议有效地将区块链压缩到恒定大小。 我们称其为压缩，因为它可以将TB的数据减少到几千字节。

但是，这不是传统意义上的数据编码或压缩，而是节点通过生成零知识证明来“压缩”网络中的数据。 节点在此过程中扮演着至关重要的角色，他们将自己指定为“ snark-worker”，为已添加到区块中的交易生成zk-SNARK证明。

当 [启动守护进程](https://codaprotocol.com/docs/my-first-transaction/#start-up-a-node)时，设置其他的参数启动“snark-worker”：

```
coda daemon \
    -discovery-port 8303 \
    -peer /dns4/seed-one.genesis.o1test.net/tcp/10002/ipfs/12D3KooWP7fTKbyiUcYJGajQDpCFo2rDexgTHFJTxCH8jvcL1eAH \
    -peer /dns4/seed-two.genesis.o1test.net/tcp/10002/ipfs/12D3KooWL9ywbiXNfMBqnUKHSB1Q1BaHFNUzppu6JLMVn9TTPFSA \
    -run-snark-worker $CODA_PK \
    -snark-worker-fee <fee>
```
作为“snark-worker”，可以获取一部分区块奖励。 区块生产者负责收集交易，并且按照协议奖励snark-worker。

该节内容涵盖了作为Coda节点的角色和职责。 由于Coda是一个无需许可的点对点网络，因此Coda协议由世界各地的节点以分散的方式进行管理和运行。 同样，Coda项目也是分布式的，无需许可即可加入。 Coda协议的代码都是开源的，并且有很多工作要做，无论是技术性的还是非技术性的。 要了解有关如何参与Coda的更多信息，请查看[如何贡献](https://codaprotocol.com/contributing)。
