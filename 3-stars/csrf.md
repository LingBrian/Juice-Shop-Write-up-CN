# Juice-Shop 攻略：CSRF

## 挑战概述

**标题：** CSRF（跨站请求伪造）\
**类别：** 访问控制失效\
**难度：** ⭐⭐⭐ (3/6)

“CSRF”挑战要求执行跨站请求伪造（CSRF）攻击，在未经用户同意的情况下更改 OWASP Juice Shop 平台上的用户名。

## 使用的工具

- **Web 浏览器**：使用缺少现代 CSRF 防护的旧版本（例如较简单的 Firefox 81.0）。
- **Burp Suite**：拦截并分析 HTTP 请求。
- **HTML 编辑器**：构造并部署 CSRF 攻击页面（建议使用 http://htmledit.squarefree.com/）。

## 方法与解决方案

### 找到存在漏洞的端点

1. **发现端点**：
   - 在应用个人资料页面更改用户名时，使用 Burp Suite 拦截并检查发出的 HTTP 请求。
   - 找到用于更改用户名的 POST 请求，并记录 URL 和表单参数。

    <img src="../assets/difficulty3/csrf_2.png" alt="request" width="500px">

### 构造 CSRF 攻击页面

2. **构造 CSRF 载荷**：
   - 创建一个 HTML 页面，其中包含指向存在漏洞端点的自动提交表单。
   - 使用隐藏表单字段设置所需的新用户名。
   - 添加 JavaScript，在页面加载时自动提交表单。

    ```html
    <html>
    <body>
      <form action="http://127.0.0.1:3000/profile" method="POST">
        <input type="hidden" name="username" value="HelloFromCSRF" />
      </form>
      <script>document.forms[0].submit();</script>
    </body>
    </html>
    ```

### 部署 CSRF 攻击

3. **执行攻击**：
   - 将 CSRF 攻击 HTML 上传到可公开访问或受控的域名。作为本 CTF 的演示，使用 http://htmledit.squarefree.com/ 在本地测试该文件，以模拟真实攻击场景。
   - 当受害者访问恶意页面时，脚本会利用其已认证会话提交表单，未经其明确同意更改用户名。

### 绕过现代浏览器防护

4. **规避浏览器的 CSRF 防护**：
   - 由于现代浏览器增强了对 CSRF 攻击的拦截能力，可使用不具备这些防护的旧版浏览器。
   - 禁用通常会拦截或警告可疑跨站请求的安全功能。

## 修复建议

为防范 CSRF 漏洞：
- **使用 Anti-CSRF Token**：确保每次表单提交都包含经服务器验证的令牌。
- **采用 Same-Site Cookie**：配置 Cookie，使其仅随来自设置该 Cookie 的同一站点的请求发送。
- **实施 CORS 策略**：正确配置跨源资源共享（CORS）策略，仅允许受信任的域访问资源。
