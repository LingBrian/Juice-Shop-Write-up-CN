# Juice-Shop Write-up: 本地文件读取（LFI）

## 挑战概述
**标题：** Local File Read (LFI)  
**类别：** 服务端注入  
**难度：** ⭐⭐⭐⭐⭐ (5/6)  

本挑战的目标是利用 Juice Shop 应用中的本地文件读取（LFI）漏洞，读取服务器上的敏感文件。

## 使用工具
- **Web 浏览器：** 用于浏览 Juice Shop 应用并访问开发者工具。
- **Burp Suite：** 用于拦截和修改 HTTP 请求，以测试注入点。
- **Fuzzer：** 用于自动测试潜在参数和值。

## 方法与解题过程

### 步骤 1：定位易受攻击的端点
在之前的挑战中，我们发现端点 `http://localhost:3000/dataerasure` 可能存在漏洞。该端点负责处理 GDPR 数据擦除请求。

<img src="../assets/difficulty5/local_file_read_1.png" alt="basic request" width="600px">

### 步骤 2：发送测试请求
1. **访问数据擦除页面：** 前往 `http://localhost:3000/dataerasure` 并提交一个随机的 GDPR 擦除请求。
2. **拦截请求：** 使用 Burp Suite 捕获 HTTP POST 请求。

### 步骤 3：分析请求
拦截到的请求如下：
```http
POST /dataerasure HTTP/1.1
Host: localhost:3000
Content-Length: 43
Content-Type: application/x-www-form-urlencoded
...
Cookie: [Your authentication cookies]
...
email=admin%40juice-sh.op&securityAnswer=test
```

### 步骤 4：模糊测试参数
使用 fuzzer 测试不同参数值以找到易受攻击的参数。目标是找出会导致服务器访问文件系统中文件的参数。

### 步骤 5：识别易受攻击的参数
模糊测试后发现存在 `layout` 参数。为 `layout` 提供无效值时，得到如下结果：
```http
500 Internal Server Error
Error: ENOENT: no such file or directory
```
该错误表明服务器尝试根据 `layout` 参数访问文件。

<img src="../assets/difficulty5/local_file_read_2.png" alt="response error example" width="600px">

### 步骤 6：构造利用请求
错误消息表明服务器会访问 `<root_directory>/juice-shop/views/<value_of_layout_parameter>` 路径中的文件。我们可以操纵 `layout` 参数，尝试读取服务器上已知的文件。

1. **目标文件：** 常见文件之一是 `package.json`。
2. **操纵参数：** 将 `layout` 参数修改为 `../package.json`：
```http
POST /dataerasure HTTP/1.1
Host: localhost:3000
Content-Length: 43
Content-Type: application/x-www-form-urlencoded
...
Cookie: [Your authentication cookies]
...
email=admin%40juice-sh.op&securityAnswer=test&layout=../package.json
```

### 步骤 7：发送利用请求
1. **发送请求：** 使用 Burp Suite 发送构造好的请求。

<img src="../assets/difficulty5/nosql_exfiltration_3.png" alt="final request" width="600px">

2. **验证响应：** 响应应包含 `package.json` 文件内容，证实 LFI 利用成功。

## 解题说明
通过发现 `dataerasure` 端点 `layout` 参数中的本地文件包含（LFI）漏洞，并将该参数修改为服务器上的已知文件路径，成功读取了文件内容，展示了该漏洞。

## 修复建议
- **输入验证与清理：** 验证和清理所有用户输入，防止路径遍历和文件包含攻击。
- **使用安全文件路径：** 避免直接将用户提供的数据用于文件路径。使用允许文件或目录的白名单。
- **错误处理：** 改进错误处理，避免泄露可帮助攻击者构造载荷的详细错误信息。
