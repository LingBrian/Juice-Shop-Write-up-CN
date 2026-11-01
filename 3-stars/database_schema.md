# Juice-Shop 攻略：数据库架构

## 挑战概述

**标题：** 数据库架构\
**类别：** 信息泄露\
**难度：** ⭐⭐⭐ (3/6)

“数据库架构”挑战旨在测试通过 SQL 注入提取数据库架构的能力；这些信息可能暴露后端数据库的敏感结构细节。

## 使用的工具

- **Web 浏览器**：访问 Web 应用。
- **HTTP 拦截工具**（例如 Burp Suite）：拦截并修改 HTTP 请求以进行 SQL 注入。

## 方法与解决方案

### 找到注入点

1. **寻找存在漏洞的端点**：
   - 找出应用中易受 SQL 注入影响的参数。本挑战使用产品搜索功能 `127.0.0.1:3000/rest/products/search?q=payload`，其中 `q` 参数存在漏洞。

### 构造注入载荷

2. **通过 SQL 注入提取架构**：
   - 利用 SQL UNION 运算符注入查询，以显示数据库架构。由于产品搜索结果以结构化形式显示，而且产品条目通常包含多个属性（列），SQL 注入载荷必须匹配正确的列数。
   - 使用载荷：`test')) UNION SELECT 1, 2, 3, 4, 5, 6, 7, 8, sql FROM sqlite_schema--`，以匹配原查询所需的列数，并从 `sqlite_schema` 表中提取架构信息。

    <img src="../assets/difficulty3/database_schema_1.png" alt="sql injection" width="500px">

### 提取架构

3. **执行注入**：
   - 通过存在漏洞的参数提交构造好的载荷。如果 UNION SELECT 注入中的列数和数据类型与原 SQL 查询的预期相匹配，应用就会在搜索结果中显示数据库架构内容。
   - 结果包含表结构、列详情，有时还包括关系和约束信息等重要内容。

### 记录结果

4. **记录架构信息**：
   - 捕获并记录 SQL 注入返回的数据库架构信息。这些信息有助于了解数据库布局，便于进一步利用或分析。

### 解决方案说明

通过利用 SQL 注入漏洞提取数据库架构，成功完成挑战。了解应用底层数据存储结构是重要的一步；这不仅揭示了严重的安全缺陷，也暴露了可能被用于升级攻击或窃取数据的敏感信息。

## 修复建议

为防止类似漏洞：

- **使用预处理语句**：使用参数化查询或预处理语句，避免 SQL 注入。
- **验证并清理输入**：确保验证并清理所有用户输入，防止恶意数据影响 SQL 查询。

## 数据库架构

以下是 Juice Shop 数据库的完整架构，供后续挑战参考：

<img src="../assets/difficulty3/database_schema_2.png" alt="database schema" width="500px">

<img src="../assets/difficulty3/database_schema_3.png" alt="database schema" width="500px">

<img src="../assets/difficulty3/database_schema_4.png" alt="database schema" width="500px">

<img src="../assets/difficulty3/database_schema_5.png" alt="database schema" width="500px">

   <img src="../assets/difficulty3/database_schema_6.png" alt="database schema" width="500px">

   <img src="../assets/difficulty3/database_schema_7.png" alt="database schema" width="500px">

   <img src="../assets/difficulty3/database_schema_8.png" alt="database schema" width="500px">
