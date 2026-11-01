# Juice-Shop 攻略：登录 Uvogin

## 挑战概述

**标题：** Login Uvogins  
**类别：** 身份验证失效  
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求利用公开来源收集的信息和安全问题，登录 Juice Shop 中 Uvogin 的账户。

## 使用的工具

- **Web 浏览器**：用于访问 Juice Shop 和社交媒体平台。
- **社交媒体平台**：尤其是 Twitter，用于收集 Uvogin 的个人喜好线索和安全问题答案。

## 方法与解法

### 步骤 1：确定 Uvogin 的邮箱和安全问题

- **邮箱：** uvogin@juice-sh.op
- **安全问题：** "你最喜欢的电影是什么？"

### 步骤 2：分析 Uvogin 的写作风格

1. **分析 Leet Speak**：
   - Uvogin 的评论一贯使用 Leet Speak，以数字和特殊字符替换字母。

   <img src="../assets/difficulty4/login_uvogin_1.png" alt="leet 1" width="500px">

   <img src="../assets/difficulty4/login_uvogin_2.png" alt="leet 2" width="500px">

   <img src="../assets/difficulty4/login_uvogin_3.png" alt="leet 3" width="500px">

   - 诸如“0 stars for this horrible security”的评论体现了他的不满，也暗示了其独特的表达风格。

### 步骤 3：利用 OSINT

2. **在线搜索**：
   - 根据有关重复使用别名和独特写作习惯的提示，我在线搜索了 `Uv0g1n`，重点查看 Twitter 等平台。

   <img src="../assets/difficulty4/login_uvogin_4.png" alt="twitter of uv0g1n" width="500px">

3. **找到相关账户**：
   - 找到了一个用户名为 `Uv0gin` 的 Twitter 个人资料，看起来是提及动漫《Hunter x Hunter》的粉丝账户，但答案并不在这里。
   - Uv0gin 对一条推文的回复提供了关键线索，其中称赞了电影 "Silence of the Lambs"：

   <img src="../assets/difficulty4/login_uvogin_5.png" alt="reponse tweet" width="500px">

### 步骤 4：测试安全问题答案

4. **重置密码**：
   - 使用从推文中得到的答案 "Silence of the Lambs"，通过 Juice Shop 的密码重置功能成功重置了 Uvogin 的密码。

   <img src="../assets/difficulty4/login_uvogin_6.png" alt="answer to security question" width="500px">

5. **登录账户**：
   - 重置密码后，使用新凭据成功访问 Uvogin 的账户。

## 解法说明

本挑战通过将 Uvogin 独特的 Leet Speak 风格与其社交媒体活动联系起来，从 Twitter 帖子中找到他最喜欢的电影，并用其回答 Juice Shop 的安全问题。这说明公开平台上看似无关紧要的个人信息也可能危及账户安全。

### 安全建议

- **使用更强的安全问题**：选择更复杂且不易预测的安全问题。
- **注意网络公开信息**：让用户了解在网上分享过多个人信息的风险。
- **不要使用安全问题**：最佳安全实践是不使用安全问题重置密码，而是向用户发送电子邮件，确认其确实希望更改密码。
