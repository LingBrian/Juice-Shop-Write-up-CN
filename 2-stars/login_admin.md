# Juice-Shop Write-up：Login Admin

## 挑战概述

**Title:** Login Admin\
**Category:** SQL Injection\
**Difficulty:** ⭐⭐ (2/6)

“Login Admin”挑战要求利用 Web 应用身份验证流程中的 SQL Injection 漏洞，未授权访问管理员账户。

## 使用的工具

- **Web Browser**：用于与应用登录界面交互。
- **Burp Suite**：用于拦截和修改 HTTP 请求，以注入 SQL 代码。

## 方法与解决方案

### 识别漏洞

分析登录机制时发现，在输入字段中插入引号（`'`）等特殊字符会显示 SQL 错误，这表明可能存在 SQL Injection 漏洞。

### 挑战解决步骤

1. **拦截登录请求**：
   - 使用 Burp Suite 拦截正常登录时发送的 HTTP POST 请求。该请求以 JSON 数据形式包含用户凭据。

   <img src="../assets/difficulty2/login_admin_1.png" alt="normal login" width="500px">

2. **修改 SQL 查询**：
   - 将 JSON 负载中的 email 字段改为 `admin@juice-sh.op' OR '1'='1' --`，从而将 SQL 命令变为始终返回 true 的语句，绕过密码验证。

   <img src="../assets/difficulty2/login_admin_2.png" alt="modified request" width="500px">

3. **执行注入请求**：
   - 将修改后的请求发送到服务器。由于注入的 SQL 代码，条件 `OR '1'='1'` 始终成立，因此可以未授权访问管理员账户。

### 解决方案说明

SQL Injection 得以成功，是因为输入净化不当，且处理 SQL 查询时未使用预处理语句。注入代码改变了 SQL 命令的逻辑结构，从而绕过身份验证，无需真实密码即可作为管理员登录。

## 修复建议

为防止真实应用中出现此类漏洞，必须实施以下安全措施：

- **使用预处理语句**：使用带参数的预处理语句，防止 SQL Injection。该技术确保 SQL 解释器能够区分代码和数据。
- **输入验证**：对所有用户输入进行严格验证，确保只处理预期的数据类型和格式。
- **错误处理**：配置错误消息，避免泄露数据库结构或 SQL 语法错误的详细信息。
- **安全测试**：定期开展安全评估和渗透测试，以发现并缓解 SQL Injection 等漏洞。
