# Juice-Shop 攻略：Bjoern 最喜欢的宠物

## 挑战概述

**标题：** Bjoern 最喜欢的宠物\
**类别：** OSINT（开源情报）\
**难度：** ⭐⭐⭐ (3/6)

“Bjoern 最喜欢的宠物”挑战要求运用开源情报，查找与 Bjoern 有关的个人信息，具体来说是找出他最喜欢的宠物的名字，并将其用作安全问题的答案。

## 使用的工具

- **Web 浏览器**：访问管理面板和社交媒体平台。
- **OSINT 技术**：利用公开数据收集个人信息。

## 方法与解决方案

### 收集初始信息

1. **查找 Bjoern 的电子邮箱**：
   - 访问应用的管理面板，获取 Bjoern 的电子邮箱：`bjoern@owasp.org`。

    <img src="../assets/difficulty3/bjoern_favorite_pet_1.png" alt="bjoern account on admin panel" width="500px">

### 运用 OSINT 查找宠物名字

2. **查看社交媒体**：
   - 使用 Bjoern 的电子邮箱搜索其社交媒体资料，并找到他的 Twitter 账号。
   - 在 Twitter 上找到 Bjoern 发布的一条动态，其中有一只名叫 Zaya 的猫。

    <img src="../assets/difficulty3/bjoern_favorite_pet_2.png" alt="post twitter" width="500px">

### 利用收集到的信息

3. **回答安全问题**：
   - 找到 Bjoern 的猫的名字后，在重置或访问其账户时，将“Zaya”用作关联安全问题的答案。

    <img src="../assets/difficulty3/bjoern_favorite_pet_3.png" alt="answer security" width="500px">

### 解决方案说明

通过运用 OSINT 技巧找出 Bjoern 的宠物名字，成功绕过了与个人账户相关的安全控制。将从管理面板获得的电子邮箱与社交媒体上的个人内容关联起来，也体现了谨慎管理线上个人信息的重要性。

## 修复建议

在真实环境中，可采取以下措施缓解类似漏洞：

- **避免使用个人信息作为安全问题**：鼓励或要求用户选择不依赖公开数据、且难以猜测的安全问题。更好的做法是不使用安全问题，而是在用户需要重置密码时向其发送电子邮件。
- **隐私设置**：建议用户使用社交媒体平台的隐私设置控制内容的可见范围，尤其是在分享个人或潜在敏感信息时。
