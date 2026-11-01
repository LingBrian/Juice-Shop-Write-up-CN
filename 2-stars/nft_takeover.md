# Juice-Shop Write-up：NFT Takeover

## 挑战概述

**Title:** NFT Takeover\
**Category:** Sensitive Data Exposure\
**Difficulty:** ⭐⭐ (2/6)

“NFT Takeover”挑战利用用户反馈中意外暴露的敏感信息，获取含有官方 Soul Bound Token (NFT) 的数字钱包。

## 使用的工具

- **Web Browser**：用于导航至指定页面并与 Web 应用交互。
- **Mnemonic Converter**：用于将助记词转换为访问区块链钱包所需的私钥。

## 方法与解决方案

### 找到 NFT 访问入口

1. **前往 NFT 页面**：
   - 检查应用的 JavaScript 文件，查找与“NFT”有关的内容，由此发现 `/juicy-nft` 路径。

   <img src="../assets/difficulty2/nft_takeover_1.png" alt="path" width="500px">

   - 访问 `http://127.0.0.1:3000/#/juicy-nft` 后，页面要求输入私钥以访问 NFT 钱包。

   <img src="../assets/difficulty2/nft_takeover_2.png" alt="nft page" width="500px">

### 利用暴露的敏感信息

2. **从用户反馈中提取助记词**：
   - 发现一条用户反馈评论意外泄露了助记词：“purpose betray marriage blame crunch monitor spin slide donate sport lift clutch.”

   <img src="../assets/difficulty2/nft_takeover_3.png" alt="admin panel" width="500px">

   - 判断该助记词可能与 `/juicy-nft` 页面所需的私钥有关。

3. **将助记词转换为私钥**：
   - 使用 Mnemonic converter 工具，将助记词转换为对应的私钥。
   - 得到私钥：`0x5bcc3e9d38baa06e7bfaab80ae5957bbe8ef059e640311d7d6d465e6bc948e3e`。

### 访问 NFT 钱包

4. **身份验证并接管 NFT**：
   - 在 NFT 页面输入推导出的私钥，成功验证并访问含有 Soul Bound Token 的钱包。

### 解决方案说明

本挑战通过发现并利用用户在反馈区域意外泄露助记词的安全疏漏而完成。将助记词转换为私钥后，即可未授权访问 NFT 钱包。

## 修复建议

为防止此类安全漏洞：

- **安全处理信息**：确保加密密钥或助记词等敏感信息不会以明文形式存储或传输到用户可访问的区域。
- **输入验证与净化**：对所有用户提交的内容实施严格的输入验证和净化，避免敏感数据意外暴露。
- **监控与过滤**：部署监控工具，检测并提醒潜在的敏感信息暴露，并使用自动过滤机制对这类数据进行遮蔽或阻止其显示。
