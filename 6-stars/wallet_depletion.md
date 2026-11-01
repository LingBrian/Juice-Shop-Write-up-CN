# Juice-Shop Write-up: 钱包余额耗尽

## 挑战概述

**标题：** Wallet Depletion  
**类别：** 密码学问题  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战要求通过拦截和操纵区块链交易请求，耗尽或操纵 Juice Shop 应用中模拟加密货币钱包的余额。

## 重要说明

**_该挑战尚未完善。如果你找到妥善完成挑战的方法，欢迎提交 pull request。_**

## 使用工具

- **Web 浏览器：** 用于操作 Juice Shop 钱包页面。
- **Burp Suite：** 用于拦截和操纵 Juice Shop 发出和接收的 HTTP 请求。

## 方法与解题过程

### 步骤 1：探索钱包功能

我先访问 Juice Shop 网站的钱包页面：

<img src="../assets/difficulty6/wallet_depletion_1.png" alt="page wallet" width="500px">

我通过界面存入少量 ETH（0.001 ETH），测试钱包功能：

<img src="../assets/difficulty6/wallet_depletion_2.png" alt="filled wallet" width="500px">

### 步骤 2：拦截提款请求

使用 Burp Suite 拦截尝试从钱包提取 ETH 时发送的网络请求。请求中包含十六进制格式编码的交易数据，表明它与区块链技术交互，很可能是调用 Ethereum 智能合约。

<img src="../assets/difficulty6/wallet_depletion_3.png" alt="transfer request" width="500px">

### 请求中的关键观察

- **函数标识符：** 交易数据字段的第一部分（`0x2e1a7d4d`）标识调用的智能合约函数。该 ID 是函数签名的哈希片段。
- **参数：** 函数标识符之后的部分是表示待提取 wei 数量的十六进制数值。

### 步骤 3：解码并操纵请求

拦截到的请求包含一个表示 0.001 ETH 的 wei 数量参数：

- **十六进制值：** `0x38d7ea4c68000`
- **十进制值：** `1,000,000,000,000,000` wei
- **换算为 Ether：** `0.001 ETH`

为了测试功能边界，我修改参数以增加提款金额。例如，我重新计算 0.1 ETH 对应的十六进制值，并据此修改请求。

### 步骤 4：重新发送修改后的请求

将期望的提款金额（0.1 ETH）编码为交易数据所需的十六进制格式后，我更新请求并使用 Burp Suite 重新发送。

使用的载荷如下：
```
"data":"0x2e1a7d4d000000000000000000000000000000000000000000000000016345785d8a0000"
```

### 遇到的问题

尝试提取 0.1 ETH 时出现错误，可能是因为余额不足或存在防止未授权提款金额的保护措施。具体错误指出交易可能因“UNPREDICTABLE_GAS_LIMIT”而失败。

<img src="../assets/difficulty6/wallet_depletion_4.png" alt="transfer request" width="500px">

## 解题说明

本挑战展示了加密货币交易中客户端控制所带来的风险。如果没有在服务端充分校验，攻击者理论上可以拦截并操纵交易数据，发起未授权操作。

## 修复建议

- **服务端验证：** 始终在服务端验证交易请求，确保其符合所有必要条件（例如参数正确、余额充足）。
