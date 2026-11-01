# Juice-Shop Write-up: NoSQL 渗出

## 挑战概述
**标题：** NoSQL Infiltration  
**类别：** NoSQL 注入  
**难度：** ⭐⭐⭐⭐⭐ (5/6)  

本挑战的目标是利用 Juice Shop 应用中的 NoSQL 注入漏洞，获取所有订单历史记录。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用。
- **Burp Suite：** 用于拦截和修改 HTTP 请求，以测试注入点。

## 方法与解题过程

### 步骤 1：识别潜在注入点
本挑战涉及订单相关功能，因此我们从订单页面着手。以下步骤用于识别注入点：
1. **访问订单历史：** 前往 `http://localhost:3000/#/order-history`（这是显而易见的订单页面），并点击其中一个订单。
2. **拦截请求：** 使用 Burp Suite 拦截并分析查看订单时发出的请求。

### 步骤 2：分析 API 端点
通过分析拦截到的请求，我们发现用于获取订单详情的端点：
- **端点：** `/rest/track-order/[id]`

<img src="../assets/difficulty5/nosql_exfiltration_1.png" alt="normal request" width="600px">

### 步骤 3：测试 NoSQL 注入（尚未成功）
为测试 NoSQL 注入，我们尝试了基本载荷：
1. **载荷：** `/rest/track-order/'`

<img src="../assets/difficulty5/nosql_exfiltration_2.png" alt="modified request" width="600px">

   - **结果：** 收到 `500 Internal Server Error`，表明这里可能存在注入点。
2. **载荷：** `/rest/track-order/''`
   - **结果：** 收到 `Unexpected string` 错误，而不是 `Unexpected token`，说明服务器可能将我们的输入解析为 NoSQL 查询的一部分。

<img src="../assets/difficulty5/nosql_exfiltration_3.png" alt="request with payload" width="600px">

### 步骤 4：构造 NoSQL 注入载荷
根据错误消息，我们推断服务器可能构造了类似以下的查询：
```javascript
{ $where: "property === '" + payload + "'" }
```
为利用该问题，我们构造了一个始终为真的载荷：
- **载荷：** `' || true || '` 
- **URL 编码后的载荷：** `%27%20%7C%7C%20true%20%7C%7C%20%27`

### 步骤 5：注入载荷
1. **注入载荷：** 修改 URL，将载荷加入其中：`/rest/track-order/%27%20%7C%7C%20true%20%7C%7C%20%27`
2. **执行请求：** 发送请求并查看响应。

### 步骤 6：观察结果
注入载荷后，服务器返回了所有订单，而非仅返回指定订单，证实 NoSQL 注入成功。

## 解题说明
通过发现 `/rest/track-order/[id]` 端点中的 NoSQL 注入漏洞，并注入始终为真的载荷，我们操纵查询返回了所有订单，展示了该漏洞。

## 修复建议
- **输入验证与清理：** 验证和清理所有用户输入，以防止注入攻击。
- **使用参数化查询：** 通过参数化查询将代码与数据分离，以防止注入。
- **错误处理：** 改进错误处理，避免泄露可帮助攻击者构造载荷的详细错误信息。
