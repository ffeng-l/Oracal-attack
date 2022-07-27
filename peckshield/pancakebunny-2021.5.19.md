---
description: 时间：May-19-2021 10:34:28 PM +UTC 性质：代码漏洞
---

# PancakeBunny（2021.5.19）

### 分析

{% embed url="https://blog.peckshield.com/2021/05/20/bunny" %}

### 攻击原因

铸币模型漏洞，使得攻击者可以通过精心的设计，作为LPs获得大量的铸币奖励套利。

### 攻击流程

1. 获取8笔不同的闪电贷款，前七笔来自PancakeSwap中WBNB的资金池，共换出232W左右WBNB，以及296万USDT来自Fortube银行。
2. 将296万USDT和7886 WBNB存入WBNB+BUSDT池作为流动资金，并铸造14.4LP代币作为回报。
3. 通过上述WBNB+BUSDT池将232万WBNB换成383万BUSDT，使池子有足够大的WBNB储备，控制BNB价格降低。
4. 调用getReward()，从VaultFlipToFlip索取奖励，攻击者获得697万BUNNY的奖励。
5. 将第1步中的flashloans返还给PancakeSwap池和Fortube。

### 细节

攻击者利用getrewar()调用中的代码逻辑漏洞来进行攻击。

在攻击流程第四步getreward（）中发生了如下交易转账：

* BUNNYMinterV2从USDT-BNB v2池中取出流动性——从池中取出296万USDT和7744枚BNB。
* 由于攻击第二步将价格控制降低，攻击者能够将296万USDT换成231万BNB。
* 一半的BNB被换成BUNNY，另一半的BNB和换来的BUNNY被添加到BNB-BUNNY池中。

大量的BNB被加入到BNB-BUNNY池中，这增加了BNB(reserve0)的数量。在之后计算要铸造的BUNNY数量时，这将被用来操纵 "valueInBNB "变量，以获取大量的BUNNY奖励代币。

get\_reward()中铸币相关逻辑如下：

![](https://image.3001.net/images/20210526/1622019057\_60ae0bf17af95a3037df3.png!small)

getReward方法中，会进一步调用 BunnyMinterV2合约中mintForV2方法来为调用者铸造Bunny代币奖励。

![](https://image.3001.net/images/20210526/1622019070\_60ae0bfe6f9e2821941f5.png!small)

mintForV2方法中，最终铸币的数量mintBunny是由valueInBNB计算而得，而valueInBNB变量是通过PriceCalculator中valueOfAsset得到。

![](https://image.3001.net/images/20210526/1622019092\_60ae0c147f95a405bbb21.png!small)

valueOfAsset()中，valueInBNB变量是通过amount和reserve0等计算得出，amount（mintForV2方法中传入的bunnyBNBAmount）值是一个非常大的值，池子由于攻击者添加的大量BNB，导致reserve0是一个非常大的数值，计算后valueInBNB变量值变大，最终mintBunny变大铸造了大量Bunny代币。
