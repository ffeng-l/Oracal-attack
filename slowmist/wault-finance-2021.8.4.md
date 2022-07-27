# Wault Finance（2021.8.4）

### 分析

{% embed url="https://mp.weixin.qq.com/s/aFSnSDPk4RYlcKz6Qr_CmQ" %}

### 攻击原因

WUSDMaster 合约的 stake 函数中，会在质押的时候 1:1 兑换 BSC\_USDT 和 WUSD，但是它还进行了一次 swap 操作，该swap 操作会额外导致WaultSwapPair(BSC\_USDT-WEX) 的池子中的代币失衡，从而形成套利空间。

![](<../.gitbook/assets/640 (2) (1).webp>)

### 攻击流程

#### 第一阶段：通过闪电贷获得初始攻击资金

1. 在 WaultSwapPair (BSC\_BUSD-WUSD) 中通过闪电贷借了 16,839,004 枚 WUSD。
2. 调用 WUSDMaster 合约中的赎回 (redeem) 函数，将闪电贷借到的 WUSD 燃烧掉，换成 BSC\_USDT 和 WEX。
3. 在PancakePair (WBNB-BSC\_USDT) 中通过闪电贷借了 40,000,000 枚BSC\_USDT。
4. 将借到的 23,000,000 枚 BSC\_USDT 在 WaultSwapPair (BSC\_USDT-WEX) 中换成了 WEX。

**第二阶段：使 BSC\_USDT-WEX 池子失衡形成套利空间**

1. 多次 (68 次) 调用 WUSDMaster 合约中的质押(stake)函数，stake 函数会swap操作，将质押一部分的 BSC\_USDT 换成 WEX，这样就会使得 WaultSwapPair (BSC\_USDT-WEX) 池子的 WEX 数量减少，价值变高。
2. 多次 stake 之后 BSC\_USDT-WEX 池子中，BSC\_USDT 数量多，WEX 数量少，形成套利空间。
3. 且攻击者每次调用 stake 都会以 1:1 的兑换方式使用 BSC\_USDT 兑换 WUSD，所以攻击者在这一步的兑换可以无损的情况下就额外的将 BSC\_USDT-WEX 池子打失衡了。

#### 第三阶段：进行套利，并偿还闪电贷

1. 攻击者将第一阶段准备好的 WEX 在已经失衡的 BSC\_USDT-WEX 池子中进行兑换，就可以换出更多的 BSC\_USDT。
2. 攻击者将多次 (68 次) 调用 stake 函数所得到的 WUSD 在偿还闪电贷之后，剩余 110,326 枚 WUSD 通过 WaultSwapPair (BSC\_BUSD-WUSD) 换成了BSC\_BUSD。
3. 将所得到的 BSC\_USDT 和 BSC\_BUSD 还完闪电贷后换成了 BEP\_ETH。\\
