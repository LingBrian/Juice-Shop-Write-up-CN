# Juice-Shop Write-up：Deprecated Interface

## 挑战概述

**Title:** Deprecated Interface\
**Category:** Security Misconfiguration\
**Difficulty:** ⭐⭐ (2/6)

本挑战涉及利用已弃用且不再得到妥善维护或保护的界面。

## 使用的工具

- **Web Browser**：用于与应用界面交互并上传文件。
- **Developer Tools**：用于检查网络请求和应用程序的 JavaScript 源代码。

## 方法与解决方案

### 识别易受攻击的界面

1. **分析现有功能**：
   - 检查应用后发现，随着新的聊天机器人服务开始处理问题，`/complain` 页面显得过时，原有功能也有些多余。

### 检查文件上传功能

2. **检查 JavaScript**：
   - 使用浏览器开发者工具检查 `main.js` 文件，其中说明了已弃用的 `/complain` 界面功能。注意到该表单包含文件上传器。

   <img src="../assets/difficulty2/depreceted_interface_1.png" alt="complain" width="500px">

   - 观察到上传器代码试图将文件上传限制为特定 MIME 类型（`application/pdf`、`text/xml`、`application/zip` 等）。

   <img src="../assets/difficulty2/depreceted_interface_2.png" alt="restriction about the type" width="500px">

3. **操纵文件上传**：
   - 尝试上传普通 XML 文件，但被拦截。将扩展名改为 `.xml.zip` 后再次尝试，最初仍未通过。
   - 选择文件后、上传前修改文件名，移除 `.zip` 后缀，欺骗上传器接受没有 `.zip` 扩展名的 XML 文件，从而绕过客户端检查。

   <img src="../assets/difficulty2/depreceted_interface_3.png" alt="request modified" width="500px">

### 利用文件上传漏洞

4. **利用配置错误**：
   - 成功上传了经过构造且标签错误的 XML 文件，利用了界面文件处理和验证流程中的缺陷。这可能进一步导致 XML External Entity (XXE) 注入或提供恶意内容等攻击。

### 解决方案说明

本挑战利用了已弃用且缺乏适当维护的界面中的文件上传器。通过修改文件名绕过了客户端文件类型检查。

## 修复建议

为防止已弃用或疏于维护的功能引发安全问题：

- **定期更新并停用旧功能**：定期审计并更新或停用过时功能，避免留下安全漏洞。
- **安全地处理文件上传**：在服务器端实施可靠的文件上传验证。确保 MIME 类型检查在服务器端而非仅在客户端执行，并扫描上传文件中的恶意软件。
- **访问控制与监控**：限制对已弃用功能的访问，并监控异常活动，因为这些功能可能成为攻击目标。
