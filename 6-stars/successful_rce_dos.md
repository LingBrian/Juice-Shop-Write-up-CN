# Juice-Shop Write-up: 成功的 RCE DoS

## 挑战概述
**标题：** Successful RCE DoS  
**类别：** 远程代码执行（RCE）/拒绝服务（DoS）  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)  

本挑战的目标是利用易受攻击的 API 端点注入恶意载荷，使 Juice Shop 服务器遭受拒绝服务（DoS）。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **GoBuster：** 用于爆破目录和文件，以发现隐藏路径。
- **Swagger UI：** 用于与 Juice Shop API 交互并测试载荷。

## 方法与解题过程

### 步骤 1：发现 Swagger API 文档
使用 GoBuster，我们发现 Juice Shop 的 Swagger API 文档位于 `http://localhost:3000/api-docs`。

### 步骤 2：定位易受攻击的端点
在 Swagger UI 中，我们找到了 `/orders` 端点，它允许添加新订单。该端点接受 JSON 数据，其中的 `orderLinesData` 属性可以包含任意 JSON。

### 步骤 3：授权 API 请求
为了发送 API 请求，我们需要提供 bearer token 进行授权：
1. **获取 Bearer Token：** 从浏览器开发者工具中提取 token。
2. **授权请求：** 在 Swagger UI 中输入 token，为请求授权。

<img src="../assets/difficulty6/successful_rce_dos_1.png" alt="authorization" width="600px">

### 步骤 4：测试 API 端点
1. **Try It Out：** 在 Swagger UI 的 `/orders` 端点中点击“Try it out”，查看示例载荷结构：
   ```json
   {
     "cid": "JS0815DE",
     "orderLines": [
       {
         "productId": 8,
         "quantity": 500,
         "customerReference": "PO0000001"
       }
     ],
     "orderLinesData": "[{\"productId\": 12,\"quantity\": 10000,\"customerReference\": [\"PO0000001.2\", \"SM20180105|042\"],\"couponCode\": \"pes[Bh.u*t\"},{\"productId\": 13,\"quantity\": 2000,\"customerReference\": \"PO0000003.4\"}]"
   }
   ```
2. **识别漏洞：** 注意到 `orderLinesData` 允许注入任意 JSON，因此可能成为注入攻击向量。

### 步骤 5：构造 DoS 载荷
为了利用该漏洞，我们构造了一个会导致正则表达式拒绝服务（ReDoS）的载荷：
- **载荷：**
  ```json
  {
    "orderLinesData": "/((a+)+)b/.test('aaaaaaaaaaaaaaaaaaaaaaaaaaaaa')"
  }
  ```
该载荷使用复杂正则表达式，导致过度回溯和处理时间增加，使服务器挂起。

### 步骤 6：执行 DoS 攻击
1. **输入载荷：** 在 Swagger UI 中用 ReDoS 载荷替换示例载荷。

<img src="../assets/difficulty6/successful_rce_dos_2.png" alt="input payload" width="600px">

2. **发送请求：** 执行请求并观察服务器响应。

### 步骤 7：观察结果
如预期，服务器返回 503 Service Unavailable 错误，表明载荷成功造成 DoS 状态。

<img src="../assets/difficulty6/successful_rce_dos_3.png" alt="Error 503" width="600px">

## 解题说明
本挑战利用了允许注入任意 JSON 的易受攻击 API 端点。注入 ReDoS 载荷后，正则表达式处理耗时过长，导致服务器挂起并发生拒绝服务。

## 修复建议
- **输入验证：** 对所有用户提供的数据实施严格的输入验证和清理。
- **正则表达式安全：** 避免使用可能造成过度回溯和处理耗时过长的复杂正则表达式。
- **速率限制与超时：** 实施速率限制和请求超时，以降低潜在 DoS 攻击的影响。
