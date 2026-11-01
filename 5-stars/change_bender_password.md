# Juice-Shop Write-up: 修改 Bender 的密码

## 挑战概述

**标题：** Change Bender's Password\
**类别：** 身份验证失效\
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求在 `juice-sh.op` 平台上将用户 Bender 的密码改为 `slurmCl4ssic`，且不能使用 SQL 注入或忘记密码功能。

## 使用工具

- **Web 浏览器**：用于与 Web 应用交互并修改密码。
- **HTTP 拦截工具**：例如 Burp Suite，用于拦截、修改并重新发送 HTTP 请求。

## 方法与解题过程

### 初始设置

1. **访问账户**：
   - 使用提供的凭据登录 Bender 账户，或绕过登录机制。

### 探索修改密码功能

2. **拦截修改密码请求**：
   - 使用 HTTP 拦截工具捕获尝试修改 Bender 密码时发出的请求。
   - 分析请求参数，尤其关注当前密码的处理方式。

   <img src="../assets/difficulty5/change_bender_password_1.png" alt="change password request" width="500px">

### 修改请求

3. **尝试修改请求**：
   - 以不同方式修改 `current` 密码字段，探索潜在弱点：
     - 直接使用错误密码——失败

     <img src="../assets/difficulty5/change_bender_password_2.png" alt="false password" width="500px">

     - 将 `current` 密码字段留空——失败

     <img src="../assets/difficulty5/change_bender_password_3.png" alt="blank password" width="500px">

     - 从请求中完全删除 `current` 密码参数——成功

     <img src="../assets/difficulty5/change_bender_password_4.png" alt="removed password" width="500px">

### 成功利用

4. **绕过密码检查**：
   - 发现完全删除 `current` 密码字段即可绕过当前密码检查，并设置新密码。
   - 删除 `current` 密码参数，并将新密码设为 `slurmCl4ssic` 后提交请求。

## 解题说明

本挑战通过发现密码修改功能中的漏洞成功解决：服务器没有正确验证 `current` 密码参数是否存在。省略该参数后，服务器会将请求视为有效，从而无需知道原密码即可修改密码。

## 修复建议

在实际应用中，可通过以下方式防止此类漏洞：

- **严格验证参数：** 在处理请求前，确保检查所有必需参数是否存在且有效。
- **强化服务端逻辑：** 实现服务端逻辑，检测并拒绝不完整或格式错误的请求。
