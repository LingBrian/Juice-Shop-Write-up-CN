# Juice-Shop Write-up: 伪造优惠券

## 挑战概述

**标题：** Forged Coupon  
**类别：** 密码学  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战要求通过逆向分析找出优惠券的生成算法，并伪造可在 Juice Shop 应用中使用的有效优惠券。

## 使用工具

- **Web 浏览器：** 用于与 Juice Shop 交互。
- **CyberChef：** 用于熵分析，评估优惠券的随机性。
- **Cryptii：** 用于确认优惠券编码方式的在线编码/解码工具。

## 方法与解题过程

### 步骤 1：分析现有优惠券

最初，我检查了在先前挑战中获取的一组优惠券，这些优惠券暗示其中可能使用了密码学方法：

```
n<MibgC7sn
mNYS#gC7sn
o*IVigC7sn
k#pDlgC7sn
o*I]pgC7sn
n(XRvgC7sn
n(XLtgC7sn
k#*AfgC7sn
q:<IqgC7sn
pEw8ogC7sn
pes[BgC7sn
```

我打算寻找规律来重现这些优惠券，或对其解码以了解其生成方式。

### 步骤 2：识别编码库

由于我推测这些优惠券由某种算法生成，于是使用在线密码算法检测工具查找所用算法，但没有找到结果。这可能意味着使用了自定义算法，或一种不太常见的算法。

随后，我检查了此前从 FTP 服务器获取的 `package.json`，寻找与密码学或编码有关的库。逐一分析软件包后，我认为 Z85 编码库可能是候选项，因为它在 npm 上的说明与此相符：

<img src="../assets/difficulty6/forged_coupon_4.png" alt="npm package z85" width="500px">

### 步骤 3：解码优惠券

使用 Cryptii 在线工具，通过 Z85 算法解码优惠券，发现其格式一致：`{月份的3个字母}{年份的2位数字}-{折扣百分比}`。

<img src="../assets/difficulty6/forged_coupon_5.png" alt="decrypting coupons" width="500px">

### 步骤 4：生成伪造优惠券

使用相同的 Z85 编码，为当前日期和指定折扣生成新的优惠券代码：

- **原始输入：** `MAY24-80`
- **编码结果：** `o*I]li4$`

### 步骤 5：测试伪造优惠券

在 Juice Shop 中使用新伪造的优惠券完成交易。系统验证通过，说明伪造优惠券与合法优惠券无法区分。

<img src="../assets/difficulty4/ephemeral_accountant_2.png" alt="decrypting coupons" width="500px">

## 解题说明

由于使用了可预测且可逆的编码方法（Z85），又缺少额外校验或混淆，因此可以轻易生成有效优惠券。

## 修复建议

- **使用安全随机数：** 使用密码学安全的方法生成优惠券代码，使其难以预测或逆向还原。
- **限制优惠券算法的可见性：** 避免在 `package.json` 等客户端可访问文件中暴露优惠券算法或相关库，以免攻击者据此推断后端处理方式。
