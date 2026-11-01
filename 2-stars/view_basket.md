# Juice-Shop Write-up：View Basket

## 挑战概述

**Title:** View Basket\
**Category:** Broken Access Control\
**Difficulty:** ⭐⭐ (2/6)

“View Basket”挑战围绕 Web 应用中的访问控制失效漏洞展开。具体漏洞允许用户通过操纵 Web 请求中的用户标识符，查看其他用户的购物篮详情。

## 使用的工具

- **Burp Suite**：用于拦截和修改用户浏览器发送到 Web 服务器的 HTTP 请求。

## 方法与解决方案

### 了解漏洞

Web 应用允许用户查看自己的购物篮内容。但使用 Burp Suite 检查网络流量后发现，获取购物篮详情的请求包含一个特定于用户的标识符（例如购物篮 ID），且该标识符可以被操纵。

### 挑战解决步骤

1. **拦截 HTTP 请求**：
   - 使用 Burp Suite 拦截获取用户本人购物篮的 HTTP 请求。
   - 请求 URL 为：`http://127.0.0.1:3000/rest/basket/1`，其中 `1` 是当前用户的购物篮 ID。

   <img src="../assets/difficulty2/view_basket_1.png" alt="request" width="500px">

   - 成功获取了自己的购物篮：

   <img src="../assets/difficulty2/view_basket_2.png" alt="basket 1" width="500px">

2. **修改购物篮 ID**：
   - 将拦截请求中的购物篮 ID 从 `1` 改为 `2`，尝试访问其他用户购物篮的详情。

   <img src="../assets/difficulty2/view_basket_3.png" alt="replaced request" width="500px">

3. **查看结果**：
   - 转发修改后的请求，响应中包含了另一位用户购物篮的详情，证实存在访问控制失效问题。

   <img src="../assets/difficulty2/view_basket_4.png" alt="new basket" width="500px">

### 解决方案说明

本挑战通过修改请求 URL 中的购物篮 ID 而成功解决。如果存在适当的访问控制机制，这种修改本不应奏效。该漏洞使攻击者能够访问其他用户的私人购物篮信息。

## 修复建议

为缓解真实应用中的此类漏洞，应考虑以下措施：

- **验证用户会话**：确保每个访问敏感信息（例如购物篮）的请求都与用户会话进行校验，确认用户只能查看自己的数据。
- **使用可靠的访问控制机制**：实施基于角色的访问控制 (RBAC) 或基于属性的访问控制 (ABAC)，严格管控数据访问权限。
- **定期安全审计**：定期开展安全审计和渗透测试，以便在访问控制问题遭到利用之前发现并修复。

本挑战强调了严格访问控制机制的重要性，它们能够防止未经授权地访问敏感用户信息。
