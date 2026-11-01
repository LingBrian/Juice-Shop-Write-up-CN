# Juice-Shop Write-up: 任意文件写入

## 挑战概述
**标题：** Arbitrary File Write  
**类别：** 易受攻击的组件  
**难度：** ⭐⭐⭐⭐⭐ (6/6)  

本挑战的目标是利用任意文件写入漏洞，覆盖 Juice Shop 服务器上的 `legal.md` 文件。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **GoBuster：** 用于爆破目录和文件，以发现隐藏路径。
- **命令行工具：** 用于创建具有特定目录结构的 ZIP 文件，以利用该漏洞。

## 方法与解题过程

### 步骤 1：查找文件上传入口
挑战提示指向应用中的文件上传功能。我们找到了两个可能的页面：
1. **投诉页面：** 用于上传投诉文件。
2. **个人资料页面：** 用于上传个人头像。

### 步骤 2：识别漏洞
通过 Twitter 高级搜索，我们发现 Juice Shop 的 Twitter 账号发布了一条推文，指出投诉页面现已支持上传 ZIP 文件：
- **推文：** 提到了投诉页面便捷的 ZIP 文件上传功能。

<img src="../assets/difficulty6/arbitrary_file_write_1.png" alt="tweet" width="600px">

我们在 Google 上搜索“arbitrary file writing using zip”，发现了 Zip Slip 漏洞。该漏洞可利用 ZIP 解压过程中的路径遍历来任意写入文件。

### 步骤 3：确定目标文件路径
要覆盖的目标文件是 `legal.md`，位于 `http://127.0.0.1:3000/ftp/legal.md`。

### 步骤 4：创建恶意 ZIP 文件
利用 Zip Slip 漏洞需要创建一个 ZIP 文件，使其中的 `legal.md` 被解压到目标路径：
1. **确定目标路径：** 需要将 `legal.md` 放到相对于解压位置的 `../../ftp/legal.md` 路径。

2. **创建目录结构：**
   ```bash
   # 创建临时工作目录
   mkdir -p ~/Desktop/temp/child/child
   
   # 切换到临时目录
   cd ~/Desktop/temp
   
   # 创建用于模拟目标路径的文件夹结构
   mkdir -p folder/file
   cd ./folder/file
   touch legal.md
   echo "恶意内容" > legal.md

   # 返回根目录
   cd ../../
   ```

3. **添加恶意文件：**
   ```bash
   cd ./child/child
   ```

4. **创建 ZIP 压缩包：**
   ```bash
   zip exploit.zip ../../ftp/legal.md
   ```

该命令创建的 ZIP 文件解压后，会将 `legal.md` 放到相对于解压位置的 `../../ftp/legal.md`，从而覆盖目标文件。

### 步骤 5：上传 ZIP 文件
1. **访问投诉页面：** 打开 Juice Shop 应用的投诉页面。
2. **上传 ZIP 文件：** 上传前一步创建的 `exploit.zip` 文件。

### 步骤 6：验证利用结果
1. **验证文件覆盖：** 检查 `http://127.0.0.1:3000/ftp/legal.md` 的内容，确认它已被恶意内容覆盖。

## 解题说明
本挑战利用了文件上传过程中的 Zip Slip 漏洞。通过构造一个能够遍历到正确目录并覆盖目标文件的 ZIP 文件，成功展示了任意文件写入漏洞。

## 修复建议
- **输入验证：** 验证并清理所有文件路径，防止路径遍历和任意文件写入攻击。
- **安全处理文件：** 使用安全的文件上传和解压库，避免在缺少适当验证的目录中解压文件。
- **用户权限：** 限制文件写入权限，降低漏洞的潜在影响。
