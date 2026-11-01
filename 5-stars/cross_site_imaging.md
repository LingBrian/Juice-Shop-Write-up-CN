# Juice-Shop Write-up: 跨站图像

## 挑战概述

**标题：** Cross-Site Imaging  
**类别：** 跨站请求伪造（CSRF）  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战的目标是操纵 Juice Shop 网站豪华会员套餐展示所用图片的源 URL，以注入外部图像。

## 使用工具

- **Web 浏览器：** 用于访问 Juice Shop 应用并修改 URL。
- **开发者工具：** 用于检查和操纵 Juice Shop 中运行的 HTML 与 JavaScript 代码。

## 方法与解题过程

### 步骤 1：分析页面

访问 `http://127.0.0.1:3000/#/deluxe-membership` 豪华会员页面，观察到页面展示了几张带有各种贴花装饰的豪华盒子图片。

<img src="../assets/difficulty5/cross_site_imaging_1.png" alt="Deluxe Membership Boxes" width="500px">

### 步骤 2：识别图片源操纵方式

检查页面源代码，了解图片的加载和显示方式。发现图片根据 `logoSrc` 属性动态加载，而该属性可通过 URL 参数操纵。

<img src="../assets/difficulty5/cross_site_imaging_5.png" alt="reference of all images" width="500px">

也可以在 main.js 中找到默认来源：

<img src="../assets/difficulty5/cross_site_imaging_6.png" alt="sources of main image" width="500px">

<img src="../assets/difficulty5/cross_site_imaging_7.png" alt="sources of stickers images" width="500px">

### 步骤 3：代码审查

分析了负责此功能的 JavaScript：
- 构造函数会将 `logoSrc` 初始设置为默认图片路径。
- 根据我对 JS 代码的理解，如果提供了 `testDecal` URL 参数，它就会覆盖默认图片路径；否则使用默认路径。

### 步骤 4：利用重定向

尝试通过 `testDecal` 参数指定外部图片：

构造的载荷使用相对路径跳转序列，并结合重定向指向外部图像：
```plaintext
http://127.0.0.1:3000/#/deluxe-membership?testDecal=../../../../redirect?to=https://dummyimage.com/600x400/000/fff&x=https://github.com/bkimminich/juice-shop
```

1. 此载荷使用目录遍历（`../../../../`）到达服务器根目录，然后利用内部重定向功能指向外部占位图片。
2. 此载荷使用 `&x=https://github.com/bkimminich/juice-shop` 绕过允许列表。如果不理解这一步，请参阅允许列表绕过挑战。

### 步骤 5：最终利用

提交包含构造好的 `testDecal` 参数的 URL 后，页面成功显示外部图片，证实图片源路径处理存在漏洞。

<img src="../assets/difficulty5/cross_site_imaging_8.png" alt="test of changing sources of stickers images" width="500px">

## 解题说明

本挑战利用了图片源属性的相对路径处理方式，并结合内部重定向到允许的 URL，再将外部图片 URL 作为参数传入。

## 修复建议

- **验证并清理输入：** 严格验证和清理所有影响文件路径或 URL 的参数，以防止目录遍历及其他注入攻击。
- **限制 URL 重定向：** 对重定向目标实施严格的允许列表，仅允许预期的目标。
- **使用绝对路径：** 修改应用以使用内部资源的绝对路径，避免相对路径被滥用。
