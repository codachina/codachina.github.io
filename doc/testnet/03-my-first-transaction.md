---
title: 第一个交易
toc: true
---

原文：https://codaprotocol.com/docs/

翻译：Star.LI (star@trapdoortech.com)

在本节中，我们将在Coda网络上进行第一笔交易。 [安装软件](https://codaprotocol.com/getting-started)之后，我们需要先创建一个新帐户，然后才能发送或接收Coda。 让我们首先启动节点。

## 启动节点

运行以下命令以启动Coda节点并连接到网络：

```
coda daemon \
    -discovery-port 8303 \
    -peer /dns4/seed-one.genesis.o1test.net/tcp/10002/ipfs/12D3KooWP7fTKbyiUcYJGajQDpCFo2rDexgTHFJTxCH8jvcL1eAH \
    -peer /dns4/seed-two.genesis.o1test.net/tcp/10002/ipfs/12D3KooWL9ywbiXNfMBqnUKHSB1Q1BaHFNUzppu6JLMVn9TTPFSA
```

上面指定的主机和端口是指种子地址。 由于Coda是[peer-to-peer](https://codaprotocol.com/glossary/#peer-to-peer)协议，因此我们不依赖任何单一的中心化服务器。 如果使用了自定义端口（TCP的不是8303），则需要向上面的命令传递一个额外的选项：`-external-port`。

注意

每当您使用`coda client`发出命令时，守护进程都需要运行，因此请确保不要意外杀死守护进程。

首次运行节点时的常见问题，请参见[常见问题](https://codaprotocol.com/docs/troubleshooting/)。

## 查看网络连接

现在我们已经启动了一个节点并正在运行Coda守护程序，打开另一个终端并运行以下命令：

```
coda client status
```

注意

目前，`coda client status`在首次启动时可能需要一分钟才能连接到守护程序。 因此，如果您看到“错误：守护程序未运行，请查看coda守护程序是否运行”，稍等片刻重试。

我们很可能会看到包含以下字段的回复：

```
...
Peers:                                         Total: 4 (...)
...
Sync Status:                                   Bootstrap
```

如果您看到`Sync Status: Bootstrap`，则表示Coda节点正在启动，需要与网络的其余部分进行同步。 您可能需要耐心等待，因为此步骤可能需要一些时间才能使节点获取所需的所有数据。 当同步状态达到`Synced`并且该节点连接到1个或多个对等节点时，我们才成功连接到网络。

## 创建账户

节点同步后，我们将创建一个公钥/私钥对，以便我们可以交易签名，以及生成一个地址以接收付款。 出于安全原因，我们希望将密钥放在攻击者更难以访问的目录下。

运行以下任意一个命令，将在当前目录的`keys`子目录下创建一个公共和私有密钥`my-wallet`和`my-wallet.pub`：

```
coda client-old generate-keypair -privkey-path keys/my-wallet 

coda accounts create
```

警告

公钥可以分享给任何人， 但切勿与任何人共享私钥，因为它等同于您的资金密码。

```
Keypair generated
Public key:  4vsRCVbLm6LvUyzYWT95WaCzyi4D4UHxRpLBhMn7q2mRPgNCgRG3Jr3tDuhgQdmzbvCcBxhUwB3REpY2Dyf1NAxSSs8Q2vdJX93pT7eyqcyRU2S9UpDddDgovj46BSknNjzydKoopebp5Kva
```

由于公钥很长且难以记住，因此将其保存为环境变量：

```
export CODA_PK=<public-key>
```

现在我们可以在任何地方以`$ CODA_PK`的形式访问它。 请注意，这只会保存到当前的终端会话中，因此，如果要保存它以备将来使用，可以将其添加到您的`~/.profile`或`~/.bash_profile`中。

## 获取测试Coda

为了发送第一笔交易，我们首先需要获取一些Coda。 转到[Coda Discord服务器](https://bit.ly/CodaDiscord)并加入“ #faucet”频道。 向Tiny的机器人索要一些Coda（您将收到100个Coda）。 命令如下：

```
$request <public-key>
```

一旦批准了您的请求，请密切关注Discord渠道以查看交易何时进行。 您的资金可能要过几分钟才能显示。

我们可以通过运行以下命令并输入公钥来检查余额，以确保我们收到了资金：

```
coda client get-balance -public-key $CODA_PUBLIC_KEY
```

您可能会看到`No account found at that public_key (zero balance)`。 耐心等待！ 根据网络中的交易情况，可能需要花费一些时间才能完成交易。

在等待期间，请运行以下命令以查看当前区块的高度：

```
coda client status
```

## 转账

最后，我们可以发送了第一笔交易！ 为了进行测试，我们已经设置了[自动退回服务](https://github.com/CodaProtocol/coda-automation/tree/master/services/echo)，该应用将立即退还您的付款（扣除了交易费用）。

警告

目前，自动退回服务存在一个已知问题，导致其无法正确退回您的付款！ 不用担心，我们仍然会为您提供Testnet积分[*]（https://codaprotocol.com/docs/my-first-transaction#disclaimer），以完成挑战。

让我们转账给自动退回服务。命令类似：

```
coda client send-payment \
  -amount 20 \
  -receiver 4vsRCVNep7JaFhtySu6vZCjnArvoAhkRscTy5TQsGTsKM4tJcYVc3uNUMRxQZAwVzSvkHDGWBmvhFpmCeiPASGnByXqvKzmHt4aR5uAWAQf3kqhwDJ2ZY3Hw4Dzo6awnJkxY338GEp12LE4x \
  -fee 5 \
  -sender $YOUR_PUBLIC_KEY
```

上述命令中的参数为：

- `amount`，交易金额（20 Coda）
- `receiver`， [自动退回服务地址](https://github.com/CodaProtocol/coda-automation/tree/master/services/echo)
- `fee`，交易手续费
- `sender`，交易发送方

如果上述交易发送成功，会收到如下的回复信息：
```
Dispatched payment with ID 3XCgvAHLAqz9VVbU7an7f2L5ffJtZoFega7jZpVJrPCYA4j5HEmUAx51BCeMc232eBWVz6q9t62Kp2cNvQZoNCSGqJ1rrJpXFqMN6NQe7x987sAC2Sd6wu9Vbs9xSr8g1AkjJoB65v3suPsaCcvvCjyUvUs8c3eVRucH4doa2onGj41pjxT53y5ZkmGaPmPnpWzdJt4YJBnDRW1GcJeyqj61GKWcvvrV6KcGD25VEeHQBfhGppZc7ewVwi3vcUQR7QFFs15bMwA4oZDEfzSbnr1ECoiZGy61m5LX7afwFaviyUwjphtrzoPbQ2QAZ2w2ypnVUrcJ9oUT4y4dvDJ5vkUDazRdGxjAA6Cz86bJqqgfMHdMFqpkmLxCdLbj2Nq3Ar2VpPVvfn2kdKoxwmAGqWCiVhqYbTvHkyZSc4n3siGTEpTGAK9usPnBnqLi53Z2bPPaJ3PuZTMgmdZYrRv4UPxztRtmyBz2HdQSnH8vbxurLkyxK6yEwS23JSZWToccM83sx2hAAABNynBVuxagL8aNZF99k3LKX6E581uSVSw5DAJ2S198DvZHXD53QvjcDGpvB9jYUpofkk1aPvtW7QZkcofBYruePM7kCHjKvbDXSw2CV5brHVv5ZBV9DuUcuFHfcYAA2TVuDtFeNLBjxDumiBASgaLvcdzGiFvSqqnzmS9MBXxYybQcmmz1WuKZHjgqph99XVEapwTsYfZGi1T8ApahcWc5EX9
Receipt chain hash is now A3gpLyBJGvcpMXny2DsHjvE5GaNFn2bbpLLQqTCHuY3Nd7sqy8vDbM6qHTwHt8tcfqqBkd36LuV4CC6hVH6YsmRqRp4Lzx77WnN9gnRX7ceeXdCQUVB7B2uMo3oCYxfdpU5Q2f2KzJQ46
```

## 查看余额

现在我们可以发送交易了。让我们通过运行以下命令来检查当前余额。查询余额时，需要提供查询帐户的公钥：

```
coda client get-balance -public-key $CODA_PK
```

查询回复类似：

```
Balance: 50 coda
```

一旦您对创建地址以及发送和接收Coda熟练后，我们就可以进入Coda网络真正独特的部分-[参与共识并帮助压缩区块链](https://codaprotocol.com /docs/node-operator)。

\*_Testnet积分仅用于跟踪对Testnet的贡献情况，Testnet积分没有现金或其他货币价值。 Testnet点不可转让，不可兑换或交换任何加密货币或数字资产。 我们可以随时修改或取消Testnet积分。_
