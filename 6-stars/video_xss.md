# Juice-Shop Write-up: 视频 XSS

## 挑战概述
**标题：** Video XSS  
**类别：** 跨站脚本（XSS）  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)  

本挑战的目标是将 XSS 载荷（`</script><script>alert('xss')</script>`）嵌入 Juice Shop 应用的宣传视频中。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用。
- **开发者工具：** 用于检查和操纵 HTML 与 JavaScript 代码。
- **Burp Suite：** 用于拦截和分析 HTTP 请求。
- **命令行工具：** 用于创建具有特定目录结构的 ZIP 文件，以利用该漏洞。
- **Twitter 高级搜索：** 用于精准搜索推文（https://twitter.com/search-advanced）

## 方法与解题过程

### 步骤 1：查找宣传视频
根据提示可知，宣传视频 URL 不在站点地图中，但其路径曾在某处公布。我们通过定向搜索找到它：
- **Twitter 搜索：** 筛选 @bkimminich 和 @owasp_juiceshop 的推文，并搜索关键词“promo、promotion、juice-shop、video”，找到该 URL：
  - `https://twitter.com/bkimminich/status/1114621693299916800`

<img src="../assets/difficulty6/video_xss_1.png" alt="tweet 1" width="600px">

  - `https://twitter.com/owasp_juiceshop/status/1350381894010077184`

<img src="../assets/difficulty6/video_xss_2.png" alt="tweet 2" width="600px">

- **直接访问 URL：** 使用 GoBuster 也可以找到 `http://localhost:3000/promotion`。

### 步骤 2：识别字幕文件
使用开发者工具检查宣传视频页面，并按关键词“subtitle”筛选后，我们发现字幕以 WebVTT 文件格式加载：

<img src="../assets/difficulty6/video_xss_3.png" alt="observation" width="600px">

我们还在开发者工具中找到了视频来源：/video 页面，地址为 http://127.0.0.1:3000/video。

- **视频来源：** 访问 127.0.0.1:3000/video 时，服务器响应头（Content-location）指向视频文件 `http://localhost:3000/assets/public/videos/owasp_promo.mp4`。
- **字幕来源：** 我们正在寻找字幕文件，知道 WebVTT 扩展名为 .vtt，因此将 `.mp4` 替换为 `.vtt`，发现字幕文件位于 `http://localhost:3000/assets/public/videos/owasp_promo.vtt`。

<img src="../assets/difficulty6/video_xss_4.png" alt="subtitle file" width="600px">

### 步骤 3：检查服务器响应
使用 Burp Suite 分析访问宣传视频页面时的服务器响应：
- **检查标头：** 注意到 `Content-Location` 标头指向 `/assets/public/videos/owasp_promo.mp4`，这表明路径正确。

### 步骤 4：尝试修改字幕文件
要注入 XSS 载荷，需要修改 `.vtt` 文件。可以利用 Zip Slip 漏洞实现，类似于“覆盖 Legal 文件”挑战。

### 步骤 5：确定正确文件路径
在使用 Zip Slip 之前，我们必须找出字幕文件的正确路径。从 /complaint 页面可知需要返回两级（../），但之后的路径尚不明确。通过反复尝试并深入检查代码，最终找到正确路径：
- **初次尝试：** 尝试 `../../assets/public/videos/owasp_promo.vtt` 等路径，但未成功。
- **找到最终路径：** 通过深入检查和引导式暴力搜索（3 层），可以确定正确路径为 `frontend/dist/frontend/assets/public/videos/owasp_promo.vtt`，这意味着 Angular Juice Shop 项目的根路径为 `frontend/dist/frontend`。该路径很难找到，我查阅官方解法后才确认。官方解释是 Angular 项目常使用关键词 `frontend` 和 `dist`，因此可以编写 Python 脚本测试所有组合。

### 步骤 6：构造利用文件
1. **准备目录结构：** 创建临时目录 `temp`，并在其中构造 `/frontend/dist/frontend/assets/public/videos/owasp_promo.vtt`。还要创建 `/child/child` 目录结构。

完成后，目录结构应如下所示：

<img src="../assets/difficulty6/video_xss_5.png" alt="structure" width="600px">

2. **添加载荷：** 将载荷 `</script><script>alert('xss')</script>` 写入 `owasp_promo.vtt` 文件。
3. **压缩利用文件：** 创建 `exploit.zip`，并调整路径以覆盖目标文件：
   - 在 `child/child` 目录中运行命令：`zip exploit.zip ../../frontend/dist/frontend/assets/public/videos/owasp_promo.vtt`。

### 步骤 7：执行利用
1. **上传利用文件：** 通过 Juice Shop 应用的投诉表单上传 `exploit.zip`。

2. **验证：** 访问 `http://localhost:3000/assets/public/videos/owasp_promo.vtt`，确认恶意载荷已插入：

<img src="../assets/difficulty6/video_xss_6.png" alt="modified subtitle file" width="600px">

2. **最终验证：** 访问 `http://localhost:3000/promotion`，查看 XSS 载荷执行：

<img src="../assets/difficulty6/video_xss_7.png" alt="xss executed in the video" width="600px">

## 解题说明
本挑战利用了文件上传过程中的路径遍历漏洞。通过构造能导航到正确目录并覆盖字幕文件的 ZIP 文件，我们成功注入 XSS 载荷，展示了该漏洞。

## 修复建议

- **验证并清理输入：** 验证并清理所有文件路径和输入参数，防止路径遍历及其他注入攻击。
- **改进文件上传处理：** 在文件上传过程中实施稳健的安全检查，避免任意文件写入。
- **使用绝对路径：** 修改应用以使用内部资源的绝对路径，避免相对路径被滥用。
