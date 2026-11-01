# Juice-Shop Write-up: 双因素身份验证绕过

## 挑战概述

**标题：** Two-Factor Authentication Bypass  
**类别：** 身份验证失效  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求获取并利用与用户账户关联的基于时间的一次性密码（TOTP），以绕过 Juice Shop 应用的双因素身份验证（2FA）机制。

## 使用工具

- **SQL 注入：** 用于从数据库中提取敏感数据。
- **TOTP 生成器：** 用于根据提取的密钥生成有效 TOTP 代码。

## 方法与解题过程

### 步骤 1：了解 TOTP

回顾 TOTP 的基本原理：这是一种常见的 2FA 方法，使用密钥和当前时间生成临时密码，通常每 30 秒变化一次。该过程通过对时间和密钥进行哈希，生成短时有效的身份验证代码。下面的示意图很好地解释了这一概念：

<img src="../assets/difficulty5/two_factor_authentification_2.png" alt="explanation schema" width="500px">

概括来说，算法如下：

<img src="../assets/difficulty5/two_factor_authentification_4.png" alt="algorithm" width="500px">

如果你还不理解这一概念，建议阅读这篇文章：https://rublon.com/blog/what-is-totp/

### 步骤 2：通过 SQL 注入提取 TOTP 密钥

接着，我利用了之前在应用搜索功能中发现的 SQL 注入漏洞：

**注入载荷：**
```sql
test')) UNION SELECT username, password, role, deletedAt, isActive, totpSecret AS TOTP, id, email, profileImage FROM USERS--
```

该载荷旨在从数据库中提取用户信息，包括 TOTP 密钥。

### 步骤 3：获取 TOTP 密钥

将载荷嵌入搜索功能的查询中，执行 SQL 注入：
`http://127.0.0.1:3000/#/search?q=test`

攻击返回了多项用户详情，其中包括用户 `wurstbot` 的 TOTP 密钥：
- **TOTP 密钥：** `IFTXE3SPOEYVURT2MRYGI52TKJ4HC3KH`

### 步骤 4：生成 TOTP 代码

使用 TOTP 生成器输入提取到的密钥，并与当前 UNIX 时间同步，生成有效的 2FA 代码。

<img src="../assets/difficulty5/two_factor_authentification_5.png" alt="TOPT Generator" width="500px">

### 步骤 5：绕过 2FA

在 Juice Shop 登录页面的 2FA 提示中输入生成的 TOTP 代码，成功绕过双因素身份验证机制。

## 解题说明

利用 SQL 注入漏洞提取了用户账户的 TOTP 密钥，再用该密钥生成有效 TOTP 代码，从而在无法直接访问用户身份验证器设备或应用的情况下绕过 2FA。这表明敏感数据（例如 TOTP 密钥）泄露会破坏双因素身份验证系统的安全性。

## 修复建议

- **安全存储密钥：** 应加密存储 TOTP 密钥，并防止其通过注入攻击被读取。
- **输入验证与清理：** 验证和清理所有用户输入，以防止 SQL 注入。
