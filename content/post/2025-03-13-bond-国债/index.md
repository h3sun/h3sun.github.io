---
title: 投资工具-Bond
summary: 美国国债 (US Treasury) 基础知识
date: 2025-03-13
authors:
  - admin
tags:
  - finance
---

## Treasury 分类

### T-Bill, Treasury Bill

1. 短期：4 week，8 week，12 week， etc。
2. 没有 coupon（票面利息），只有折价发行，折价率，本质是 interest 而不是 capital gain。
3. 利息是 recurring gain，税率低。

### T-Note, Treasury Note

1. 中期：2-10 年。
2. 开始有票面利息，6 个月付一次。
3. 有二级市场，流动性好。

- 一级市场：primary market, 融资的公司直接卖给你，价格事先通过投行（investment banking）定好，IPO。Bond 等于是政府定价。
- 二级市场：secondary market，投资人与投资人之间互相的买卖，价格通过互相买卖的市场决定。

### T-Bond, Treasury Bond

1. 长期：10-30 年期。
2. 6 个月付一次利息。
3. 花一定价格购买 face value 的 Bond，会有利息，到期以后你会拿到 face value 的钱

- 收益率曲线倒挂：变成钟形，长期反而低。（yield 叫做实际收益率）可能会出现经济危机。

### TIPS, Treasury Inflation Protected Securities

1. 长期：5 年，10 年，30 年。
2. 6 个月付一次，本金会随着通货膨胀率调整。通货膨胀时候拿的多，相反通货紧缩拿的少。

### FRN, Floating Rate Note

1. 中期：2 年期。
2. 6 个月付一次，利率根据 13 周 T-Bills 利率上下浮动。

## 常识

1. 国债免 state tax，local tax，但是 federal tax 是不能免除的。
2. bank/broker 购买和 treasury direct 购买的区别：bank/broker 可以用 non-competitive billing 和 competitive billing 购买。非竞争性竞价肯定可以买到，但是买之前不能准确知道以什么价格买，竞争性竞价规定我以什么价格购买。

## reference:

1. https://www.youtube.com/watch?v=kj87yxWJ9qQ&ab_channel=%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E6%9D%A5%E8%B0%88%E9%92%B1
2. https://www.youtube.com/watch?v=HdcL9J1cp8w&ab_channel=%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E6%9D%A5%E8%B0%88%E9%92%B1

## Q&A

### What are Series EE and Series I savings bond?

1. Savings Bond 就是一个从风险和收益率和定期存款非常像的一个产品，定期存款 CD 在美国是商业银行发行的，Savings Bond 是美国政府发行的。
2. Savings Bond 分 Series EE 和 Series I。
   - Series EE 是折价发行，甚至是半价，20 年就能赎回，1 年内赎回就丧失所有钱，5 年内丧失最后三个月利息（early withdraw penalty），没有二级市场，所以更像 Saving 而不是储蓄。首先利率很低，其次即使翻倍也有通货膨胀的因素，(1.03)^20 = 1.8, (1.04)^20 = 2.1
   - Series I 最大不同在于它并不是折价发行，面值发行，赋予利息，利率是 CPI correlated 的。Series I 有 floor，下限。
3. Series H Bond，这种在 04 年以后不发行了。
4. tax
   - 只是 federal tax，no local/state tax。
   - 独特之处在于你可以选择每年拿利息交税，也可以前 20 年不拿利息，最后 20 年交一笔税，选了之后不能变了，如果不止一只 savings bond，每个 bond 选择得相同。
   - 如果你用于 education，可以用来学费，qualified intuition expense，书本住宿还是不能免税，结婚以后得联合 file tax。

### C of I, Zero Interest Certificate of Indebedness

1. Zero Interest 不等于 0 coupon(没有票面利息的债券)，折价发行会有利息在里面
2. 就是没有利息的理财产品？为什么不存在银行拿 interest 或者买 bond？

### Strips，Separate Trading of Registered Interest and Principal of Securities

1. 把一个 T-Note，T-Bond 给拆成本金和利息，你只拿到利息或者只拿到本金

### 没有看到国债有 fdic 保险

1. 基本没有风险，不用考虑这个问题。

### Secondary Issue Bond

1. 二级市场购买的 Bond。
