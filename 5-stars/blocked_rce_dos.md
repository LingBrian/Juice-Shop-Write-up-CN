# Juice-Shop Write-up: 已阻止的 RCE DoS

## 挑战概述
**标题：** Blocked RCE DoS  
**类别：** 不安全反序列化  
**难度：** ⭐⭐⭐⭐⭐ (5/6)  

本挑战的目标是执行远程代码执行（RCE），让应用持续忙碌，从而触发服务器针对特定 DoS 攻击向量的防护。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **GoBuster：** 用于爆破目录和文件，以发现隐藏路径。
- **Swagger UI：** 用于与 Juice Shop API 交互并测试载荷。
- **Burp Suite：** 用于拦截和修改 HTTP 请求，以测试注入点。

## 方法与解题过程

### 步骤 1：发现 API 文档
使用 GoBuster，我们发现了 Juice Shop 的 Swagger API 文档：`http://localhost:3000/api-docs`。

<img src="../assets/difficulty5/blocked_rce_dos_1.png" alt="Swagger API Documentation" width="600px">

### 步骤 2：定位易受攻击的端点
在 Swagger UI 中，我们找到了 `/orders` 端点，它允许添加新订单。该端点接受 JSON 数据，其中的 `orderLinesData` 属性可以包含任意 JSON 字符串。

### 步骤 3：授权 API 请求
为了发送示例 API 请求（这对下一步很有用），我们需要提供 bearer token 进行授权：
1. **获取 Bearer Token：** 从浏览器开发者工具中提取 token。
2. **授权请求：** 在 Swagger UI 中输入 token，为示例请求授权。

<img src="../assets/difficulty5/blocked_rce_dos_3.png" alt="Authorization token" width="600px">

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

这里可以看到示例页面：

<img src="../assets/difficulty5/blocked_rce_dos_2.png" alt="Example of API Call" width="600px">

### 步骤 5：构造 DoS 载荷
为了利用该漏洞，我们构造了一个会让服务器陷入无限循环的载荷：
- **载荷：**
  ```json
  {
    "orderLinesData": "(function dos() { while(true); })()"
  }
  ```

### 步骤 6：注入载荷
1. **替换载荷：** 在 Swagger UI 中修改示例载荷，加入 DoS 载荷。
2. **发送请求：** 点击“Execute”执行请求。

### 步骤 7：观察结果
注入载荷后，服务器约 2 秒内无响应，随后恢复并显示内部服务器错误。这表明服务器检测并缓解了 DoS 攻击；这是正常现象，因为 Juice Shop 将 DoS 攻击限制在 2 秒，以避免服务器崩溃。

<img src="../assets/difficulty5/blocked_rce_dos_4.png" alt="Internal Server error response" width="600px">

## 解题说明
通过识别 `/orders` 端点 `orderLinesData` 参数中的不安全反序列化漏洞，并注入导致无限循环的载荷，触发了服务器针对 DoS 攻击的防护机制。

## 修复建议
- **输入验证与清理：** 确保正确验证和清理所有用户输入，以防止注入攻击。
- **采用安全的反序列化实践：** 使用安全的反序列化方法，防止执行任意代码。
- **速率限制与超时：** 实施速率限制和请求超时，以降低潜在 DoS 攻击的影响。
