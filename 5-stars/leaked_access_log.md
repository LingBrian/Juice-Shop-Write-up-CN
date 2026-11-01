
# Juice-Shop Write-up: 泄露的访问日志

## 挑战概述

**标题：** Leaked Access Log  
**类别：** 敏感数据暴露  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战围绕寻找泄露的访问日志展开，其中包含敏感用户信息，可能暴露登录凭据。

## 使用工具

- **Google 搜索：** 用于高级搜索（Google Dorks）。
- **Stack Overflow 和 Pastebin：** 开发者可能会在这些平台上无意中分享敏感信息。

## 方法与解题过程

### 步骤 1：收集初步线索

根据挑战提示，我们怀疑开发者可能在 Stack Overflow 或 Pastebin 等公共平台上发布了敏感日志：

<img src="../assets/difficulty5/leaked_access_log_5.png" alt="Challenge Hint" width="500px">

### 步骤 2：使用 Google Dork 搜索

执行以下 Google Dork 查询：
```
intext:"pastebin" juice shop stackoverflow
```
该查询旨在查找提及包含 Juice Shop 日志的 Pastebin 链接的 Stack Overflow 帖子：

### 步骤 3：分析 Stack Overflow 帖子

找到一条相关的 Stack Overflow 讨论，其中开发者询问如何降低 Express.js/Morgan 日志的详细程度，并引用了一个 Pastebin 链接：

<img src="../assets/difficulty5/leaked_access_log_1.png" alt="Google Dork pastebin" width="500px">

### 步骤 4：检查 Pastebin 内容

通过 Stack Overflow 帖子访问相应的 Pastebin 链接，其中包含一系列访问日志。日志中有一条用户尝试修改密码的记录，显示确认密码字段存在差异，暗示可能重复使用了旧密码。

<img src="../assets/difficulty5/leaked_access_log_2.png" alt="changing password log" width="500px">

### 步骤 5：解码并测试密码

从日志中找到了密码：
```
0Y8rMnww$*9VFYE%C2%A759-!Fg1L6t&6lB
```
解码 URL 编码字符并整理密码：
```
0Y8rMnww$*9VFYE§59-!Fg1L6t&6lB
```

### 步骤 6：暴力测试

1. 使用之前某个管理面板挑战中收集的用户邮箱……

<img src="../assets/difficulty5/leaked_access_log_4.png" alt="wordllist" width="500px">

2. ……我设置了暴力测试，将解码后的密码与已知用户账户进行匹配。

<img src="../assets/difficulty5/leaked_access_log_3.png" alt="Burp suite Brute Force" width="500px">

### 步骤 7：成功登录

成功使用以下邮箱登录 Juice Shop 应用：
```
J12934@juice-sh.op
```
并使用从访问日志中获取的密码。这证实泄露的凭据仍然有效，且属于该用户账户。

## 解题说明

本挑战通过拼合来自多个在线平台的信息并进行推理，找到了并使用泄露的凭据。成功利用这些凭据凸显了保护日志输出，以及监控开发环境中敏感信息的分享位置和方式的重要性。

## 建议

- **日志管理：** 确保日志机制不会意外记录或显示敏感信息，尤其是在生产环境中。
- **开发者培训：** 教育开发者了解在公共论坛上分享潜在敏感信息的风险。
