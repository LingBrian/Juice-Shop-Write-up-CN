# Juice-Shop 攻略：泄露的不安全商品

## 挑战概述

**标题：** Leaked Unsafe Product  
**类别：** 注入  
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求在 Juice Shop 商品数据库中，查找有关被标记为已删除的潜在危险商品的信息。

## 使用的工具

- **Web 浏览器**：用于浏览 Juice Shop 并与其 API 交互。
- **开发者工具**：用于分析和修改网络请求。

## 方法与解法

### 步骤 1：确定注入点

1. **商品搜索 API**：
   - 注入点位于 `127.0.0.1:3000/rest/products/search?q=payload`，该接口根据搜索查询参数返回商品。我们之前在用户凭据挑战中用过这一技巧。

### 步骤 2：构造 SQL 注入查询

2. **通过 SQL 注入发现隐藏商品**：
   - 使用查询 `test')) UNION SELECT * FROM PRODUCTS WHERE deletedAt IS NOT NULL--` 获取标准列表中隐藏的商品。推测这些商品可能因安全问题或其他原因而被移除。

   <img src="../assets/difficulty4/leaked_unsafe_product_4.png" alt="removed product" width="500px">

### 步骤 3：分析结果

3. **检查已删除商品**：
   - 根据独特的描述和可疑成分，识别出“Rippertuer Special Juice”。

   <img src="../assets/difficulty4/leaked_unsafe_product_1.png" alt="dangerous product" width="500px">

### 步骤 4：调查危险成分

4. **成分分析**：
   - 通过外部资料调查每种成分的潜在危险，但没有发现特别有用的信息。
   - 我在 Google 上搜索饮料名称，并找到一个介绍“Rippertuer Special Juice”成分的 Pastebin 链接。
   - 成分说明中包含以下内容：

   <img src="../assets/difficulty4/leaked_unsafe_product_2.png" alt="unsafe" width="500px">

### 步骤 5：报告危险成分

5. **找到问题成分**：
   - 发现果汁含有 Hueteroneel（Manchineel fruit），这种成分毒性极强，不适合食用，尤其是与 Eurogium Edule 同时存在时。因此，我通过投诉区进行了报告：

   <img src="../assets/difficulty4/leaked_unsafe_product_3.png" alt="complain" width="500px">

## 解法说明

解题过程结合了 SQL 注入和外部调查。通过精心构造的 SQL 查询获取隐藏商品数据，再通过在线资料核实成分安全性，确认该商品含有危险成分。最后通过 Juice Shop 的投诉系统报告，完成了挑战。

### 安全建议

为防止类似漏洞：
- **正确清理输入**：防止 SQL 注入。
- **确保全面的商品审核流程**：尤其是供人食用的商品，应进行严格检查。
- **安全、透明地披露成分**：维护消费者信任并遵守相关安全法规。

本挑战强调了后端安全措施对于防止未经授权的数据获取的重要性，也凸显了准确、安全地维护商品列表的必要性。
