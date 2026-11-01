# Juice-Shop Write-up：Visual Geo Stalking

## 挑战概述

**Title:** Visual Geo Stalking\
**Category:** Sensitive Data Exposure\
**Difficulty:** ⭐⭐ (2/6)

“Visual Geo Stalking”挑战通过公开的视觉信息推断安全问题答案，并利用该信息重置用户密码。本挑战考察图像分析、OSINT 技术，以及将看似无关的数据联系起来以访问敏感信息的创造性思维。

## 使用的工具

- **Google Lens**：用于识别图片中的地点和物体。
- **Google Maps**：用于探索定位地点的周边环境并收集更多信息。
- **Web Browser**：用于一般研究。

## 方法与解决方案

### 初步线索分析

1. **检查照片**：
   - Emma 在类似社交媒体的平台上发布了一张建筑物照片，说明中提到了她以前的工作地点。
   - 本挑战要求通过提供的图片识别 Emma 以前的工作地点。

   <img src="../assets/difficulty2/visual_geostaking_1.png" alt="post" width="500px">

### 详细图像分析

2. **使用 Google Lens**：
   - 使用 Google Lens 分析图片，识别出地点是“Kenau Park area in Haarlem”。

   <img src="../assets/difficulty2/visual_geostaking_2.png" alt="lens" width="500px">

3. **探索 Google Maps**：
   - 使用 Google Maps 虚拟访问该地点，检查各种商家标牌和牌匾，以便将可能的答案与安全问题对应起来。

   <img src="../assets/difficulty2/visual_geostaking_3.png" alt="signs" width="500px">

4. **识别关键信息**：
   - 仔细查看图片的另一部分后，在窗户上发现了“ITsec”标牌。这一细节似乎非常关键，但最初被忽略了。

### 回答安全问题并重置密码

5. **回答安全问题**：
   - 根据照片中的视觉线索和在线工具推断出的信息，尝试用“ITsec”回答有关 Emma 以前工作地点的安全问题。

   <img src="../assets/difficulty2/visual_geostaking_4.png" alt="itsec" width="500px">

6. **重置密码**：
   - 成功使用“ITsec”作为安全问题答案，重置了 Emma 账户的密码。

### 解决方案说明

本挑战结合了用户发布内容中的视觉信息、在线地图工具和图像识别技术，成功推断出答案。这种方法凸显了公开分享详细视觉信息可能带来的隐私风险。

## 修复建议

为防止通过视觉或其他公开信息泄露敏感数据：

- **教育用户**：告知用户分享详细照片或信息的风险，因为这些内容可能无意中暴露安全问题答案或其他敏感数据。
- **隐私设置**：鼓励用户在社交媒体及其他平台使用严格的隐私设置，控制个人信息和帖子对谁可见。
- **安全问题选择**：建议不要使用容易被发现的信息作为安全问题答案，鼓励选择更个人化且不易预测的答案。
