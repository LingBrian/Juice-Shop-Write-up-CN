# Juice-Shop Write-up: 解锁高级内容挑战

## 挑战概述

**标题：** Unlock Premium Challenge  
**类别：** 密码学问题  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战要求找到并使用密钥，访问 OWASP Juice Shop 中模拟应用和游戏常见“高级内容”付费墙的锁定区域。目标是在不实际付款的情况下解锁该区域。

## 使用工具

- **Web 浏览器**：用于浏览 Juice Shop 并下载文件。
- **加密密钥目录**：用于获取所需的解密密钥。
- **OpenSSL**：用于解密加密字符串。

## 方法与解题过程

### 步骤 1：找到加密密钥

如前一挑战所述，使用 GoBuster 分析站点时，我找到了加密密钥目录：
```
http://127.0.0.1:3000/encryptionkeys
```
该目录列出了名为 `premium.key` 的文件，我随即将其下载。

<img src="../assets/difficulty6/premium_paywall_2.png" alt="key folder" width="500px">

### 步骤 2：分析密钥

文件内容截图：

<img src="../assets/difficulty6/premium_paywall_3.png" alt="key file" width="500px">

`premium.key` 的内容看起来是十六进制字符串，可能用作加密密钥：

```
1337133713371337.EA99A61D92D2955B1E9285B55BF2AD42
```

这表明它可能用于 AES-128 加密方案。

**注意：** 为节省时间，也可以从此 GitHub 仓库的 files 文件夹中获取该文件。

### 步骤 3：发现加密数据

挑战提示要绕过付款机制，因此我开始在应用中查找隐藏注释或脚本，寻找可以用刚获得的密钥解密的数据。最初，

### 步骤 4：手动搜索并检查 OWASP 指南

我没能直接在网站可访问的源代码中找到加密字符串。查看 OWASP Juice Shop 指南中的解法后，我得知源代码中应该存在一条加密注释，但我完成挑战时并未找到：
```
<!--IvLuRfBJYlmStf9XfL6ckJFngyd9LfV1JaaN/KRTPQPidTuJ7FR+D/nkWJUF+0xUF07CeCeqYfxq+OJVVa0gNbqgYkUNvn//UbE7e95C+6e+7GtdpqJ8mqm4WcPvUGIUxmGLTTAC2+G9UuFCD1DUjg==-->
```

### 步骤 5：解密隐藏 URL

使用 `premium.key` 中的 AES 密钥和 OpenSSL，解密上述 Base64 编码字符串：
```bash
echo "IvLuRfBJYlmStf9XfL6ckJFngyd9LfV1JaaN/KRTPQPidTuJ7FR+D/nkWJUF+0xUF07CeCeqYfxq+OJVVa0gNbqgYkUNvn//UbE7e95C+6e+7GtdpqJ8mqm4WcPvUGIUxmGLTTAC2+G9UuFCD1DUjg==" | openssl enc -d -aes-256-cbc -K EA99A61D92D2955B1E9285B55BF2AD42 –iv 337133713371337 -a –A
```
这揭示了付费墙后隐藏的 URL：
```
http://localhost:3000/this/page/is/hidden/behind/an/incredibly/high/paywall/that/could/only/be/unlocked/by/sending/1btc/to/us
```

### 步骤 6：访问隐藏内容

访问解密得到的 URL 后，即可进入 Juice Shop 的高级内容区域，完全绕过付款机制并完成挑战。

## 解题说明

本挑战通过使用从服务器暴露目录中找到的密钥解密 URL 来解决，绕过了模拟付费墙。这表明公开暴露敏感密钥的风险，以及保护支付相关功能的重要性。

## 修复建议

- **安全存储密钥**：切勿将加密密钥存放在可公开访问的目录中，也不要将其嵌入客户端代码。
- **访问控制**：实施严格的访问控制，防止未经授权访问敏感目录和文件。
