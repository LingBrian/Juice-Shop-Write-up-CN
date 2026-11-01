# Juice-Shop 攻略：上传文件大小

## 挑战概述

**标题：** 上传文件大小\
**类别：** 输入验证不当\
**难度：** ⭐⭐⭐ (3/6)

“上传文件大小”挑战要求上传大于 100 KB 的文件，超过应用客户端验证所实施的大小限制。

## 使用的工具

- **Web 浏览器**：浏览 Web 应用并操作文件上传。
- **Burp Suite**：拦截、修改并重新发送 HTTP 请求，以绕过客户端限制。

## 方法与解决方案

### 首次尝试

1. **测试文件上传**：
   - 尝试通过个人资料图片上传功能，直接上传大于 100 KB 的文件。

    <img src="../assets/difficulty3/upload_size_1.png" alt="large file" width="500px">

   - 遇到错误：`MulterError: File too large`，表明服务器端实施了文件大小限制。

    <img src="../assets/difficulty3/upload_size_2.png" alt="error file too large" width="500px">

### 分析应用

2. **探索其他上传入口**：
   - 根据提示转而查看投诉页面，该页面也支持文件上传。
   - 发现该页面仅通过客户端 JavaScript 浅层限制文件大小，因此可能有办法绕过限制。

### 构造载荷

3. **创建边界情况文件**：
   - 使用 Linux 命令生成一个略小于 100 KB 的文件：`base64 /dev/urandom | head -c 99900 > random.pdf`。

### 利用服务器端检查

4. **修改请求**：
   - 上传 `random.pdf`，并使用 Burp Suite 拦截 HTTP 请求。

    <img src="../assets/difficulty3/upload_size_3.png" alt="request of upload" width="500px">

   - 通过重复文件载荷中的内容来修改被拦截的请求，使文件大小超过 100 KB。

    <img src="../assets/difficulty3/upload_size_4.png" alt="modified request of upload" width="500px">

### 绕过大小限制

5. **重新发送修改后的请求**：
   - 发送修改后、文件大小已增加的请求。
   - 服务器成功响应上传请求，表明文件大小检查已被绕过。

### 解决方案说明

通过利用服务器文件大小检查机制的弱点完成挑战：只有初始文件大小在客户端进行验证，而服务器端检查可通过修改请求绕过。这体现了常见的安全疏漏：服务器端没有对客户端检查后可能被篡改的参数进行充分复核。

## 修复建议

在真实应用中，可采取以下措施防止此类漏洞：

- **实施健全的服务器端验证**：确保所有输入验证（包括文件大小）都在提交后由服务器重新执行。
- **限制输入修改**：使用严格限制整个载荷大小的 multipart/form-data 解析库，而不要依赖客户端限制。
