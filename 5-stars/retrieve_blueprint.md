# Juice-Shop Write-Up: 获取蓝图

## 挑战概述

**标题：** Retrieve Blueprint\
**类别：** 取证/侦察\
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求找到并获取与 OWASP Juice Shop 项目相关的 3D 蓝图。

## 使用工具

- **Aperisolve**：用于分析图像并提取隐藏元数据、字符串或其他嵌入信息。
- **Web 浏览器**：用于访问 URL 并检查网页内容。
- **wget**：用于从 Web 下载文件的命令行工具。
- **AperiSolv**：隐写分析工具（https://www.aperisolve.com/）

## 方法与解题过程

### 初步侦察

1. **图像分析：**
   - 根据提示，我们知道蓝图不在 FTP 服务器上，而是与图像中展示的 3D 打印物品有关。
   - 我们还知道可以从产品主页上的 OWASP Juice Shop Logo 3D 打印商品入手，因此先下载该商品图片。
   - 使用 Aperisolve 分析位于 `assets/public/images/products/3d_keychain.jpg` 的 `3d_keychain.jpg` 图像，提取隐藏元数据和字符串。

### 提取信息

2. **跟进线索：**
   - 检查图像中发现的字符串，尤其是 URL 和提及的软件 OpenSCAD。

   <img src="../assets/difficulty5/retrieve_blueprint_2.png" alt="Aperisolv" width="500px">

   - 检查发现的 URL（Imgur 和 Adobe 链接）是否有效及其内容，但最终没有进展。

### 误入 GitHub 仓库

3. **探索相关资源：**
   - 偶然通过 Google Dork 找到一个 GitHub 仓库，其中包含与商品图片相符的蓝图文件（`JuiceShop_KeyChain.stl`），但并不能直接解决挑战。如果需要真实的 3D 模型，可访问此链接：https://github.com/OWASP/owasp-swag/blob/master/projects/juice-shop/3d/JuiceShop_KeyChain.stl

### 提出假设并暴力猜测

4. **猜测文件路径：**
   - 根据图像存储路径和文件格式（.stl），推测可能存在其他相关商品对应的 STL 文件 URL。
   - 使用常见命名习惯并逐一尝试，猜测正确的文件名和路径。
   - 更具体地说，我尝试了：`JuiceShop.stl`

### 成功获取

5. **下载蓝图：**
   - 使用 `wget` 访问猜测的 URL，成功获取蓝图：`http://localhost:3000/assets/public/images/products/JuiceShop.stl`。

## 解题说明

本挑战结合图像分析、对现有元数据进行逻辑推断，以及系统地猜测文件路径来解决。获取未直接列出、但可以根据仓库中其他文件合理推测命名的 `JuiceShop.stl` 文件是关键。

## 修复建议

为防止蓝图等敏感文件被未授权访问：

- **安全存储文件：** 限制对包含敏感文件的目录的公开访问，并使用随机或难以预测的文件名。
- **访问控制：** 实施服务端检查，确保只有经过授权的用户才能访问特定文件或目录。
