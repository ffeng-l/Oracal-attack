# Akropolis（2020.11.12）

### 报道

2020年11月12日夜晚。DeFi聚合器Akropolis遭受攻击，黑客\[1]通过Flashloan（闪电贷）+ 重入的攻击方式，盗取了存储在Akropolis中价格超过2,000,000美元的数字资产。

### 攻击原理

由于Akropolis并没有对用户传递进来的token参数进行校验，加上deposit函数没有防重入，导致攻击者利用伪造的token contract重入deposit函数，从而达到偷天换日，鱼目混珠，使用少量的DAI换出大量的DAI的效果。
