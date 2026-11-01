# Juice-Shop 攻略：报复时刻挑战

## 挑战概述

**标题：** 报复时刻\
**类别：** 业务逻辑错误\
**难度：** ⭐⭐⭐ (3/6)

本挑战通过操纵电商应用中的商品定价，利用业务逻辑缺陷非法获利。

## 使用的工具

- Web 浏览器
- Burp Suite 或用于发送请求的开发者工具

## 方法与解决方案

### 初步步骤与发现

1. **检查应用代码**：首先检查 `main.js` 文件，其中通常包含路径和客户端逻辑。发现路径 `/accounting`，看起来可能与管理操作有关。

2. **尝试访问会计页面**：在浏览器中直接访问 `http://127.0.0.1:3000/#/accounting`，结果进入授权错误页面。

<img src="../assets/difficulty3/payback_time_1.png" alt="403" width="500px">

3. **找出具备所需权限的账户**：

   - 从商品评论中找到可能拥有所需权限的用户邮箱（`accountant@juice-sh.op`）。

    <img src="../assets/difficulty3/payback_time_2.png" alt="email of accountant" width="500px">

   - 利用先前发现的 SQL 注入漏洞获取该账户的访问权限。要准确定位会计账户，SQL 注入语法应使用 `AND` 而不是 `OR`。

    <img src="../assets/difficulty3/payback_time_3.png" alt="SQL Injection" width="500px">

### 利用业务逻辑

1. **以会计身份访问会计页面**：
   - 以会计身份登录后，重新访问会计页面，此时页面会显示商品价格并允许修改。

    <img src="../assets/difficulty3/payback_time_4.png" alt="accoutant panel" width="500px">

2. **尝试修改商品价格**：
   - 尝试直接在用户界面中设置负数价格，系统会将其修正为零。
   - 使用 Burp Proxy 拦截价格更新请求。

    <img src="../assets/difficulty3/payback_time_5.png" alt="update price request" width="500px">

3. **操纵商品定价**：
   - 修改被拦截的请求，将价格参数设为负值。
   - 服务器接受了修改后的请求，并将商品价格更新为负值。

    <img src="../assets/difficulty3/payback_time_6.png" alt="updated product" width="500px">

4. **利用负数定价**：
   - 以负数价格购买商品。
   - 系统处理负金额时，不是扣款而是向账户记入余额，最终实现返款。

    <img src="../assets/difficulty3/payback_time_7.png" alt="purchase" width="500px">

## 解决方案说明

本挑战利用了一系列漏洞和配置错误：
- **授权缺陷**：不当的访问控制允许通过 SQL 注入获得未授权访问。
- **业务逻辑错误**：系统未能妥善处理商品定价输入，允许通过操纵后端提交负数。

## 修复建议

在真实应用中，可采取以下措施防止此类漏洞：
- **正确验证输入**：确保对所有输入（尤其是与金融交易相关的输入）在客户端和服务器端都进行验证，防止遭到操纵。
- **使用 SQL 预处理语句**：使用带参数的预处理语句，避免 SQL 注入。
