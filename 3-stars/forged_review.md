# Juice-Shop 攻略：伪造评论

## 挑战概述

**标题：** 伪造评论\
**类别：** 访问控制失效\
**难度：** ⭐⭐⭐ (3/6)

本挑战通过操纵 Web 应用中的评论提交流程，以其他用户的身份发布评论，利用了访问控制薄弱的问题。

## 使用的工具

- Burp Suite（用于拦截和修改 HTTP 请求）

## 方法与解决方案

### 识别漏洞

1. **提交评论**：先通过 Web 应用界面提交一条普通评论。
2. **拦截请求**：使用 Burp Suite 拦截提交评论时发出的 HTTP 请求。

<img src="../assets/difficulty3/forged_review_1.png" alt="dummy review" width="500px">

3. **分析请求参数**：注意请求载荷包含 `author` 参数，该参数似乎可以手动设置评论作者。这表明服务器没有验证提交评论的用户是否与请求中指定的实际作者相符。

### 利用漏洞

1. **修改作者参数**：将 HTTP 请求中的 `author` 参数改为另一位用户的名称或标识符。

<img src="../assets/difficulty3/forged_review_2.png" alt="modified request" width="500px">

2. **重新发送请求**：使用 Burp Suite 将修改后的请求发送回服务器。
3. **验证评论提交**：在应用中查看评论是否以更改后的作者名发布，以确认漏洞存在。

<img src="../assets/difficulty3/forged_review_3.png" alt="result" width="500px">

### 其他操纵方式

- **编辑已有评论**：发现也可以通过拦截并以类似方式操纵编辑请求来修改评论。

<img src="../assets/difficulty3/forged_review_5.png" alt="edition request" width="500px">

- **查找评论 ID**：可以通过“点赞”等其他功能获取评论 ID；拦截相关请求即可看到被评论消息的 ID。

<img src="../assets/difficulty3/forged_review_6.png" alt="get id from like" width="500px">

- **按 ID 操纵评论**：取得 ID 后，将其注入编辑请求即可修改其他用户的评论内容，而无需成为原作者。

<img src="../assets/difficulty3/forged_review_7.png" alt="modified request" width="500px">

- **结果**：

<img src="../assets/difficulty3/forged_review_8.png" alt="result final" width="500px">

## 解决方案说明

本挑战揭示了与输入验证和会话管理有关的严重安全缺陷：
- **缺少服务器端验证**：服务器没有验证已登录用户是否为评论的 `author`。
- **不安全的直接对象引用（IDOR）**：应用没有妥善保护评论 ID，攻击者只需知道 ID 就能修改任意评论。

## 修复建议

为防范此类漏洞，可采取以下措施：
- **严格的服务器端验证**：确保服务器验证请求用户是否为待修改账户或内容的所有者。可通过检查会话令牌与用户 ID 的关联来实现。
- **使用安全的会话处理机制**：实施健全的会话管理，将用户与其操作安全关联，并防止参数操纵。
- **基于角色的访问控制**：在允许修改数据之前，实施访问控制以验证用户的角色和权限。
