# Juice-Shop Write-up：Bonus Payload

## 挑战概述

**Title:** Bonus Payload\
**Category:** Cross-Site Scripting (XSS)\
**Difficulty:** ⭐ (1/6)

继前一个 DOM XSS 挑战之后，本挑战要求注入更复杂的 XSS payload，其中包含嵌入 SoundCloud 播放器的 iframe。

## 使用的工具

- **Web Browser**：用于执行攻击并观察其效果。

## 方法与解决方案

### Payload 解析

给定的 payload 是一个 iframe，用于将 SoundCloud 音乐播放器动态嵌入应用程序的 DOM。该 payload 本身并无恶意，但它展示了通过 XSS 嵌入任意第三方内容的可能性，而这可能导致钓鱼攻击、不需要的广告或恶意脚本。

### 挑战解决步骤

1. **构造并编码 Payload**：
   - 对提供的 iframe 代码进行 URL 编码，以确保其作为查询参数传递时能被 Web Browser 正确处理。
   - 注入 payload 后的完整 URL 如下：`http://127.0.0.1:3000/#/search?q=%3Ciframe%20width%3D%22100%25%22%20height%3D%22166%22%20scrolling%3D%22no%22%20frameborder%3D%22no%22%20allow%3D%22autoplay%22%20src%3D%22https:%2F%2Fw.soundcloud.com%2Fplayer%2F%3Furl%3Dhttps%253A%2F%2Fapi.soundcloud.com%2Ftracks%2F771984076%26color%3D%2523ff5500%26auto_play%3Dtrue%26hide_related%3Dfalse%26show_comments%3Dtrue%26show_user%3Dtrue%26show_reposts%3Dfalse%26show_teaser%3Dtrue%22%3E%3C%2Fiframe%3E`

2. **执行攻击**：
   - 访问修改后的 URL 后，iframe 在 Web 应用中成功渲染，并按预期嵌入 SoundCloud 播放器。

### 解决方案说明

本挑战通过证明可以利用 XSS 漏洞向 Web 应用注入复杂的 iframe 元素而完成。此处的 iframe 嵌入了 SoundCloud 播放器，说明攻击者可以利用 XSS 向易受攻击的 Web 应用插入几乎任意内容。

## 修复建议

为防范此类 XSS 攻击，尤其是包含复杂 payload 的攻击，请参考以下建议：

- **Content Security Policy (CSP)**：实施 CSP，限制可加载脚本和 iframe 的来源。该策略有助于阻止未授权脚本执行，以及不需要的外部内容嵌入。
- **输入净化**：确保在将所有用户输入加入 DOM 前都进行净化。这包括对 HTML 元素进行编码或移除。
