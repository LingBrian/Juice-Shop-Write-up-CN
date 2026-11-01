# Juice-Shop Write-up: 虚构挑战

## 挑战概述

**标题：** Imaginary Challenge  
**类别：** 密码学问题  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

“虚构挑战”是 OWASP Juice Shop 中的一个隐喻式挑战，旨在强调理解环境并运用创造性解决问题的能力。

## 重要说明

**_该挑战文档不够完善。如果你找到妥善完成挑战的方法，欢迎提交 pull request。_**

## 使用工具

- **Web 浏览器：** 用于与 Juice Shop 应用交互。
- **代码审查：** 分析 package.json 及相关脚本。

## 方法与解题过程

### 步骤 1：初步分析

由于提示暗示挑战可能并不存在，我开始调查 Juice Shop 的基础设施，重点研究挑战通常如何完成，尤其是用于验证挑战完成情况的“ContinueCodes”机制。

### 步骤 2：调查 `package.json`

审查 `package.json` 后，我发现了一个名为 `hashid` 的依赖，它用于对类似哈希的标识符进行编码和解码。这可能是生成或解码 `ContinueCodes` 的工具。

### 步骤 3：研究并参考社区解题文档

由于没有直接找到解决方法，我查阅了社区已有的解题文档。资料指出，该挑战最初依赖 `hashid` 提供的功能，但该功能现已不再以原来的形式存在。该项目曾有一个网站，几年前网站上展示了一个可用于测试 hashid 的 CodePen 链接。后来 hashid 被出售给另一家公司，网页重新设计，CodePen 也被移除。

### 步骤 4：理论上的解法

原本的解决方法应为：
- 使用 `hashid` 演示工具，以演示工具默认提供的特定盐值对数字 999 进行编码。
- 这将生成一个 `ContinueCode`。
- 通过 PUT 请求将该 `ContinueCode` 提交至：
  ```
  http://localhost:3000/rest/continue-code/apply/{ContinueCode}
  ```
  其中 `{ContinueCode}` 是 `hashid` 编码的输出。

## 解题说明

该挑战表明，在安全场景中使用软件默认值可能会带来风险。

## 修复建议

- **默认值：** 使用工具时，切勿直接使用其默认值。
