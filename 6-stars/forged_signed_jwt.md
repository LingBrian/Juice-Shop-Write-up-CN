# Juice-Shop Write-up: 伪造已签名 JWT

## 挑战概述

**标题：** Forged Signed JWT  
**类别：** 易受攻击的组件  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战的目标是创建一个看似由 RSA 正确签名、实际却利用算法混淆攻击绕过安全措施的伪造 JWT。

## 使用工具

- **jwt.io：** 用于解码和编码 JWT。
- **Web 浏览器：** 用于从 Juice Shop 服务器获取 RSA 公钥。
- **命令行工具：** 例如 `curl` 或 `wget`，用于与 Juice Shop API 交互。
- **在线 HMAC 生成器：** 用于计算 HMAC 签名。

## 方法与解题过程

### 步骤 1：获取现有 JWT

获取一个已知用户（例如 `admin@juice-sh.op`）的 JWT，将其作为伪造模板。例如：

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJzdGF0dXMiOiJzdWNjZXNzIiwiZGF0YSI6eyJpZCI6MSwidXNlcm5hbWUiOiIiLCJlbWFpbCI6ImFkbWluQGp1aWNlLXNoLm9wIiwicGFzc3dvcmQiOiIwMTkyMDIzYTdiYmQ3MzI1MDUxNmYwNjlkZjE4YjUwMCIsInJvbGUiOiJhZG1pbiIsImRlbHV4ZVRva2VuIjoiIiwibGFzdExvZ2luSXAiOiJ1bmRlZmluZWQiLCJwcm9maWxlSW1hZ2UiOiJhc3NldHMvcHVibGljL2ltYWdlcy91cGxvYWRzL2RlZmF1bHRBZG1pbi5wbmciLCJ0b3RwU2VjcmV0IjoiIiwiaXNBY3RpdmUiOnRydWUsImNyZWF0ZWRBdCI6IjIwMjQtMDUtMDIgMDc6MDM6MTcuODIyICswMDowMCIsInVwZGF0ZWRBdCI6IjIwMjQtMDUtMDIgMDk6MjA6MzIuNTU4ICswMDowMCIsImRlbGV0ZWRBdCI6bnVsbH0sImlhdCI6MTcxNDY0MTY4N30.Yb2VAr9NGwR9c7WGUsKQaCKMI8sJ_Vg0mAN2pnpe1q7yj68em_X-RqPBLNFutP8eceQV5xbrb-lo5kFKlM-ympEypusk38_8koIUWO2Gip1quy_aiDEtuaq6cLK-2r7vaN-cds8cHe8c4Rg1HkG9hINPw6neYw9k0Tcy6dfAVRc
```

### 步骤 2：分析 JWT 结构

使用 jwt.io 解码 JWT，了解其结构、Header、Payload 和签名方法。如果已经完成未签名 JWT 挑战，就熟悉了这一结构；否则建议先从该挑战开始。

### 步骤 3：获取 RSA 公钥

由于之后要为构造的恶意 JWT 签名，并且挑战提示也有所暗示，我们必须先获取 JWT 的签名密钥。请先查看挑战提示：

<img src="../assets/difficulty6/forged_signed_jwt_1.png" alt="hints" width="500px">

在开始 Juice Shop 学习时，我曾使用 GoBuster 扫描 Web 应用。在侦察过程中找到了 URL `http://127.0.0.1:3000/encryptionkeys`，其中列出了 JWT 公钥文件（`jwt.pub`）。因此我下载该文件，以便之后伪造签名。

<img src="../assets/difficulty6/forged_signed_jwt_3.png" alt="jwt file" width="500px">

### 步骤 4：准备伪造 JWT

在 jwt.io 中修改 JWT 的 Payload，冒充一个不存在的用户（`rsa_lord@juice-sh.op`）。

我们只有公钥，可能会疑惑：服务器的私钥才用于签名，如何用公钥签名 JWT？JWT 存在多种可利用方式，其中一种是将 Header 中的算法从非对称算法改为对称算法。如果服务器配置错误，就会接受这种修改，进而允许使用公钥为 JWT 签名。这种攻击称为“算法混淆攻击”。

为此，我们将 JWT Header 中的 `alg` 从 `RS256` 改为 `HS256`，利用 JWT 实现中常见的这一漏洞。

### 步骤 5：使用 RSA 公钥通过 HMAC 签名

接下来为 JWT 签名。再次建议手动操作以避免错误。我们将 RSA 公钥当作密钥，为 JWT 生成 HMAC-SHA256 签名。

<img src="../assets/difficulty6/forged_signed_jwt_4.png" alt="generator" width="500px">

如前所述，在对称 HMAC 过程中误用非对称密钥是算法混淆攻击的核心。最后，用新签名替换旧签名：

<img src="../assets/difficulty6/forged_signed_jwt_5.png" alt="JWT full" width="500px">

### 步骤 6：将伪造 JWT 发送给服务器

在发往 `/rest/user/whoami` 的请求中，用新伪造的 JWT 替换现有 JWT。服务器将 HMAC 签名误认为有效的 RSA 签名，并授予了 `rsa_lord@juice-sh.op` 的访问权限。

<img src="../assets/difficulty6/forged_signed_jwt_7.png" alt="result" width="500px">

## 解题说明

本挑战利用了常见的 JWT 实现缺陷——“算法混淆攻击”。将 JWT Header 中的签名算法改为 HMAC-SHA256（`HS256`），再把 RSA 公钥当作 HMAC 密钥为 token 签名，即可绕过原本使用 RSA 签名验证 token 的身份验证机制。

## 修复建议

- **严格限制算法：** 确保服务器强制使用预期的 JWT 签名算法，并拒绝指定其他算法的 token。
- **更新库：** Juice-Shop 当前使用的 express-jwt 版本较旧，存在包括此类问题在内的许多安全问题。
