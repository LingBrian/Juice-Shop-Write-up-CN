# Juice-Shop Write-up：DOM XSS

## 挑战概述

**Title:** DOM XSS\
**Category:** Cross-Site Scripting (XSS)\
**Difficulty:** ⭐ (1/6)

“DOM XSS”挑战要求通过未过滤的输入字段，向 Web 应用的 Document Object Model 注入恶意 JavaScript 代码，以执行基于 DOM 的 XSS 攻击；具体目标是 `iframe` 的 source 属性。

## 使用的工具

- **Web Browser**：用于执行攻击并观察其效果。

## 方法与解决方案

### 漏洞利用概述

本挑战需要将 XSS payload 注入 Web 应用的动态内容区域。目标是找到会直接影响 DOM、且未经过适当净化的输入参数。

### 挑战解决步骤

1. **找到注入点**：
   - 发现应用的搜索功能可能成为 XSS 攻击途径。通过 URL 中的搜索参数 `q` 操纵 DOM。

2. **构造 Payload**：
   - 使用的 XSS payload 为：`<iframe src="javascript:alert(`xss`)">`。该脚本会直接从 iframe 的 source 属性执行 JavaScript，触发 XSS 弹窗。

3. **编码并注入 Payload**：
   - 对 payload 进行 URL 编码，以绕过基本 URL 解析，然后通过搜索 URL 将其注入应用：`http://127.0.0.1:3000/#/search?q=%3Ciframe%20src=%22javascript:alert(%60xss%60)%22%3E`

4. **执行攻击**：
   - 访问修改后的 URL 后，恶意脚本成功执行，并显示弹窗，表明 XSS 攻击成功。

   <img src="../assets/difficulty1/dom_xss_1.png" alt="alt text" width="500px">

### 解决方案说明

本挑战通过在会影响 DOM 的 URL 参数中，经由 iframe 的 source 属性注入并执行 JavaScript 代码而解决。这展示了一个典型的基于 DOM 的 XSS 案例：用户输入会直接影响 DOM，却未经过适当净化。

## 修复建议

为防止此类基于 DOM 的 XSS 攻击，应采取以下措施：

- **使用能自动处理 XSS 的框架**：Angular、React 和 Vue.js 等现代框架内置了通过转义或默认净化用户输入来防范 XSS 的机制。确保这些功能已启用并正确配置。
- **净化用户输入**：在客户端和服务器端对输入进行严格验证和净化，避免恶意脚本执行。
- **Content Security Policy (CSP)**：实施 CSP 响应头，限制可执行脚本的来源，降低成功 XSS 攻击造成的影响。

如需了解防止基于 DOM 的 XSS 的详细指南，请参阅 OWASP DOM-based XSS Prevention Cheat Sheet：https://cheatsheetseries.owasp.org/cheatsheets/DOM_based_XSS_Prevention_Cheat_Sheet.html
