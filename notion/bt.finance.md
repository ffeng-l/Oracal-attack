# BT.Finance

### 攻击报道

[https://www.jinse.com/news/blockchain/998140.html](https://www.jinse.com/news/blockchain/998140.html)

### 攻击流程

1. 攻击者首先从 dydx 中使用闪电贷借出约100000个[ETH](https://link.jinse.com/s/K5S5A2?coin\_keyword=1\&coin=ethereum)。
2. 攻击者将大约57000个ETH存到 Curve sETH池中。
3. 攻击者从 Curve sETH池中取出sETH，由于存入了大量的ETH，导致sETH的价格上升，此时攻击者取出了约35000个sETH。
4. 攻击者将大约4340个ETH存入bt.finance ETH策略池中。
5. 攻击者调用earn函数。
6. 攻击者将步骤3中取出的sETH全部存入Curve sETH池中并取出ETH，最后触发 bt.finance ETH策略池的withdraw函数，取出全部存入该池中的ETH。
7. 重复上述 2-5 步骤 5 次，并归还闪电贷，完成获利。

### 总结

操纵利率，实现套利
