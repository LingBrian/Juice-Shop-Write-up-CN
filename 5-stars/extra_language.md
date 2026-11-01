# Juice-Shop Write-up: 额外语言

## 挑战概述

**标题：** Extra Languages  
**类别：** 本地化  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求利用语言选择功能，在 Juice Shop 应用中访问隐藏或不受支持的语言设置。

## 使用工具

- **Web 浏览器：** 用于操作语言选择功能并发送 HTTP 请求。
- **Burp Suite：** 用于拦截网络请求并分析响应。

## 方法与解题过程

### 步骤 1：分析语言切换请求

- 通过用户界面切换语言时，捕获网络请求，发现应用根据两个组成部分获取语言文件：

1. **语言代码（ISO 639-1）：** 表示语言的两字母代码（例如，`fr` 表示法语，`en` 表示英语）。
2. **国家代码（ISO 3166-1 alpha-2）：** 表示国家的两字母代码（例如，`FR` 表示法国，`US` 表示美国）。

- 观察到的 URL 格式为：
```
/assets/i18n/{language_code}_{COUNTRY_CODE}.json
```

- 这是切换语言时的网络请求：

<img src="../assets/difficulty5/extra_language_1.png" alt="request when changing languages" width="500px">

### 步骤 2：探索可用语言

- 根据提示，我在项目官方网站上查找与语言开发相关的部分，并在 companion guide 中找到了该内容：

<img src="../assets/difficulty5/extra_language_2.png" alt="screenshot from companion guide that give us the link of Crowdin project" width="500px">

- 可以看到，任何人都可以通过 Crowdin 项目参与贡献。

### 步骤 3：提出假设并调查

考虑到可能存在彩蛋或隐藏语言，我开始调查开发者为测试或娱乐而加入的特殊或虚构语言。为避免逐一检查所有语言，我在 Crowdin 页面上筛选了开发进度最靠前的语言。

### 步骤 4：从文档中发现线索

我发现了《星际迷航》中的虚构语言“克林贡语”，其代码为 `tlh_AA`。

<img src="../assets/difficulty5/extra_language_3.png" alt="fictional language" width="500px">

### 步骤 5：成功注入语言代码

使用发现的语言代码：
```
tlh_AA
```
应用成功加载了一个新的语言文件；该语言未在用户界面中列出，也未获官方支持。这证实我们利用语言处理功能访问了隐藏设置。

## 解题说明

解题关键在于理解应用如何构造语言文件路径，再利用文档和其他资源推测可能存在的隐藏功能。正确猜出并注入虚构语言代码后，便成功启用了隐藏语言。
