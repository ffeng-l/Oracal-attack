# YFI

## 报道

[千万美元损失背后的闪电贷攻击——yearn finance 被黑简析](https://mp.weixin.qq.com/s/\_0Q3-rXBRGxViZbxH6Wsbg)

[DeFi项目YFI闪电贷攻击事件始末](https://www.jinse.com/news/blockchain/997890.html)

## 攻击步骤

1. 攻击者首先从 dYdX 和 AAVE 中使用闪电贷借出大量的 `ETH`
2. 攻击者使用从第 1 步借出的 `ETH` 在 Compound 中借出 `DAI` 和 `USDC`
3. 攻击者将第 2 步中的所有 `USDC` 和 大部分的 `DAI` 存入到 Curve DAI/USDC/USDT 池中，这个时候由于攻击者存入流动性巨大，其实已经控制 Curve DAI/USDC/USDT 的大部分流动性
4. 攻击者从 Curve 池中取出一定量的 `USDT`，使 DAI/USDT/USDC 的比例失衡，及 DAI/ (USDT\&USDC) 贬值
5. 攻击者第 3 步将剩余的 `DAI` 充值进 yearn DAI 策略池中，接着调用 yearn DAI 策略池的 earn 函数，将充值的 `DAI` 以失衡的比例转入 Curve DAI/USDT/USDC 池中，同时 yearn DAI 策略池将获得一定量的 `3CRV` 代币
6. 攻击者将第 4 步取走的 `USDT` 重新存入 Curve DAI/USDT/USDC 池中，使 DAI/USDT/USDC 的比例恢复
7. 攻击者触发 yearn DAI 策略池的 `withdraw` 函数，由于 yearn DAI 策略池存入时用的是失衡的比例，现在使用正常的比例提现，`DAI` 在池中的占比降低，导致同等数量的 `3CRV` 代币能取回的 `DAI` 的数量变少。这部分少取回的代币留在了 Curve DAI/USDC/USDT 池中
8. 由于第 3 步中攻击者已经持有了 Curve DAI/USDC/USDT 池中大部分的流动性，导致 yearn DAI 策略池未能取回的 `DAI` 将大部分分给了攻击者
9. 重复上述 3-8 步骤 5 次，并归还闪电贷，完成获利。

## 总结

根源不是闪电贷，而是脆弱的价格机制

没有出现不一致
