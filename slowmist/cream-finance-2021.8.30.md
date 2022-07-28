# Cream Finance（2021.8.30）

### 分析

{% embed url="https://mp.weixin.qq.com/s/a9s61_u30f4X8310A952_Q" %}

### 攻击原因

Cream 借贷模型与 AMP 代币不兼容导致的。由于 AMP 代币转账时会使用钩子函数回调目标地址，且 Cream cToken 合约是在借贷转账后才记录借贷数量，最终造成了超额借贷的问题。

### 攻击细节

1\. 攻击者通过闪电贷获得抵押向crAMP合约请求借贷，借出AMP

2\. 借贷时，平台通过doTransferOut函数将AMP代币转给攻击者，并记录

3\. AMP代币在transfer时会调用钩子（\_callPostTransferHooks）函数回调攻击者合约的tokensReceived函数

4\. 该函数计再次向crETH合约请求借贷，借出ETH

5\. 由于是两次向不同合约借贷，防重入修饰器时效，而borrow是在平台借出代币后再进行记录，导致可以重入攻击

6\. 并最终使用合约对爆仓合约清算完成攻击。
