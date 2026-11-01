# Juice-Shop 攻略：用户凭据

## 挑战概述

**标题：** Users Credentials\
**类别：** 注入\
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求利用 SQL 注入漏洞，从应用数据库中提取敏感用户凭据。

## 使用的工具

- **Web 浏览器**：用于与应用交互并篡改 URL 参数。
- **开发者工具**：用于检查网络流量并修改 HTTP 请求。

## 方法与解法

### 步骤 1：确定注入点

1. **探索**：
   - 最初尝试在登录表单中输入各种内容来测试 SQL 注入漏洞，但没有成功。

   <img src="../assets/difficulty4/user_credentials_1.png" alt="sql injection" width="500px">

   - 随后转而调查商品搜索功能的端点，因为它看起来会直接与数据库交互。我在请求中找到了该端点：

   <img src="../assets/difficulty4/user_credentials_2.png" alt="request product using burp" width="500px">

### 步骤 2：测试 SQL 注入

2. **利用搜索功能**：
   - 基本搜索会返回 JSON 结果，这意味着我们可以在客户端与该 API 交互：

   <img src="../assets/difficulty4/user_credentials_3.png" alt="endpoint search" width="500px">

   - 尝试在搜索词后附加 `' OR 1=1 /**/` 进行基本 SQL 注入，成功绕过了初始搜索逻辑。

   <img src="../assets/difficulty4/user_credentials_4.png" alt="full search" width="500px">

### 步骤 3：构造注入查询

3. **确定表结构**：
   - 最初尝试使用基本联合查询提取 `USERS` 表数据：`test' UNION SELECT * FROM USERS /**/`。结果报错，指出列数不匹配。

   <img src="../assets/difficulty4/user_credentials_5.png" alt="endpoint syntax error" width="500px">

   <img src="../assets/difficulty4/user_credentials_6.png" alt="select union error" width="500px">

   - 经过进一步测试，确定了正确的列数，并将查询改为：`test')) UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, deluxeToken, profileImage FROM USERS--`。

### 步骤 4：最终载荷和数据提取

4. **成功提取数据**：
   - 调整查询，使其列结构与 `Products` 表匹配后，成功提取出用户凭据。

   <img src="../assets/difficulty4/user_credentials_7.png" alt="payload result" width="500px">

   - 为触发挑战标记，注入查询必须包含 `email` 字段，最终载荷为：`test')) UNION SELECT username, password, role, deletedAt, isActive, createdAt, id, email, profileImage FROM USERS--`。

   <img src="../assets/difficulty4/user_credentials_8.png" alt="new payload result" width="500px">

## 解法说明

我们利用商品搜索 API 端点中的 SQL 注入漏洞，直接从数据库中提取了用户名、密码和电子邮件地址等用户凭据。

解法包含几个关键步骤：
- 找到存在漏洞的端点。
- 测试并确认 SQL 注入漏洞。
- 构造与原始查询预期结构相匹配的查询，以避免错误。
- 确保输出包含所有必要的用户字段，以满足挑战条件。

### 安全措施

为防止此类漏洞：
- **输入清理**：清理所有输入，防止注入恶意 SQL 代码。
- **使用预处理语句**：使用带参数的预处理语句防止 SQL 注入。
- **最小权限原则**：将应用的数据库权限限制在其功能所需的最小范围内。
