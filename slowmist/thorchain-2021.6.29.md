# THORChain（2021.6.29）

### 分析

{% embed url="https://mp.weixin.qq.com/s?__biz=MzU4ODQ3NTM2OA%3D%3D&chksm=fddd6b37caaae2217e7a4922f958e25aa72318ac0477ca3208d70459cac3cca15dce8dedb704&idx=1&mid=2247488944&scene=21&sn=eddee62ef3599fe55d8e0066ae037d30#wechat_redirect" %}

### 攻击原因

由于 THORChain 代码上的逻辑漏洞，即当跨链充值的 ERC20 代币符号为 ETH 时，漏洞会导致充值的代币被识别为真正的以太币 ETH，进而可以成功的将假 ETH 兑换为其他的代币。攻击者通过部署了假币合约，可以完成跨链“假充值“。

### 攻击细节

在处理跨链充值事件时，调用了 **getAssetFromTokenAddress （）** 去获取代币信息，并传入了资产合约地址作为参数方法：

![](<../.gitbook/assets/640 (1) (1).webp>)

该方法调用了 **getTokenMeta（）** 去获取代币元数据，此时也传入了资产合约地址作为参数：

![](<../.gitbook/assets/640 (2).webp>)

在初始化代币时，默认赋予了代币符号为 **ETH**。如果传入合约地址对应的代币符号为 ETH，那么此处关于 **symbol** 的验证将被绕过，这也是攻击的漏洞关键点。

看到当代币地址在系统中不存在时，会从以太坊主链上去获取合约信息，并以获取到的 **symbol** 构建出新的代币：

\\

![](<../.gitbook/assets/640 (3) (1).webp>)

![](<../.gitbook/assets/640 (4).webp>)

![](<../.gitbook/assets/640 (5).webp>)

通过以上合约代码信息可以看出，当ERC20 代币符号为 **ETH**，那么将会出现逻辑错误，导致充值的代币被识别为真正的以太币 **ETH。**
