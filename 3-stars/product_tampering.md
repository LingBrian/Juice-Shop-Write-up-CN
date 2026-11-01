# Juice-Shop 攻略：篡改商品信息

## 挑战概述

**标题：** 篡改商品信息\
**类别：** 访问控制失效\
**难度：** ⭐⭐⭐ (3/6)

本挑战要求更改“OWASP SSL Advanced Forensic Tool (O-Saft)”商品描述中超链接的 href 属性，使其指向新 URL：`https://owasp.slack.com`。

## 使用的工具

- **Web 浏览器**：浏览应用并使用商品管理功能。
- **Burp Suite**：修改请求并重新发送到服务器。

## 方法与解决方案

### 找到注入点

1. **检查访问控制**：
   - 前往可更新商品的会计页面；这表明可能没有严格限制哪些用户可以修改商品详情。

    <img src="../assets/difficulty3/product_tampering_1.png" alt="admin panel" width="500px">

### 尝试更新

2. **捕获并分析 HTTP 请求**：
   - 使用开发者工具或 Burp Suite 捕获更新商品描述时发送的请求。
   - 最初尝试覆盖描述内容，但未能生效，这表明后端可能对输入进行了验证或过滤。

    <img src="../assets/difficulty3/product_tampering_2.png" alt="override" width="500px">
     
### 探索 API 端点

3. **发现 API 端点**：
   - 使用浏览器或工具访问 `/api/Products`，列出所有商品详情。
   - 从 JSON 响应中找到“OWASP SSL Advanced Forensic Tool (O-Saft)”的特定商品 ID。
   - 发现此端点存在描述覆盖漏洞：

    <img src="../assets/difficulty3/product_tampering_3.png" alt="descritpion override" width="500px">

### 成功提交载荷

4. **构造有效载荷**：
   - 构造 JSON 载荷，将 `description` 字段修改为包含新超链接：`<a href="https://owasp.slack.com" target="_blank">More...</a>`。
   - 使用正确的商品 ID，通过负责更新商品详情的 API 端点提交修改后的载荷，例如 `/api/Products/[ID]`。

    <img src="../assets/difficulty3/product_tampering_4.png" alt="modified request" width="500px">

### 验证结果

5. **验证**：
   - 在应用中检查商品描述，确认超链接已按预期更改。
   - 确认链接指向新的目标 URL：`https://owasp.slack.com`。

## 结论

成功更改商品链接，说明服务器端处理和验证用户输入的方式存在严重缺陷，导致未经授权的商品详情修改。攻击者利用此类漏洞可能造成错误信息、网络钓鱼或其他恶意后果。

## 修复建议

为防止此类漏洞：

- **实施严格的基于角色的访问控制（RBAC）**：确保只有获授权的人员可以修改商品详情。
- **清理并验证所有输入**：妥善清理和验证所有用户输入，尤其是允许包含 HTML 内容的商品描述。
