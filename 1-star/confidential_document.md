# Juice-Shop Write-up：Confidential Document Challenge

## 挑战概述

**Title:** Confidential Document\
**Category:** Sensitive Data Exposure\
**Difficulty:** ⭐ (1/6)

本挑战需要从 FTP 服务器访问并获取名为 `acquisition.md` 的机密文档。它揭示了因访问控制配置不当而导致敏感数据暴露的问题。

## 使用的工具

- Web browser

## 方法与解决方案

完成挑战的步骤如下：

1. **访问 FTP 服务器**：使用 Web browser 访问托管在 `127.0.0.1:3000/ftp` 的 FTP 服务器。
2. **识别目标文件**：在服务器文件列表中找到 `acquisition.md`。从文件名推测，它可能包含通常属于机密内容的公司收购信息。
3. **下载并查看文件**：从服务器下载 `acquisition.md` 并打开，确认其中包含其他公司的计划收购信息。
4. **分析内容**：文档明确标注为机密且不得传播，其中包含可能影响股市表现的战略商业计划。

<img src="../assets/difficulty1/confidential_document_1.png" alt="confidential" width="500px">

## 解决方案说明

获取 `acquisition.md` 表明敏感文档未得到充分保护。该文件存放在可公开访问的 FTP 服务器上，且没有适当限制，因此遭到未授权访问。

## 修复建议

本挑战提示了防止类似泄露的修复思路。建议如下：

- **限制访问**：对包含敏感信息的目录实施严格访问控制，并使用身份验证确保只有授权用户可以访问。
- **移除公开访问的敏感文件**：审计服务器，确保没有敏感文件存放在公开可访问的位置。
- **服务器配置**：修改服务器配置，阻止提供预期公开内容之外的文件。建议的代码修改如下：
  ```javascript
  app.use('/ftp', serveIndexMiddleware, serveIndex('ftp', { icons: true })),
  app.use('/ftp(?!/quarantine)/:file', fileServer()),
  app.use('/ftp/quarantine/:file', quarantineServer())
  ```
  其目的是只提供不包含敏感商业文档的静态内容。
- **定期审计与监控**：定期审计服务器内容和访问日志，以发现并响应未授权访问尝试。
