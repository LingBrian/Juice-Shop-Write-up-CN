# Juice-Shop 攻略：HTTP 标头 XSS

## 挑战概述
**标题：** HTTP-Header XSS  
**类别：** 跨站脚本（XSS）  
**难度：** ⭐⭐⭐⭐ (4/6)  

本挑战的目标是通过 HTTP 标头使用 `<iframe src="javascript:alert('xss')">` 发起持久型 XSS 攻击。

## 使用的工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **Burp Suite：** 用于拦截和修改 HTTP 请求，以注入 XSS 载荷。

## 方法与解法

### 步骤 1：确定目标端点
本挑战要求通过 HTTP 标头注入 XSS 载荷。我们推测管理面板中的 IP 地址记录功能可能是目标，因为它是少数可通过标头输入、且显然会在某处显示的信息之一。该功能会记录用户的 IP 地址，并在管理界面中显示：

<img src="../assets/difficulty4/x_header_xss_1.png" alt="Page that show IP of client" width="600px">

1. **进入管理面板：** 访问 `http://localhost:3000/#/privacy-security/last-login-ip`。

### 步骤 2：拦截请求
1. **退出登录：** 执行退出操作，并使用 Burp Suite 拦截请求，分析发送到服务器的标头。

<img src="../assets/difficulty4/x_header_xss_2.png" alt="Last login IP request" width="600px">

### 步骤 3：找到相关标头
检查拦截到的请求后，我们发现 `True-Client-IP` 标头可以被篡改，以反映任意值：
```http
POST /rest/user/logout HTTP/1.1
Host: localhost:3000
...
True-Client-IP: 99.99.99.99
...
```

该请求发出后，最后 IP 页面显示如下，证实可以进行注入：

<img src="../assets/difficulty4/x_header_xss_3.png" alt="Last know IP" width="600px">

### 步骤 4：测试 IP 反映
1. **修改标头：** 在 `True-Client-IP` 标头中设置一个常规 IP 地址，观察其是否会显示在管理面板中。
2. **确认反映：** 确认管理面板中出现了该 IP 地址。

### 步骤 5：构造 XSS 载荷
要利用此漏洞，需要构造一个会在管理面板中反映的 XSS 载荷：
- **载荷：**
  ```http
  True-Client-IP: <iframe src="javascript:alert('xss')">
  ```

### 步骤 6：注入载荷
1. **在 Burp Suite 中修改标头：** 将 `True-Client-IP` 的值替换为 XSS 载荷：`<iframe src="javascript:alert('xss')"`>。
2. **发送请求：** 通过 Burp Suite 退出登录并发送修改后的请求。

### 步骤 7：验证 XSS 执行
1. **以管理员身份登录：** 重新登录以访问 IP 面板。
2. **观察 XSS 执行：** 检查界面是否执行了 XSS 载荷。若成功，应出现内容为 `xss` 的警告框。

<img src="../assets/difficulty4/x_header_xss_4.png" alt="XSS proof" width="600px">

## 解法说明
本挑战通过识别 `True-Client-IP` 标头中的反射型 XSS 漏洞完成。注入会在管理面板中反映的载荷后，我们成功执行了持久型 XSS 攻击。

## 修复建议
- **输入验证和清理：** 正确验证并清理所有用户输入，包括 HTTP 标头，防止注入攻击。
- **转义输出：** 在用户界面显示用户输入时始终进行转义，防止 XSS。
