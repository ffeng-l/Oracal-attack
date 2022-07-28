# Cheese Bank

### 报道

[CheeseBank攻击报告](https://blog.csdn.net/PeckShield/article/details/109759331?ops\_request\_misc=%257B%2522request%255Fid%2522%253A%2522161580893616780261991470%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D\&request\_id=161580893616780261991470\&biz\_id=0\&utm\_medium=distribute.pc\_search\_result.none-task-blog-2\~all\~baidu\_landing\_v2\~default-1-109759331.first\_rank\_v2\_pc\_rank\_v29\&utm\_term=cheese+bank)

### 攻击过程

1、攻击者首先通过 dYdX 闪电贷贷出 21,000 枚ETH

2、在 UniswapV2 中将 50 枚WETH 兑换为 107,000 枚CHEESE (Cheese Bank 通证），此时 CHEESE 的单价为 0.00047 ETH/枚、

3、攻击者在 UniswapV2 中将 20,000 枚ETH 兑换为 288,000 枚CHEESE

4、通过提高到UNI\_V2 LP，换取了USDC 、 USDT 和 DA，并偿还贷款。

### 攻击分析

攻击者主要通过第三步来操纵利率，这一步会影响 UniswapV2 上 CHEESE 池子的平衡，进而抬高 CHEESE 的价格，此时 CHEESE 的单价为 0.069 ETH/枚（相较于此前 0.00047 ETH/枚的价格提高了145倍），攻击者剩余 871 枚ETH（包含交易手续费）。CHEESE 单价的提高能够有效地提升抵押在 Cheese Bank 中 UNI\_V2 LP 的价值，帮助攻击者提高在 Cheese Bank 中借贷加密资产的额度

![](<../.gitbook/assets/image (4).png>)

在这一步中，攻击者通过重置喂价预言机来操纵 UNI\_V2 LP 凭证的价格，从下列公式可知，UNI\_V2 LP 的价格受到流动池中 WETH 的数量和 UNI\_V2-USDT-ETH 池中 ETH价格的影响，即 WETH_2_ETH的价格=LP总价，LP单价=LP总价/LP总供应量，当攻击者将增加池中 WETH 的数量时，UNI\_V2 LP 凭证的价格也会上涨。

### 总结

攻击者通过贷款的方式，操作流动性币池中的汇率以此来获取利润
