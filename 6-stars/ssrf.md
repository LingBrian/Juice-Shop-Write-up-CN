# Juice-Shop Write-up: SSRF（服务端请求伪造）

## 挑战概述

**标题：** SSRF  
**类别：** 访问控制失效  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战要求利用服务端请求伪造（SSRF）漏洞，通过服务器自身请求服务器上的隐藏资源。

## 使用工具

- **Web 浏览器**：用于与 Juice Shop 应用交互并修改个人头像 URL。
- **Burp Suite**：用于拦截和修改发送到服务器的 HTTP 请求。

## 方法与解题过程

### 步骤 1：寻找潜在 SSRF 点

了解到挑战涉及 SSRF 后，我开始寻找 Juice Shop 应用中由服务器获取外部 URL 的功能。用户个人资料区域允许通过 URL 设置头像，这似乎是一个可能的入口。

<img src="../assets/difficulty6/ssrf_3.png" alt="profile image" width="500px">

### 步骤 2：测试假设

使用 Burp Suite 拦截修改个人头像时发送的请求，观察到该请求接受外部图片 URL。这表明服务器会从用户提供的 URL 获取图片，是典型的 SSRF 易受攻击场景。

<img src="../assets/difficulty6/ssrf_4.png" alt="url" width="500px">

实际上，我通过 Burp 发现 URL 被直接注入请求的 imageURL 参数中，这意味着服务器端会获取该 URL。

### 步骤 3：探索隔离目录

以下是挑战提示：

<img src="../assets/difficulty6/ssrf_1.png" alt="hints" width="500px">

根据挑战说明中的提示，我检查了服务器 `/ftp/quarantine/` 目录下恶意软件文件中包含的 URL。

<img src="../assets/difficulty6/ssrf_2.png" alt="ftp quarantine" width="500px">

其中每个 URL 都可能触发 SSRF。

### 步骤 4：操纵图片 URL

最初，我尝试直接将恶意软件文件中的 URL 用作个人资料图片源，以触发 SSRF，但都没有成功。

### 步骤 5：找到触发 URL

我还没有打开过恶意软件文件，因此决定反编译它们，了解其工作方式。

在恶意软件文件中，我注意到一个特定 URL：
```
http://localhost:3000/solve/challenges/server-side?key=tRy_H4rd3r_n0thIng_iS_Imp0ssibl3
```

该 URL 看起来是专门为本挑战设计的，很可能是 SSRF 的触发点。事实上，单独访问该 URL 时没有任何事情发生。

### 步骤 6：利用 SSRF

将个人头像 URL 改为在恶意软件文件中发现的 URL 并应用更改后，服务器向该内部 URL 发出了请求。该内部请求成功解开了挑战。

## 解题说明

服务器配置不当，未适当验证或限制用户提供的 URL。通过将个人头像 URL 改为内部链接，我使服务器请求了隐藏资源，展示了 SSRF 漏洞。

## 修复建议

- **验证并清理输入**：正确验证和清理用户提供的 URL，防止服务器发起非预期请求。
- **限制 URL 获取范围**：仅允许获取已知安全的域或路径。
- **在客户端预先获取**：如有可能，在客户端预先获取图片，只向服务器发送获取后的图片。
