# Value DeFi

### 报道

{% embed url="https://blog.peckshield.com/2020/11/15/valuedefi/" %}

### 结论

没有发生`defi`的数据结构不一致，这是一起典型套利攻击。

### 概述

由于`value Defi`使用基于`AMM`的`oracle`（即`Curve`）来测量资产价格的协议中有错误。攻击者利用闪电贷，在`Curve`中扰乱`value Defi`所依赖的`AMM`中测量资产的价格，从中套利。

### 细节

#### 正常步骤

步骤1 ： 利用闪电贷从`Aave`借款`80K ETH`

步骤2 ：在`uniswapV2`中将`WETH`转换为`DAI`

步骤3 ：在`uniswapV2`中将`ETH`转换为`USDT`

步骤4 ：在`value DeFi`中存入90M `DAI` ，由此获得24.9M `poolToken`（交给攻击者）和新的24.956M `3crv`（`value DeFi`保管）

#### 扰乱AMM价格机制，价格操纵

步骤5 ：在`Curve`中使用`DAI`交换大量`USDC`

步骤6 ：在`Curve`中使用`USDT`交换大量`USDC`

以上两个步骤会导致Curve中的3pool不平衡，使得USDC价格虚高

#### 攻击

步骤7 ： 燃烧24.9M `poolToken`，此时由于`AMM`的错误评估，攻击者会拿走33.089M `3crv`，而不是24.956M `3crv`

以上一个步骤将导致defi财产损失

#### 恢复AMM正常价格

步骤8 ： 在`Curve`中使用`USDC`交换`USDT`

步骤9 ： 在`Curve`中使用`USDC`交换`DAI`

#### 套现

步骤10 ： 在`value DeFi`中燃烧33.089M `3crv`，获得33.11M `DAI`，从而套利
