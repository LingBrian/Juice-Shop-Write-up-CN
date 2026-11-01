# Juice-Shop Write-up：Empty User Registration

## 挑战概述

**Title:** Empty User Registration\
**Category:** Improper Input Validation\
**Difficulty:** ⭐⭐ (2/6)

“Empty User Registration”挑战要求利用输入验证不当相关漏洞，在没有电子邮件和密码的情况下注册用户。本挑战用于测试应用的用户输入验证机制是否健全。

## 使用的工具

- **Web Browser**：用于与注册界面交互。
- **Burp Suite**：用于拦截和修改 HTTP 请求。

## 方法与解决方案

### 挑战准备

任务是创建账户，但需要找到绕过常规验证的方法，因为常规验证会阻止使用空凭据注册。

<img src="../assets/difficulty2/empty_user_registration_1.png" alt="normal user registration" width="500px">

### 挑战解决步骤

1. **拦截注册请求**：
   - 使用 Burp Suite，在正常注册过程中拦截注册请求。由此捕获发送到 `/api/Users/` 的 POST 请求。

   <img src="../assets/difficulty2/empty_user_registration_2.png" alt="request" width="500px">

2. **修改请求负载**：
   - 最初尝试将 `email` 和 `password` 字段设为空字符串（`""`）或 `null`，结果分别是收到“Bad Request”错误，或意外创建了普通用户。
   - 突破点是将这些字段从 JSON 负载中完全删除。也就是说，不设置为空字符串或 `null`，而是直接删除 `email` 和 `password` 的整个键值对。

3. **提交修改后的请求**：
   - 从 JSON 负载中删除 `email` 和 `password` 条目后，转发修改后的请求。服务器在缺少这些字段的情况下仍处理了请求，并创建了没有电子邮件和密码的用户。

   <img src="../assets/difficulty2/empty_user_registration_3.png" alt="request" width="500px">

### 解决方案说明

本挑战发现了服务器输入验证流程中的缺陷：系统没有充分检查关键字段（`email` 和 `password`）缺失的情况。从 POST 数据中完全删除这些字段后，应用未能执行本应拒绝该请求的输入验证规则。

## 修复建议

为防止真实应用中出现此类漏洞，应实施以下措施：

- **严格的输入验证**：确保对所有必需字段进行存在性和内容验证。应直接拒绝缺少必要数据的请求。
- **必填字段检查**：后端逻辑应包含必要的检查，确认所有必填数据字段均存在且非空，然后再处理请求。
- **使用 Schema 验证**：对传入数据实施 schema 验证，以自动强制执行数据存在性、类型和格式规则。
