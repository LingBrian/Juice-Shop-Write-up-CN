# Juice-Shop 攻略：检查隐私政策

## 挑战概述

**标题：** 检查隐私政策\
**类别：** 通过隐蔽实现安全\
**难度：** ⭐⭐⭐ (3/6)

“检查隐私政策”挑战要求通过发现政策文档中的隐藏元素或消息，证明自己熟悉该隐私政策。

## 使用的工具

- **Web 浏览器**：访问隐私政策页面并浏览隐藏链接。
- **分析能力**：仔细阅读并识别文本中的隐蔽信息。

## 方法与解决方案

### 定位隐私政策

1. **访问政策页面**：
   - 使用爬虫和网站地图工具进行梳理。
   - 爬虫（例如 BurpSuite）会发现该 URL：`http://127.0.0.1:3000/#/privacy-security/privacy-policy`
   - 通过该 URL 访问应用的隐私政策。

    <img src="../assets/difficulty3/privacy_policy_inspection_1.png" alt="privacy policy" width="500px">

### 检查文档

2. **详细检查政策内容**：
   - 仔细阅读隐私政策，留意任何不合常理或异常的措辞、格式或嵌入式超链接。

### 发现隐藏元素

3. **识别隐藏触发内容**：
   - 检查时注意到，将鼠标悬停在政策中的某些部分或词语上，会触发叠加弹窗或显示隐藏信息。

    <img src="../assets/difficulty3/privacy_policy_inspection_2.png" alt="example 1" width="500px">

    <img src="../assets/difficulty3/privacy_policy_inspection_3.png" alt="example 2" width="500px">

   - 将这些词语或短语组合起来，形成可能指向隐藏 URL 或文档的路径或消息。

    <img src="../assets/difficulty3/privacy_policy_inspection_4.png" alt="error page" width="500px">

### 访问隐藏内容

4. **根据线索继续操作**：
   - 前一个 URL 会返回错误信息，但错误信息中包含另一个路径。
   - 按照错误信息或文档叠加提示，访问 `/juice-shop/frontend/dist/frontend/assets/private/thank-you.jpg`。

### 验证挑战完成

5. **确认访问隐藏内容**：
   - 成功访问隐藏内容或页面，即可确认已彻底检查隐私政策并满足挑战条件。

### 解决方案说明

通过仔细阅读隐私政策并发现指向隐藏资源的嵌入线索，成功完成挑战。

## 修复建议

不适用于现实应用。
