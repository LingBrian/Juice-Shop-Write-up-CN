
# Juice-Shop Write-Up：Reflected XSS

## 挑战概述
**Title:** Reflected XSS  
**Category:** Web Exploitation  
**Difficulty:** ⭐⭐ (2/6)  

本挑战的目标是利用 Web 应用中的反射型跨站脚本 (XSS) 漏洞，注入在页面加载时执行的恶意 JavaScript 代码。

## 使用的工具
- **Web Browser:** 用于浏览 Web 应用并测试 XSS payload。
- **Developer Tools:** 用于检查 Web 应用中的 HTML 和 JavaScript 代码。

## 方法与解决方案

### 步骤 1：测试初始 Payload（未奏效）
我首先尝试在应用搜索框中使用 `q` 参数测试基本 XSS payload：
```html
<iframe src="javascript:alert(`xss`)">
```
虽然该 payload 成功执行，但它没有反映在页面上，因此不符合挑战要求。

### 步骤 2：寻找反射点
为找到输入会直接反映到页面上的位置，我浏览了应用，并发现订单追踪页面使用 URL 中的 `id` 参数显示订单详情。

<img src="../assets/difficulty2/reflected_xss_2.png" alt="url" width="600px">

该 `id` 参数会在响应中反射出来。

<img src="../assets/difficulty2/reflected_xss_1.png" alt="order tracking" width="600px">

### 步骤 3：构造 Payload
我构造了一个通过 URL 中的 `id` 参数注入的 payload：
```html
http://127.0.0.1:3000/#/track-result?id=test
```

### 步骤 4：插入 Payload
访问带有构造 payload 的 URL 后，输入内容反射到了订单追踪页面，并导致 JavaScript 代码执行：
```html
http://127.0.0.1:3000/track-result?id=<script>alert('xss')</script>
```
加载该 URL 时脚本会执行，从而确认存在反射型 XSS 漏洞。

<img src="../assets/difficulty2/reflected_xss_3.png" alt="proof" width="600px">

### 解决方案说明
本挑战通过发现订单追踪页面 `id` 参数中的反射型 XSS 漏洞而解决。将 script 标签注入该参数后，恶意 JavaScript 代码会在页面加载时执行。

### 修复建议
为防止反射型 XSS 漏洞，应用应当：
- **验证并净化输入：**确保所有用户输入在加入 HTML 响应之前都经过适当验证和净化。
- **编码输出：**在 HTML 内容中渲染用户提供的数据之前进行适当的 HTML 编码，防止注入脚本执行。
