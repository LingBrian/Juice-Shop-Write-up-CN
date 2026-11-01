# Juice-Shop Write-up：Repetitive Registration Challenge

## 挑战概述

**Title:** Repetitive Registration\
**Category:** Improper Input Validation\
**Difficulty:** ⭐ (1/6)

“Repetitive Registration”挑战探讨注册过程中用户输入验证不当的问题，重点关注密码确认字段的处理。

## 使用的工具

- Web browser
- Developer tools

## 方法与解决方案

### 注册表单分析

检查注册表单时发现，用户需要输入两次密码。这是一种常见做法，目的是避免输入错误。不过，挑战提示建议利用“Don't Repeat Yourself”（DRY）原则；在此语境下，这意味着系统不应依赖冗余的用户输入才能正常工作。

### 挑战解决步骤

1. **填写注册表单**：
   - 输入所有必需信息，包括电子邮件、密码和安全问题。
   - 故意让“Password”和“Re-type your password”字段的内容不同。

   <img src="../assets/difficulty1/repetitive_registration_1.png" alt="incorrect" width="500px">

2. **修改表单行为**：
   - 使用浏览器开发者工具检查“Sign Up”按钮，发现密码不一致时该按钮处于禁用状态。
   - 通过开发者工具手动从 HTML 中移除按钮的 `disabled` 属性。

   <img src="../assets/difficulty1/repetitive_registration_2.png" alt="code" width="500px">

3. **提交表单**：
   - 启用按钮后提交表单。尽管密码不匹配，注册仍然成功，说明服务器端没有对此条件进行验证。

### 解决方案说明

本挑战通过利用服务器端未验证密码确认字段的缺陷而解决。应用客户端最初会阻止密码不匹配时提交表单，但通过修改客户端 HTML 属性即可轻易绕过检查，暴露出严重的安全问题。

## 修复建议

为防止真实应用中出现此类漏洞，除了客户端验证外，还必须实施服务器端检查。建议如下：

- **服务器端验证**：确保客户端执行的所有输入验证也在服务器端强制执行，包括密码字段匹配、输入长度限制、格式要求等。
- **安全的表单处理**：表单应拒绝关键字段遭到篡改或不符合应用要求的提交。
- **定期安全审计**：开展全面的安全审计，测试常见漏洞，包括身份验证机制中的输入验证不当问题。
