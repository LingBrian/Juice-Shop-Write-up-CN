# Juice-Shop 攻略：Bjoern 登录

## 挑战概述

**标题：** Bjoern Login  
**类别：** 身份验证失效  
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求登录 Juice Shop 的创建者之一 Bjoern 的账户，不直接攻破他的 Google 账户，也不从事非法活动。

## 使用的工具

- **Web 浏览器**：用于与 Juice Shop 应用交互。
- **开发工具**：用于测试和验证 JavaScript 函数。

## 方法与解法

### 步骤 1：分析应用代码

1. **调查 OAuth 实现**：
   - 挑战线索建议检查 Juice Shop 应用过去实现 OAuth 登录的方式。
   - 搜索关键词并阅读 `main.js` 后，我发现应用主 JavaScript 文件中的函数 `ngOnInit()` 负责处理 OAuth 登录。

### 步骤 2：提取 Bjoern 的邮箱

2. **逆向分析密码生成过程**：
   - 在 `ngOnInit()` 中，密码通过用户邮箱以特定方式生成。流程为：
     - 将邮箱字符串拆分为字符数组。
     - 反转数组。
     - 将数组重新连接为字符串。
     - 使用 `btoa()` 对结果进行 Base64 编码。

3. **公式说明**：
   - `const i = btoa(o.email.split("").reverse().join(""));`，其中 `o.email` 是用户邮箱。
   - 密码实际上是邮箱地址的可逆变换，因此每个用户的密码都可根据其邮箱预测。

### 步骤 3：计算 Bjoern 的密码

4. **应用逆向得到的公式**：
   - 已知 Bjoern 的邮箱 `bjoern.kimmich@gmail.com`，密码由反转邮箱并进行 Base64 编码生成：
     ```
     var email = "bjoern.kimmich@gmail.com";
     var reversed = email.split("").reverse().join("");
     var password = btoa(reversed);
     ```
   - 在 JavaScript 控制台中运行该过程即可得到确切密码字符串。

### 步骤 4：登录 Bjoern 的账户

5. **使用推导出的密码**：
   - 在 Juice Shop 登录页面输入邮箱和推导出的密码，即可访问 Bjoern 的账户。

## 解法说明

本挑战利用了 Juice Shop OAuth 用户注册系统的缺陷：该系统根据用户邮箱生成可预测的密码。通过逆向分析这一过程，可以计算出 Bjoern 的密码并访问其账户。

### 安全建议

- **安全地生成密码**：避免使用可预测的密码生成算法，尤其不要在客户端生成。应采用强随机且不可逆的方法。
