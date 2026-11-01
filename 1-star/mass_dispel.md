# Juice Shop Write-up：Mass Dispel Challenge

## 挑战概述

**Title:** Mass Dispel\
**Category:** Miscellaneous\
**Difficulty:** ⭐ (1/6)

“Mass Dispel”挑战要求一次操作关闭多条“Challenge solved”通知。本挑战依据提供的文档，考察高效浏览和使用应用功能的能力。

## 使用的工具

- Web browser
- OWASP Juice Shop Companion Guide（文档）

## 方法与解决方案

解决本挑战的步骤如下：

1. **查阅 Companion Guide**：访问官方 OWASP Juice Shop Companion Guide：https://pwning.owasp-juice.shop/companion-guide/latest/part1/challenges.html。该指南详细介绍了应用程序及其各项功能的使用方法。
2. **查看文档**：找到指南中有关处理多条通知的部分。文档说明，按住 `Shift` 键并点击某条通知的关闭按钮，即可同时关闭多条通知。
3. **应用该技巧**：打开 OWASP Juice Shop Web 界面，完成多个挑战以生成多条“Challenge solved”通知，然后按照指南所述，使用 `Shift` 加点击的方式一次性关闭所有通知。

## 解决方案说明

该方案利用了 OWASP Juice Shop 应用程序中一项有文档说明的功能，帮助用户更高效地管理界面。关闭通知时按住 `Shift` 键，应用程序便会同时关闭所有可见通知。

## 修复建议

无需修复，因为本挑战旨在让用户了解 Juice Shop 应用程序的特定功能，而非揭示安全漏洞。不过，无论是在 CTF 场景还是在实际应用中，有效利用文档都是一项重要技能。
