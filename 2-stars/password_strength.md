# Juice-Shop Write-up：Password Strength

## 挑战概述

**Title:** Password Strength\
**Category:** Broken Authentication\
**Difficulty:** ⭐⭐ (2/6)

“Password Strength”挑战探讨弱密码策略带来的漏洞，重点是管理员弱密码容易遭到暴力破解。本挑战强调实施可靠密码策略以保护身份验证机制的重要性。

## 使用的工具

- **Web Browser**：用于与身份验证界面交互。
- **Burp Suite**：用于拦截 Web 请求并执行暴力破解攻击。

## 方法与解决方案

### 识别管理员凭据

1. **查找管理员电子邮件**：
   - 搜索网站中可见或隐藏的管理员电子邮件信息。网站源代码中的注释暴露了该电子邮件地址：`admin@juice-sh.op`。

   <img src="../assets/difficulty2/password_strength_1.png" alt="email" width="500px">

### 暴力破解攻击

2. **拦截登录请求**：
   - 使用管理员电子邮件和随机密码尝试登录。
   - 使用 Burp Suite 拦截登录请求，捕获暴力破解所需的请求详情。

   <img src="../assets/difficulty2/password_strength_2.png" alt="login capture" width="500px">

3. **在 Burp Suite 中设置暴力破解**：
   - 配置 Burp Suite 的 Intruder 模块，使用常见密码列表，在攻击期间用列表中的每个条目替换占位密码。

   <img src="../assets/difficulty2/password_strength_3.png" alt="request" width="500px">

4. **执行攻击**：
   - 运行暴力破解攻击，并监控响应中是否出现身份验证成功的迹象。
   - 使用密码 `admin123` 成功通过身份验证。

   <img src="../assets/difficulty2/password_strength_4.png" alt="result" width="500px">

### 解决方案说明

由于应用采用了弱密码策略，允许使用常见且容易猜到的密码，因此暴力破解攻击得以奏效。本挑战通过发现并利用该漏洞，未授权访问了管理员账户。

## 修复建议

为防止真实应用中出现此类漏洞，可考虑以下步骤：

- **强密码策略**：强制实施强密码策略，要求密码包含大写字母、小写字母、数字和特殊字符，长度至少为 8 个字符。
- **密码复杂度与黑名单**：检查密码是否属于常见密码或公开泄露事件中出现的已泄露密码。
- **速率限制**：限制来自单个 IP 地址或账户的登录失败次数，并在达到上限后强制进入冷却期，以防止暴力破解攻击。
- **多因素身份验证 (MFA)**：要求使用密码之外的第二种验证方式，增加额外安全层。

如需进一步了解密码安全最佳实践，请参阅 NIST Special Publication 800-63B，并查看 OWASP 身份验证指南等资源：https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html。
