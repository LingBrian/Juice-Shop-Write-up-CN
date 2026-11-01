# Juice Shop Write-up：Score Board Challenge

## 挑战概述

**Title:** Score Board\
**Category:** Miscellaneous\
**Difficulty:** ⭐ (1/6)

本挑战要求找到应用中隐藏得很巧妙的“Score Board”页面。解决方法是分析应用程序的 JavaScript 代码，以发现该页面的路由。

## 使用的工具

- Web browser（例如 Chrome、Firefox）
- 用于查看 JavaScript 源代码的 Developer Tools

## 方法与解决方案

解决“Score Board”挑战时，我们检查了应用程序的 JavaScript 代码。步骤如下：

1. **打开 Developer Tools**：在与应用交互时打开 Web browser 的开发者工具。
2. **查看 JavaScript 文件**：切换到开发者工具中的“Sources”选项卡，检查应用加载的文件。
3. **分析 main.js**：重点查看 `main.js` 文件，因为其中通常包含路由定义和应用逻辑。
4. **搜索隐藏路由**：在 `main.js` 文件中搜索各种应用路由，发现该文件明确列出多个路径定义，其中包括隐藏的“Score Board”路径。

<img src="../assets/difficulty1/scoreboard_1.png" alt="scoreboard" width="500px">
5. **访问 Score Board**：使用在 `main.js` 文件中找到的路由，在浏览器地址栏输入路径，直接访问 Score Board 页面。

## 解决方案说明

`main.js` 是找到 Score Board 的关键，因为它包含应用程序使用的所有路由定义。查看该文件后，很容易发现 Score Board 的路由。本挑战考察 JavaScript 分析和 Web 应用路由的基础技能。

## 修复建议

本挑战以找到隐藏页面为目标，因此无需修复。不过，在真实场景中，敏感页面不应仅仅依靠隐藏客户端代码中的路由来保护，而应设置适当的访问控制。
