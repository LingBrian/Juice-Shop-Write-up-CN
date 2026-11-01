# Juice-Shop 攻略：重置 Jim 的密码

## 挑战概述

**标题：** 重置 Jim 的密码\
**类别：** OSINT\
**难度：** ⭐⭐⭐ (3/6)

“登录 Jim 账户”挑战要求使用 Jim 安全问题的答案重置其密码。

## 使用的工具

- **Web 浏览器**：在线搜索并收集与该角色有关的信息。

## 方法与解决方案

### 调查角色背景

1. **确认 Jim 的身份**：
   - 挑战提示称 Jim 是一位名人，其安全问题的答案可以在公开信息中找到。
   - Jim 在一条评论中引用了《Star Trek》中的台词。
   - 因此可以推测他可能与《Star Trek》有关，于是搜索“Jim Star Trek”。
   - 发现 Jim 指的是《Star Trek》中的 James Tiberius Kirk。

### 分析安全问题

2. **查看安全问题详情**：
   - 以下是 Jim 的安全问题：
   
    <img src="../assets/difficulty3/reset_jim_password_1.png" alt="security question of jim" width="500px">

### 收集相关信息

3. **查找家庭关系**：

   - 搜索 James T. Kirk 的家庭信息，找到安全问题的答案。

    <img src="../assets/difficulty3/reset_jim_password_2.png" alt="security question of jim" width="500px">

   - 查明他的兄弟名叫 George Samuel Kirk。

### 访问账户

4. **测试安全问题答案**：
   - 在账户恢复或登录流程中，将“George Samuel Kirk”用作安全问题的答案：

    <img src="../assets/difficulty3/reset_jim_password_3.png" alt="security question of jim" width="500px">

### 解决方案说明

通过识别 Jim 的虚构身份——《Star Trek》中的 James T. Kirk 船长——成功完成挑战。

## 修复建议

在真实应用中，可采取以下措施防止类似安全问题：

- **选择健全的安全问题**：避免使用答案容易被猜到或查到的问题。应选择答案安全且难以预测的问题。如有可能，应取消安全问题，并在用户需要重置密码时向其发送电子邮件。
- **教育用户**：指导用户选择安全问题及答案，并强调使用容易获取的信息所带来的风险。
- **实施多重身份验证**：要求使用安全问题之外的多种身份验证方式，增强安全性。
