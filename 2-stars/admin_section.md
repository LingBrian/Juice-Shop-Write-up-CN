# Juice-Shop Write-up：Admin Section

## 挑战概述

**Title:** Admin Section\
**Category:** Broken Access Control\
**Difficulty:** ⭐⭐ (2/6)

“Admin Section”挑战涉及访问 Web 应用中受限的管理区域。

## 使用的工具

- **Web Browser**：用于访问应用并使用开发者工具检查源文件。
- **Developer Tools**：具体使用源代码查看器检查 JavaScript 文件中的路由定义。

## 方法与解决方案

### 探索应用路由

1. **源代码分析**：
   - 使用浏览器开发者工具检查 `main.js` 文件。该文件包含各种应用路由（路径和组件）的定义，其中包括“administration”区域的路径。
   - 路由设置表明，只要知道正确路径，就可以直接访问管理区域。

   <img src="../assets/difficulty2/admin_section_1.png" alt="routes" width="500px">

2. **直接访问路径**：
   - 在浏览器地址栏直接输入路径 `127.0.0.1:3000/#/administration`。
   - 最初由于权限不足，页面拒绝了访问。

   <img src="../assets/difficulty2/admin_section_2.png" alt="permissions" width="500px">

### 获取管理员访问权限

3. **提升角色权限**：
   - 使用管理员账户凭据登录。
   - 再次访问 `/administration` 路径后，成功显示管理仪表板。这表明访问控制不当，系统仅依赖路径隐蔽性来保护该区域。

   <img src="../assets/difficulty2/admin_section_3.png" alt="page access" width="500px">

### 解决方案说明

本挑战通过从 JavaScript 源文件中找出隐藏的管理路径，再使用管理员凭据访问该路径而解决。这表明应用未能充分保护敏感区域，而只是将其隐藏，并假定未授权用户不会发现该路径。

## 修复建议

为有效保护管理区域，可考虑实施以下措施：

- **适当的访问控制**：在授予用户访问应用敏感部分的权限前，实施可靠的身份验证和授权措施。通过服务器端检查确认用户权限。
- **关注点分离**：如建议所述，将管理功能与主应用分离。理想情况下，可将管理面板托管在不同的服务器上，或作为无法从公共互联网访问的独立服务。
- **使用 Web Application Firewalls (WAF)**：部署 WAF，以监控并阻止未经授权的访问受限区域的尝试。
