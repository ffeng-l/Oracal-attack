---
description: 时间：07:41:39 PM +UTC, May 7, 2021  性质:代码漏洞
---

# ValueDeFi（2021.5.16）

### 分析

{% embed url="https://blog.peckshield.com/2021/05/08/ValueDeFi" %}

### 攻击原因

ValueDeFi对非50/50比例资金池的代码逻辑中流动性注入存在变量检查漏洞，导致攻击者可以用不符合要求的值来跳过变量检查，从而利用漏洞套利。

攻击者共攻击了9个非50/50的资金池，盗取了15,000 BNB、2,700 FARM、1,700 BASv2、8,500万 BDO、68,300 BUSD、41,400 MDG、945,000 VBOND、1,200 万 BAC、11,000 FIRO共计1100万美元资产。

### 细节

ValueDefi fork了 uniswap的代码，然而uniswap仅支持50/50比例添加流动性操作，并且有充分的安全检查，因此50/50比例资金池未遭到攻击。而ValueDeFi在非50/50资金池中，效仿 Bancor允许用戶按不同价值比例投入到资金池中，在这个过程中需要检查几个静态值（`reserve0`，`reserve1`，`weight0`，`weight1`，`balance0Adjusted`，`balanced1Adjusted`）是否符合公式要求:

$$reserve0 ^{\frac{weight0}{50}} \times reserve1 ^{\frac{weight1}{50}} \le balance0Adjusted ^{\frac{weight0}{50}} \times balance1Adjusted ^{\frac{weight1}{50}}$$

这个检查过程在ensureConstantValue（）中：

![](<../.gitbook/assets/image (9).png>)

该权重乘积用power(`baseN`, `baseD`, `expN`, `expD`) ，计算公式为$$\frac{baseN}{baseD}^{\frac{expN}{expD}} \times 2 ^{precision}$$，其中power（）要求`baseN ≥ baseD` ，并且函数内部也没有设置安全检查来中断不满足的情况。

攻击者利用精心设计的输入，使得`balance1Adjusted < reserve1`, 破坏power（）的执行，但是函数仍完整的进行下去并且通过了验证。

利用这个漏洞攻击者可以可以从资金池中用0.05个WBNB，换出1 WEI vBSWAP和3,543个WBNB。不断进行如上操作可以不断将资金池搬空。

###
