# Juice-Shop 攻略：服务器端 XSS 防护

## 挑战概述
**标题：** Server-side XSS Protection  
**类别：** 跨站脚本（XSS）  
**难度：** ⭐⭐⭐⭐ (4/6)  

本挑战的目标是通过绕过服务器端安全机制，利用 `<iframe src="javascript:alert('xss')">` 发起持久型 XSS 攻击。

## 使用的工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **Burp Suite：** 用于拦截和修改 HTTP 请求，测试注入点。
- **文本编辑器：** 用于构造和测试载荷。

## 方法与解法

### 步骤 1：确定存在漏洞的输入
本挑战要求找到一个会在服务器端反映用户输入的输入框。投诉页面适合用于测试：
1. **进入联系页面：** 访问 `http://localhost:3000/#/complaints`。
2. **提交测试消息：** 输入测试消息，并使用 Burp Suite 拦截请求以分析参数。

### 步骤 2：了解服务器端清理机制
服务器上的 `package.json` 文件可通过 `http://localhost:3000/ftp/package.json` 访问，其中显示使用了 1.4.2 版的 "sanitize-html" 库。

### 步骤 3：确定 "sanitize-html" 的漏洞
调查 "sanitize-html" 库后，我们在 GitHub 的 issue #29 中发现了一个漏洞（https://github.com/apostrophecms/sanitize-html/issues/29）：
- **漏洞说明：** 清理操作不会递归执行，因此某些遮蔽攻击可以绕过清理流程。
- **Issue 中的示例：**
  ```html
  I am not harmless: <<img src="csrf-attack"/>img src="csrf-attack"/> 
  ```
  清理后变为：
  ```html
  I am not harmless: <img src="csrf-attack"/>
  ```

### 步骤 4：构造 XSS 载荷
要利用此漏洞，需要构造能利用不完整清理机制的载荷：
- **载荷：**
  ```html
  <<script>Foo</script>iframe src="javascript:alert('xss')">
  ```

### 步骤 5：注入载荷
1. **输入框：** 在投诉表单的消息字段中注入载荷。
3. **提交载荷：** 确保载荷成功提交。

### 步骤 6：验证 XSS 执行
1. **访问反映输入的页面：** 打开输入内容被反映的页面（例如管理员查看联系消息的页面）。
2. **观察执行结果：** 确认是否出现内容为 `xss` 的警告框，以验证 XSS 攻击成功。

## 解法说明
本挑战利用了 "sanitize-html" 库不会递归清理 HTML 的漏洞。构造利用此缺陷的载荷后，我们绕过清理机制并执行了持久型 XSS 攻击。

## 修复建议
- **更新库：** 使用已修复递归清理问题的新版 "sanitize-html"。
- **输入验证和清理：** 增加输入验证与清理层，确保所有输入都符合要求。
- **输出编码：** 在页面渲染用户输入时始终进行编码，以防 XSS。
