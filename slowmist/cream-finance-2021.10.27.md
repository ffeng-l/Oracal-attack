# Cream Finance（2021.10.27）

### 报道

Cream的预言机价格计算使用与yUSD的totalAsset有关。通过闪电贷获取大量资金后，利用合约设计缺陷，大幅改变价格导致获利。向yUSD合约直接转账时，不会更新Debt，从而使totalAsset增大，从而使得yUSD价格变高，可以从Cream中借出更多的资金

### 分析

{% embed url="https://www.jinse.com/blockchain/1170026.html" %}

{% embed url="https://mp.weixin.qq.com/s/ykz63ZtfbObwRs3UTE3toQ" %}
、
{% endembed %}

### 攻击原因

Cream对抵押物价格获取存在缺陷，导致用户可操控抵押物价格。

### 攻击细节

1\. 在Cream中通过函数BorrowAllowed函数对比cToken的资产价值和节代的Tokne价值和来判断用户是否可以借贷。

2\. 在函数pricePerShare中，直接返回\_shareValue作为抵押物的价值。

而 \_shareValue = \_totalAssets / share数量（self.totalSupply）计算出。

3．通过操纵\_totalAssets即可拉高单个share价格，使抵押物价值变高，以借出更多代币

4． 而\_totalAssets = 合约上的代币+策略池中的数额。攻击者向合约注入代币即可拉高\_totalAssets.
