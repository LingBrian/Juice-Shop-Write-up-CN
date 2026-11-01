# Juice-Shop Write-Up: 重置 Bjoern 的密码

## 挑战概述

**标题：** Reset Bjoern's Password\
**类别：** OSINT/取证\
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求通过正确回答关于邮政编码的安全问题，重置 `juice-sh.op` 平台上 Bjoern Kimminich 账户的密码。挑战利用 OSINT 技术，从各种个人资料和社交媒体页面收集信息，找出正确的邮政编码。

## 使用工具

- **Web 浏览器：** 用于访问和分析社交媒体及个人网站上的信息。
- **OSINT 技术：** 用于收集、关联和分析公开信息。

## 方法与解题过程

### 收集信息

1. **分析个人资料：**

   - 安全问题如下：

   <img src="../assets/difficulty5/reset_bjoern_password_1.png" alt="security question" width="500px">

   - 检查 Bjoern Kimminich 的社交媒体资料和个人网站，收集其所在地及相关邮政编码等公开信息。

   Twitter : https://twitter.com/bkimminich?lang=fr
   Portfolio : https://bkimminich.github.io/
   Facebook : https://www.facebook.com/bjoern.kimminich/

   - 记录可能与特定邮政编码有关的重要人生事件、求学地点和工作经历：

   <img src="../assets/difficulty5/reset_bjoern_password_2.png" alt="facebook" width="500px">

   - 收集到的邮政编码如下：

        - 25436
        - 65479
        - 25337

### 关联与假设

2. **邮政编码历史数据：**
   - 我们从 Bjoern 的资料中找到了当前邮政编码（Uetersen 的 25436）。该编码不起作用，但我在论坛上看到德国曾对邮政编码管理方式进行重大更新。
   - 因此，我使用工具根据新邮政编码查找旧邮政编码：`https://www.alte-postleitzahlen.de`，考虑到安全问题可能要求填写已过时的编码。

### 测试假设

3. **逐个尝试：**
   - 使用账户恢复选项测试所有收集到的邮政编码，看哪个能正确回答安全问题。
   - 同时测试当前和历史邮政编码。

### 成功重置

4. **重置密码：**
   - 发现历史邮政编码“West-2082”（Uetersen 改革前的编码）是安全问题的正确答案，因此可以重置密码。

## 解题说明

通过分析公开信息，找到了与 Bjoern 的生活和职业经历相关的候选邮政编码。关键在于意识到安全问题可能涉及过时的邮政编码，并将当前信息与历史邮政编码数据关联，找出符合要求的答案。

## 修复建议

为防范类似攻击：

- **设置复杂的安全问题：** 选择无法轻易通过公开信息回答的安全问题。
- **减少个人信息暴露：** 注意在社交媒体和个人网站公开的个人信息数量。
- **移除安全问题：** 安全问题并不可靠，可改为向申请重置密码的用户发送电子邮件。
