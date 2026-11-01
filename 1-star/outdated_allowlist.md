# Juice-Shop Write-up：Outdated Allowlist

## 挑战概述

**Title:** Outdated Allowlist\
**Category:** Unvalidated Redirects\
**Difficulty:** ⭐ (1/6)

“Outdated Allowlist”挑战涉及利用未验证的重定向，以及 Web 应用重定向机制中已过时的允许地址列表。任务是利用允许列表的弱点操纵 URL，将用户重定向到未经批准或非预期的加密货币地址。

## 使用的工具

- **Web Browser**：用于与应用交互并分析 URL 重定向。
- **Developer Tools**：用于检查 JavaScript 代码和网络流量，以了解重定向的处理方式。

## 方法

### 初步分析

与挑战交互时，可以看到 Web 应用提供了加密货币交易二维码。`main.js` 文件中负责管理这些二维码的 JavaScript 代码显示，特定加密货币地址被硬编码，并且有一个重定向函数会根据用户的选择指向相应地址。

<img src="../assets/difficulty1/outdated_allowlist_1.png" alt="allow list" width="500px">

```javascript
showBitcoinQrCode() {
    this.dialog.open({
        data: {
            data: "bitcoin:1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm",
            url: "./redirect?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm",
            address: "1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm",
            title: "TITLE_BITCOIN_ADDRESS"
        }
    })
}
```

### 挑战解决步骤

1. **操纵 URL**：
   - 修改 `redirect` URL 参数，使其指向允许列表中的新地址。
   - 访问修改后的 URL：`http://127.0.0.1:3000/redirect?to=https://www.blockchain.com/explorer/addresses/btc/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm`

### 解决方案说明

本挑战通过利用过时的允许列表而完成。

## 修复建议

为防止真实应用中出现此类问题，请考虑以下建议：

- **定期更新并验证允许列表**：确保定期更新允许列表。
