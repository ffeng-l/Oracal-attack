# ChainSwap（2021.7.11）

### 报道

2021年7月11凌晨，去中心化跨链资产桥梁项目ChainSwap遭到攻击，部署于该跨链桥上的20+个项目遭到攻击，损失超过800万美元，是DeFi发展史迄今为止发生的最严重的攻击事件。2021年7月11凌晨，去中心化跨链资产桥梁项目ChainSwap遭到攻击，部署于该跨链桥上的20+个项目遭到攻击，损失超过800万美元，是DeFi发展史迄今为止发生的最严重的攻击事件。

### 攻击原理

ChainSwap会为该项目承接的每一个代币创建一个TokenMapped合约来代表该代币在链上的映射。攻击者的入口即为TokenMapped合约的receive方法。为了跨链地证明交易，ChainSwap设置了验证者来进行交易验证，这些验证者被命名为签名者（Signatory）。且为每一个验证者限定了配额（Quota），也就是说每个验证者验证的交易额在一段时间内是有限制的。但验证者会有一定的初始配额，且该配额会随着时间累积。

![](<../.gitbook/assets/image (4).png>)

上图展示了receive函数的全部代码。正常情况下，用户在其他链上向ChainSwap发送某种Token之后，交易经过签名者验证，用户可以将验证信息发给以太坊主链上的ChainSwap合约，后者会在验证之后将Token转账给给用户。receive实现的正是这样一个流程。但是入口函数receive并没有对签名者（Signatory）的身份进行验证，Signatory参数的存在仅仅用于配合ecrecover函数验证消息是否确实是由Signatory进行签名。

简要来说，receive函数实现的整个过程，都没有对传入的Signatory的合法性进行检查。因此攻击者只需要随机生成一个地址并生成对应的签名，即可骗过ChainSwap，自己为自己提供签名。同时，由于receive函数的内部函数\_decreaseAuthQuota中的authQuotaOf在实现逻辑上的错误，在Signatory不合法时会返回一个非常大的值，导致了这次攻击事件的发生。
