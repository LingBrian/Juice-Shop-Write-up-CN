# Juice-Shop 攻略：绕过 CAPTCHA

## 挑战概述

**标题：** 绕过 CAPTCHA\
**类别：** 自动化防护失效\
**难度：** ⭐⭐⭐ (3/6)

“绕过 CAPTCHA”挑战要求绕过 CAPTCHA 防护，在短时间内提交多条客户反馈。

## 使用的工具

- **Web 浏览器**：与反馈提交表单交互。
- **HTTP 拦截工具**（例如 Burp Suite）：捕获并修改 HTTP 请求。

## 方法与解决方案

### 首次提交反馈

1. **提交反馈**：
   - 前往反馈提交表单，填写评论和评分，并像平常一样完成 CAPTCHA 验证。点击提交发送反馈。

    <img src="../assets/difficulty3/captcha_bypass_1.png" alt="dummy feedback" width="500px">

### 拦截并分析

2. **捕获提交请求**：
   - 使用 Burp Suite 拦截提交反馈时发出的 HTTP POST 请求，并分析请求载荷和标头。

### 找出 CAPTCHA 机制的缺陷

3. **分析 CAPTCHA 的实现**：
   - 注意到请求中的 CAPTCHA 验证仅依赖 `captchaId` 和提供的 `captcha` 答案。
   - 判断 CAPTCHA 系统可能没有有效跟踪或验证先前的提交，因此可能允许重复使用同一个 CAPTCHA 答案。

    <img src="../assets/difficulty3/captcha_bypass_2.png" alt="request for captcha" width="500px">

### 自动重复使用 CAPTCHA

4. **重放请求**：
   - 修改被拦截的 POST 请求，重复使用同一个 `captchaId` 和 `captcha` 答案。更改 `comment` 等其他字段，模拟不同的反馈内容。
   - 在 20 秒内快速重放修改后的请求多次（超过 10 次），以测试自动化防护措施。

    <img src="../assets/difficulty3/captcha_bypass_3.png" alt="modified request" width="500px">

### 验证挑战完成

5. **确认多次提交**：
   - 快速提交多条反馈后，检查应用响应或控制面板，确认所有反馈均被接受，且无需重新完成 CAPTCHA 验证。

### 解决方案说明

通过利用 CAPTCHA 机制的弱点成功完成挑战：同一个已验证的 CAPTCHA 答案可以被重复使用。这说明应用的自动化防护逻辑存在缺陷，因为系统理应跟踪并验证每次 CAPTCHA 尝试，以防止滥用。

## 修复建议

在真实应用中，可采取以下措施防止此类漏洞：

- **增强 CAPTCHA 的健壮性**：实现能够跟踪尝试次数的 CAPTCHA 系统，并确保每个挑战仅可用于一次提交。
- **改进 CAPTCHA 逻辑**：考虑使用 reCAPTCHA 等更复杂的 CAPTCHA 方案，其中包含高级风险分析。
- **速率限制**：对表单提交实施速率限制，以降低自动化攻击风险。
- **基于会话的验证**：确保 CAPTCHA 验证与用户会话绑定，并确保每个会话只能使用一次 CAPTCHA 答案。
