# Harvest.Finance

### 报道

{% embed url="https://mp.weixin.qq.com/s/Qhh4c70Nmi6rs_JnNJXKiQ" %}

{% embed url="https://www.8btc.com/article/661998" %}

### 攻击流程

1. 闪电贷借贷得到大笔USDC和USDT；
2. 利用借贷所得USDT通过Curve转换为USDC，提高USDC价格 ；
3. 将获得的USDC存入Harvest.Finance项目的USDC储藏室(vault)中, 同时Harvest.Finance会为该存入的行为攻击者铸造一定数目的fUSDC(铸造的数目受Curve影响)；
4. 将初始借贷所得的USDC通过Curve转换为USDT，提高USDT的价格，同时USDC价格降低 ；
5. 最终攻击者将持有额所有fUSDC转换回USDC，此时因为Curve中的USDC价格降低，导致影响了兑换回USDC的数目增加。

### 细节

和其他套利经济攻击一样，此次攻击起源于一笔巨额闪电贷。攻击者通过多次操纵 Curve Y 池的价格，以耗尽Harvest 的 fUSDT、fUSDC 池的资金。攻击者随后将资金转换为 renBTC 并套现。像其他闪电贷攻击操作一样，攻击者动作迅速，没有给平台反应的时间，连续 7 分钟端到端地进行攻击。攻击者以 USDT 和 USDC 的形式向开发者退回 2478549.94 美元 攻击原理暂时没有报道，但是属于套利攻击，非defi不一致情况。

### 总结

此次攻击主要是 Harvest Finance 的 fToken(fUSDC、fUSDT...) 在铸币时采用的是 Curve y池中的报价(即使用 Curve 作为喂价来源)，导致攻击者可以通过巨额兑换操控外部的价格来控制 Harvest Finance 中 fToken 的铸币数量，从而使攻击者有利可图。
