# THORChain（2021.7.23）

### 分析

{% embed url="https://mp.weixin.qq.com/s/6DBtGGXtUs9Gcy4v0IxU1A" %}

### 攻击原因

利用的是 THORChain Router 合约中关于退款的逻辑缺陷。

具体细节未完全披露，TORNChain官方也仅仅提及调用了router合约的带有退款memo的returnVaultAssets函数。

![](../.gitbook/assets/PQ2Z]AC\_3H6\`%8IP$7S%\~VS.png)

### 攻击细节

攻击者部署了一个攻击合约，作为自己的 router，在攻击合约里调用 THORChain Router 合约。

![](<../.gitbook/assets/640 (6).webp>)

但不同的是，攻击者这次利用的是 THORChain Router 合约中关于退款的逻辑缺陷，攻击者调用 returnVaultAssets 函数并发送很少的 ETH，同时把攻击合约设置为 asgard。然后 THORChain Router 合约把 ETH 发送到 asgard 时，asgard 也就是攻击合约触发一个 deposit 事件，攻击者随意构造 asset 和 amount，同时构造一个不符合要求的 memo，使 THORChain 节点程序无法处理，然后按照程序设计就会进入到退款逻辑。
