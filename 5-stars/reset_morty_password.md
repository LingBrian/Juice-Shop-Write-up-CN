# Juice-Shop Write-up: 重置 Morty 的密码

## 挑战概述

**标题：** Reset Morty's Password\
**类别：** 安全配置错误\
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求通过回答安全问题“你最喜欢的宠物叫什么？”来重置 Morty 的账户密码（morty@juice-sh.op）。

## 使用工具

- **自定义 Leet Speak 脚本**：用于生成名称“Snuffles”和“SnowBall”的所有 leet 变体。[脚本见此处](https://gist.github.com/philly-vanilly/70cd34a7686e4bb75b08d3caa1f6a820)。
- **HTTP 请求操纵工具**：用于自动发送请求并修改 HTTP 标头，以绕过安全措施。

## 方法与解题过程

### 准备与初步分析

1. **理解挑战：**
   - 提示如下：

   <img src="../assets/difficulty5/reset_morty_password_1.png" alt="hints for this challenge" width="500px">

   - 分析提供的提示和粉丝页面详情，确认安全问题的可能答案。

   <img src="../assets/difficulty5/reset_morty_password_2.png" alt="fanpage" width="500px">

   - 根据提示，候选答案为“Snuffles”或“SnowBall”。挑战还要求使用少于 10 个字符、不含特殊字符的混淆密码，并通过操纵 `X-Forwarded-For` 标头绕过防暴力破解机制。

### 生成字典

2. **生成 Leet Speak 变体：**
   - 使用自定义脚本，根据“Snuffles”和“SnowBall”生成包含 leet speak 替换的候选密码字典。

### 设置暴力破解

3. **配置暴力破解工具：**
   - 启动工具遍历生成的字典。每次尝试密码时都修改 `X-Forwarded-For` 标头，以避免被暴力破解机制检测。

### 确认解决方案

4. **验证：**
   - 成功重置后，使用新密码登录账户，确认能够访问。

### 解题说明

通过生成 leet speak 字典，并操纵标头绕过基于 IP 的安全措施，成功重置了密码。这种方法揭示了密码重置功能可能存在的弱点，以及其易受暴力破解和标头伪造攻击的风险。

## 修复建议

为增强对此类攻击的防护，应用应：

- **保护账户恢复流程：** 增加额外验证步骤，防止未经授权的访问。
- **移除安全问题：** 安全问题并不可靠，可改为向申请重置密码的用户发送电子邮件。
