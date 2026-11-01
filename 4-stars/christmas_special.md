# Juice-Shop 攻略：圣诞优惠

## 挑战概述

**标题：** Christmas Offer\
**类别：** 注入\
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战的目标是利用 SQL 注入，访问隐藏的圣诞优惠商品；该商品可能已从正常商品列表中移除或隐藏（在数据库中标记为已删除）。

## 使用的工具

- **Web 浏览器**：用于与 Juice Shop 网页界面交互。
- **开发者工具**：用于监控和修改网络流量。

## 方法与解法

### 步骤 1：确定注入点

1. **使用商品搜索**：
   - 我们发现搜索功能 `127.0.0.1:3000/rest/products/search?q=payload` 存在 SQL 注入点，与之前涉及提取用户数据的挑战类似。

### 步骤 2：构造 SQL 注入查询

2. **利用搜索功能**：
   - 使用查询 `test')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--` 发起 SQL 注入。

   <img src="../assets/difficulty4/christmas_special_1.png" alt="sql injection" width="500px">

   - 这是基于这样的推测：圣诞优惠等隐藏或特别商品可能在系统中被标记为已删除。

### 步骤 3：获取商品 ID

3. **提取隐藏商品详情**：
   - SQL 注入成功返回了常规商品列表中不可见的商品详情，其中包括圣诞优惠。
   - 查明圣诞优惠的 `productId` 为 `10`。

### 步骤 4：将商品加入购物车

4. **篡改购物车请求**：
   - 将商品加入购物车，并使用 Burp 捕获请求：

   <img src="../assets/difficulty4/christmas_special_2.png" alt="dummy product adding" width="500px">

   - 随后在 POST 请求中将 `ProductId` 设置为 `10`，把特别商品加入购物车：

   <img src="../assets/difficulty4/christmas_special_3.png" alt="basket" width="500px">

## 解法说明

我们利用商品搜索 API 中的 SQL 注入发现隐藏商品。注入查询了 `deletedAt` 字段非空的商品，揭示了圣诞优惠商品。随后将获取的 `productId` 用于把该商品加入购物车，绕过了常规浏览和选择流程。

### 安全措施

为降低此类风险：
- **正确验证和清理输入**：对所有输入进行适当验证和清理，以防 SQL 注入。
- **使用参数化查询**：将 SQL 逻辑与输入数据分离，从而防止 SQL 注入。
- **可见性和访问控制**：避免使用逻辑删除（将商品标记为已删除），并确保不会因此留下可访问的数据（应彻底删除）。
