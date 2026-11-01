# Juice-Shop Write-up：Weird Crypto

## 挑战概述

**Title:** Weird Crypto\
**Category:** Cryptographic Issues\
**Difficulty:** ⭐⭐ (2/6)

“Weird Crypto”挑战考察识别并报告应用中使用已弃用加密方法的能力，具体关注密码哈希的不安全实现。

## 使用的工具

- **Web Browser**：用于检查身份验证令牌并提交漏洞报告。
- **Base64 Decoder**：用于解码经过 Base64 编码的身份验证令牌。

## 方法与解决方案

### 分析身份验证令牌

1. **检查令牌**：
   - 从浏览器的 cookie 存储中提取 JWT (JSON Web Token)。
   - 注意到 JWT payload 使用 Base64 编码。使用 Base64 decoder 查看其内容。

   <img src="../assets/difficulty2/weird_crypto_1.png" alt="base64 decode" width="500px">

   注：更简单的方法是将内容复制并粘贴到 jwt.io。

### 识别加密缺陷

2. **解码 JWT Payload**：
   - 解码 Base64 字符串以查看明文 payload，其中暴露了包括密码哈希在内的多个用户属性。
   - 密码哈希显示为 `"password":"0192023a7bbd73250516f069df18b500"`。

3. **分析哈希**：
   - 根据哈希的模式和长度，识别出它是 MD5 哈希。考虑到 MD5 已知的弱点以及它不符合当前安全最佳实践，因此将其确定为报告对象。

### 报告问题

4. **使用联系表单**：
   - 使用应用的联系表单，报告使用 MD5 对密码进行哈希这一不安全做法。

### 解决方案说明

本挑战通过识别应用使用了不安全且过时的加密方法（MD5）进行密码哈希而完成。解码 JWT 并分析哈希后确认了该方法，然后通过应用内的相应渠道报告问题，满足了挑战要求。

## 修复建议

为降低使用已弃用加密函数带来的风险：

- **升级哈希算法**：将 MD5 替换为更强的密码哈希算法，例如 SHA-256 或 bcrypt，以提高抵抗暴力破解的能力，避免哈希计算过快和碰撞攻击等问题。
- **添加 Salt**：为每个密码哈希使用唯一的 salt，防止使用预计算彩虹表，并提高破解难度。
