# Juice Shop Write-up：Privacy Policy Challenge

## 挑战概述

**Title:** Privacy Policy\
**Category:** Miscellaneous\
**Difficulty:** ⭐ (1/6)

本挑战的目标是查阅网站的 Privacy Policy，其中间接提供了 FTP 服务器路径的线索。本挑战考察对细节的关注，以及从看似非技术性的信息中推断线索的能力。

## 使用的工具

- Web browser

## 方法与解决方案

解决 Privacy Policy 挑战的步骤很简单：

1. **找到 Privacy Policy**：前往网站上通常放置法律或隐私相关文档的区域。该页面可能是：127.0.0.1:3000/#/about
2. **检查内容**：打开并仔细阅读 Privacy Policy；本例中，它以 Markdown 文件（`legal.md`）的形式呈现。

<img src="../assets/difficulty1/policy_1.png" alt="policy" width="500px">

## 解决方案说明

只需打开该文件即可。

## 修复建议

在真实场景中，务必确保法律或政策文档不会（有意或无意地）泄露敏感信息。应定期审计和审查所有已发布内容，以防止此类信息泄露。
