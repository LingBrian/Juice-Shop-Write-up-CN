# Juice-Shop Write-up: 登录支持团队账户

## 挑战概述

**标题：** Login Support Team  
**类别：** 安全配置错误  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战要求使用支持团队的原始凭据访问其账户，不得使用 SQL 注入或其他绕过方法。

## 使用工具

- **Web 浏览器**：用于访问 Juice Shop 平台及其各项功能。
- **KeePass**：免费开源密码管理器，可帮助安全地管理密码。
- **John the Ripper**：常用的密码破解软件。
- **keepass2john**：用于从 KeePass 数据库提取哈希值以供破解的工具。

## 方法与解题过程

### 步骤 1：识别并下载关键文件

完成挑战时，提示很重要：

<img src="../assets/difficulty6/login_support_team_1.png" alt="alt text" width="500px">

- 我们发现团队使用外部密钥管理器存储凭据，最常用的此类工具之一是 KeePass。
- 不能使用 SQL 注入。
- 不能暴力破解密码。

既然看起来需要找到密码，我开始考虑它可能存储在某个文件中。在 Juice Shop FTP 服务器上，我找到了并下载了 `incident-support.kdbx` 文件，这是支持团队的 KeePass 数据库。如果你不知道 KDBX 文件是什么，它就是 KeePass 密钥管理器的数据库（KDB 表示 KeePass Database）。供参考，KeePass 几年前进行过重大更新；在此之前使用的是 .kbd 文件，而非 .kdbx。该数据库使用 Twofish 算法加密，因此要访问它，我们必须先获取主密钥。

### 步骤 2：转换 KeePass 数据库

幸运的是，可以对 KeePass 数据库进行离线暴力破解（速度非常快）。这里使用 John the Ripper。首先要将文件转换成 John the Ripper 可接受的格式。

使用 `keepass2john` 将 `.kdbx` 文件转换为哈希格式：

```bash
keepass2john incident-support.kdbx > keepasshash.txt
```

<img src="../assets/difficulty6/login_support_team_6.png" alt="oupout" width="500px">

输出提供了可供破解的哈希：
```bash
incident-support:$keepass$*2*1*0*2360b9f8557b8c25a0dbb36a1610958a083caf96fddf7ee3a164e3659b80e136*34e52e0601bbd92ed7b65f2d86a6832b81368cbdfc6125b5639134f98911ab93*2231292f58a92775b0c08b7a46c682b6*38c10e8133656d9c5d10c4bc9c8829449749b526732ea88eb72e2c327b9ea4de*1f1098d8a24cc7bf23f0a58542fd1851662a4cab8f2422d6541658521f199bd5
```

### 步骤 3：破解密码

使用 John the Ripper 和 `rockyou.txt` 字典尝试破解提取出的哈希：

```bash
john --wordlist=rockyou.txt keepasshash.txt
```

这次尝试未成功，看来密码不在该字典中。我尝试在 main.js 中搜索“support”，以了解更多密码线索，并找到一个与支持团队密码重置相关的部分。

<img src="../assets/difficulty6/login_support_team_8.png" alt="support reset section" width="500px">

我们还发现一条用户操作错误时可能出现在控制台中的错误消息。这条消息经过了混淆：

<img src="../assets/difficulty6/login_support_team_9.png" alt="error message obfusacted" width="500px">

消息为罗马尼亚语，使用 Google 翻译后内容是：“支持团队密码不符合特权账户的公司策略！请相应地更改密码！”

我们在代码中搜索该策略，并在同一行找到一个正则表达式，说明支持团队新密码必须包含或不能包含哪些字符：

```
/(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{12,30}/
```

我们可以编写 Python 脚本，根据 `{大写字母}{字母}{数字}{字符}` 的形式生成字典。

最终找到密码 `Support2022!`。

### 步骤 4：提取凭据

破解密码后，我用它解锁 KeePass 数据库，提取支持团队账户的凭据。

<img src="../assets/difficulty6/login_support_team_11.png" alt="opened keepass" width="500px">

凭据如下：

<img src="../assets/difficulty6/login_support_team_12.png" alt="credentials" width="500px">

### 步骤 5：登录支持团队账户

使用从 KeePass 数据库中提取的凭据：J6aVjTg0pRs@?5l!Zkq2AYnCE@RF$P
我以支持团队成员身份登录了 Juice Shop 平台。

## 解题说明

支持团队凭据安全地存储在 KeePass 数据库中，并由强密码保护。通过使用 John the Ripper 和自定义字典进行密码破解，我解锁了数据库并获取了完成挑战所需的凭据。

## 修复建议

- **保护敏感文件**：确保密码数据库等敏感文件不会通过 FTP 服务器等公开或安全性较低的渠道访问。
- **使用强访问控制**：实施健全的访问控制，并监控访问日志，防止未经授权访问敏感信息。
