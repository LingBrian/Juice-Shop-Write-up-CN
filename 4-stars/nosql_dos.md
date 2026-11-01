# Juice-Shop 攻略：NoSQL 拒绝服务攻击

## 挑战概述
**标题：** NoSQL DoS  
**类别：** 拒绝服务（DoS）  
**难度：** ⭐⭐⭐⭐ (4/6)  

本挑战的目标是利用 Juice Shop 应用中的 NoSQL 注入漏洞，让服务器休眠，从而发动拒绝服务（DoS）攻击。

## 使用的工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **Burp Suite：** 用于拦截和修改 HTTP 请求，以测试注入点。

## 方法与解法

### 步骤 1：确定存在漏洞的端点
之前的一个挑战中，我们发现了一个可修改评论的端点，该端点可能存在 NoSQL 注入漏洞：
- **端点：** `/rest/products/[productId]/reviews`

### 步骤 2：拦截请求
1. **进入评论区域：** 在 Juice Shop 中打开商品页面并提交评论。
2. **拦截请求：** 使用 Burp Suite 拦截提交或修改评论时发出的请求。

### 步骤 3：分析请求
拦截到的评论修改请求如下：
```http
PUT /rest/products/1/reviews HTTP/1.1
Host: localhost:3000
Content-Length: 60
Content-Type: application/json
...

{
  "id": "oBNfC3Xcj4wpYT587",
  "message": "test modify"
}
```

### 步骤 4：构造 DoS 载荷
要利用 NoSQL 注入漏洞，需要构造能让服务器休眠指定时长的载荷：
- **载荷尝试 1：** 在 ID 参数中注入 `sleep` 函数：
  ```json
  {"id":"sleep(10000)", "message": "test modify"}
  ```
  - **结果：** 此载荷未按预期生效。

- **载荷尝试 2：** 直接将载荷放入 URL：
  ```http
  PUT /rest/products/sleep(20000)/reviews HTTP/1.1
  Host: localhost:3000
  ```

### 步骤 5：执行载荷
1. **直接访问 URL：** 通过 Burp Suite 直接注入没有效果，因此我们直接访问该 URL：
   ```http
   http://localhost:3000/rest/products/sleep(20000)/reviews
   ```
2. **验证 DoS：** 服务器应在指定时长内无响应，表明 DoS 攻击成功。

### 步骤 6：观察结果
注入载荷后，服务器在 `sleep` 函数指定的时长内无响应，确认攻击成功。

## 解法说明
通过识别 `/rest/products/[productId]/reviews` 端点中的 NoSQL 注入漏洞，并构造利用该漏洞的载荷，我们让服务器休眠指定时长，造成拒绝服务（DoS）状态。

## 修复建议
- **输入验证和清理：** 确保对所有用户输入进行适当验证和清理，防止注入攻击。
- **使用参数化查询：** 通过分离代码与数据，使用参数化查询防止 NoSQL 注入。
- **速率限制和超时：** 实施速率限制和请求超时，以降低潜在 DoS 攻击的影响。
