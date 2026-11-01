# Juice-Shop Write-up：Locate Security Policy File

## 挑战概述

**Title:** Locate Security Policy File\
**Category:** OSINT/Discovery\
**Difficulty:** ⭐⭐ (2/6)

本挑战需要在 Web 服务器上找到隐藏的 `security.txt` 文件。`Security.txt` 是一项拟议标准，用于让网站公布安全政策。

## 使用的工具

- **Web Browser**：用于访问 URL 并查看 Web 内容。

## 方法与解决方案

### 背景研究

1. **了解 `security.txt` 概念**：
   - 初步研究发现，`security.txt` 是由 Internet Engineering Task Force (IETF) 提出的草案，旨在帮助组织更方便地披露安全实践。按照该草案指南，建议网站将此文件放在 `/.well-known/` 目录下（https://securitytxt.org/）。

### 搜索策略

2. **检查标准目录**：
   - 根据研究结果，此类文件的标准存放位置是 `/.well-known/` 目录。Web 服务通常使用该目录存放网站的各种元数据。

3. **访问文件**：
   - 在 Web browser 中直接访问 `http://127.0.0.1:3000/.well-known/security.txt`。
   - 成功找到 `security.txt` 文件，其中包含联系信息、加密详情、安全沟通的首选语言以及招聘信息。

   <img src="../assets/difficulty2/white_hat_1.png" alt="file" width="500px">

### 解决方案说明

本挑战利用了 Web 应用通常存放安全政策文件的位置知识。遵循 securitytxt.org 中列出的标准后，很容易找到该文件，也说明了熟悉 Web 安全实践和标准的重要性。

## 修复建议

对于真实应用：
- **实施 `security.txt`**：组织设置 `security.txt` 文件有助于沟通安全问题。建议将文件放置在 `/.well-known/` 目录中。
- **安全访问**：确保通过 `security.txt` 披露的任何敏感信息不会意外带来其他安全风险。
- **定期更新**：保持 `security.txt` 中的信息为最新，包括联系信息和加密密钥。
