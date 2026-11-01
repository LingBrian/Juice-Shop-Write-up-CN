# Juice-Shop Write-up: 供应链攻击

## 挑战概述

**标题：** Supply Chain Attack  
**类别：** 易受攻击的组件  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求识别项目依赖项中遭到入侵的软件包，该问题可能允许攻击者在开发或生产环境中执行任意代码。

## 使用工具

- **Web 浏览器：** 用于搜索 Juice Shop 所用软件包的相关漏洞。
- **GitHub：** 用于查找原始漏洞报告。

## 方法与解题过程

### 步骤 1：检查 `package.json`

检查之前在 Juice Shop 应用的某个挑战中获取的 `package.json` 文件，重点查看依赖项和开发依赖项。如果没有该文件，可从 127.0.0.1:3000/ftp 获取，也可以直接从此 GitHub 仓库的 files 文件夹中获取。

### 步骤 2：识别潜在易受攻击的软件包

彻底审查 `package.json` 中列出的每个软件包，特别检查开发工具，因为它们经常被忽略，但也可能存在导致供应链攻击的漏洞。

### 步骤 3：调查已知漏洞

使用各种安全数据库和漏洞跟踪器搜索每个软件包的已知漏洞。更具体地说，由于挑战标题提到了供应链漏洞，我便以此为方向进行搜索。

<img src="../assets/difficulty5/supply_chain_attack_2.png" alt="google search" width="500px">

### 步骤 4：发现遭入侵的软件包

发现项目中使用的 `eslint-scope@3.7.2` 存在重大漏洞。GitHub 上的安全事件报告详细说明了该问题，解释了 `eslint-scope` 的特定版本如何被入侵以窃取 npm token。

<img src="../assets/difficulty5/supply_chain_attack_3.png" alt="github vulnerability" width="500px">

**漏洞报告：** [ESLint-scope Security Issue Report](https://github.com/eslint/eslint-scope/issues/39)

### 步骤 5：提交漏洞报告

向 Juice Shop 开发团队提交原始报告 URL 和受影响版本（`eslint-scope@3.7.2`）的详情，以供审查和修复。

## 解题说明

本挑战通过在 Juice Shop 的 `package.json` 中找到一个存在已知漏洞的软件包来解决，该漏洞可能导致供应链攻击。关联说明攻击途径及受影响软件包版本的 GitHub 问题后，便满足了挑战要求。

## 修复建议

- **更新遭入侵的软件包：** 立即更新或替换存在漏洞的 `eslint-scope`，改用已修补版本。
- **审计依赖项：** 定期审计生产依赖和开发依赖中的漏洞。使用 Snyk 或 dependabot 等工具自动执行此过程。
