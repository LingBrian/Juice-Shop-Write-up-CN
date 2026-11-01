# Juice-Shop 攻略：登录 Bender 账户

## 挑战概述

**标题：** 登录 Bender 账户\
**类别：** 注入漏洞\
**难度：** ⭐⭐⭐ (3/6)

本挑战重点是利用 SQL 注入漏洞绕过身份验证机制，未经授权访问用户账户，目标是应用中与“Bender”关联的账户。

## 使用的工具

- Web 浏览器

## 方法与解决方案

### 查找目标信息

1. **查找 Bender 的电子邮箱**：
   - 以管理员身份登录应用，使用管理功能。
   - 在管理面板中找到 Bender 的电子邮箱（`bender@juice-sh.op`）：

    <img src="../assets/difficulty3/login_bender_1.png" alt="bender username from administration panel" width="500px">

### 执行 SQL 注入

2. **尝试以 Bender 身份登录**：
   - 前往应用登录页面。
   - 在电子邮箱字段中输入 Bender 的电子邮箱。
   
3. **构造 SQL 注入**：
   - 修改电子邮箱输入内容，加入 SQL 注入载荷：`bender@juice-sh.op' AND '1'='1' --`。
   - 此载荷通过单引号闭合电子邮箱字符串，加入始终为真的条件（`AND '1'='1'`），再用 `--` 注释掉 SQL 命令的剩余部分，以操纵登录表单背后的 SQL 查询。

### 观察结果

4. **观察身份验证绕过**：
   - 提交载荷后，注入会改变 SQL 查询，导致系统错误地将会话认证为 Bender，无需密码。
   - 成功以 Bender 身份登录，证明 SQL 注入漏洞可被利用。

    <img src="../assets/difficulty3/login_bender_2.png" alt="login proof" width="500px">

## 解决方案说明

通过利用登录表单电子邮箱字段中的典型 SQL 注入漏洞，成功完成挑战。该漏洞允许操纵底层 SQL 查询，使条件始终为真并绕过密码验证，从而未经授权访问 Bender 的账户。

## 修复建议

在真实应用中，可采取以下措施防止此类漏洞：
- **使用预处理语句**：采用带参数的预处理语句处理 SQL 命令。此做法将 SQL 逻辑与数据分离，从而防止 SQL 注入。
- **实施适当的输入验证**：确保验证并清理所有用户输入，防止恶意数据影响 SQL 命令。
