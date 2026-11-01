# Juice-Shop 攻略：允许列表绕过

## 挑战概述

**标题：** Allowlist Bypass\
**类别：** 访问控制失效\ 
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求绕过 OWASP Juice Shop 应用中 URL 重定向功能的允许列表机制。

## 使用的工具

- **浏览器**：用于测试不同的 URL 重定向并观察行为。
- **代码编辑器**：用于分析源代码，了解重定向机制。

## 方法与解法

### 步骤 1：探索重定向功能

1. **初步探索**：
   - 首先查看 `main.js`，了解应用如何管理和处理重定向。为此，我搜索了关键词“review”，发现重定向 URL 的基本格式为 `127.0.0.1:3000/redirect?to=targetURL`。

   <img src="../assets/difficulty4/allowlist_bypass_1.png" alt="redirect example" width="500px">

### 步骤 2：测试并了解允许列表

2. **测试不在允许列表中的 URL**：
   - 使用常见的外部 URL（google.com）测试重定向功能。该尝试被阻止，证实存在允许列表机制。

   <img src="../assets/difficulty4/allowlist_bypass_2.png" alt="not allowed to redirect error" width="500px">

   - 错误信息指出了源代码中允许列表检查的位置（`/juice-shop/build/routes/redirect.js`），为后续调查提供了可能的文件路径。

### 步骤 3：绕过允许列表

3. **初步绕过尝试**：
   - 我尝试了多种方法，包括在查询参数中加入已允许的 URL 再附加未允许的 URL、注入空字节，以及其他常见 URL 绕过技术，但均未成功。

### 步骤 4：改进绕过技巧

4. **成功绕过允许列表**：
   - 我改为构造一个会重定向到其他 URL 的个人网站，并推测验证逻辑可能只检查整个字符串中是否出现允许列表中的 URL。
   - 构造了 URL `https://estebanmagnon.com/redirect/?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm`，它先经过个人网站，再重定向到允许列表中的目标。
   - 使用格式 `127.0.0.1:3000/redirect?to=https://estebanmagnon.com/redirect/?to=https://blockchain.info/address/1AbKfgvw9psQ41NbLi8kufDQTezwG8DRZm` 测试后，成功绕过允许列表限制。

## 解法说明

此次绕过利用了机制只检查重定向查询参数中任意位置是否包含允许列表 URL 的缺陷。通过嵌套重定向，让请求先经过中间网站再跳转到允许的站点，欺骗了安全检查，使原本受限的 URL 得以通过。这说明仅进行部分 URL 验证可能导致复杂的绕过问题。

### 安全建议

- **验证完整 URL**：全面验证 URL，包括完整路径和参数，而非只匹配部分内容。
- **使用更可靠的重定向机制**：避免仅依靠客户端检查来实现 URL 重定向等关键安全功能。
