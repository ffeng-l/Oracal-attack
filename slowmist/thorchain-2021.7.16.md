# THORChain（2021.7.16）

### 分析

{% embed url="https://mp.weixin.qq.com/s/6DBtGGXtUs9Gcy4v0IxU1A" %}

### 攻击原因

THORChain 在获取用户充值金额时，使用交易里的 msg.value 值覆盖了正确的 Deposit event 中的 amount 值，攻击者利用该漏洞进行攻击。

### 攻击细节

攻击者在攻击合约中调用了 THORChain Router 合约的 deposit 方法，传递的 amount 参数是 0。然后攻击者地址发起了一笔调用攻击合约的交易，设置交易的 value(msg.value) 不为 0，由于 THORChain 代码上的缺陷，在获取用户充值金额时，使用交易里的 msg.value 值覆盖了正确的 Deposit event 中的 amount 值，导致了 “空手套白狼” 的结果。
