# Juice-Shop Write-up：Meta Geo Stalking

## 挑战概述

**Title:** Meta Geo Stalking\
**Category:** Sensitive Data Exposure\
**Difficulty:** ⭐⭐ (2/6)

“Meta Geo Stalking”挑战考察利用网上发布图片的元数据回答安全问题，并重置用户密码的能力。

## 使用的工具

- **Web Browser**：用于访问图片下载链接和密码重置页面。
- **ExifTool**：用于提取图片的 GPS 坐标和其他元数据。
- **GPS Mapping Tool / Google Maps**：用于根据 GPS 坐标确定具体位置。

## 方法与解决方案

### 初步准备与发现

1. **识别安全问题**：
   - 尝试重置 John 的账户密码后，系统显示安全问题：“What is your favorite place to walk?”

   <img src="../assets/difficulty2/meta_geostaking_1.png" alt="reset question" width="500px">

   - 这个问题与照片墙上的图片说明直接对应，暗示答案与照片所示地点有关。

   <img src="../assets/difficulty2/meta_geostaking_2.png" alt="image from photo wall" width="500px">

2. **下载并分析图片**：
   - 从 `http://127.0.0.1:3000/assets/public/images/uploads/favorite-hiking-place.png` 下载图片。
   - 使用 ExifTool 提取图片元数据，其中包含 GPS 坐标。

### 确定具体地点

3. **使用 GPS 坐标**：
   - 将 GPS 坐标输入地图工具以获得精确位置。提取到的坐标为 `36°57'31.38"N 84°20'53.58"W`。

   <img src="../assets/difficulty2/meta_geostaking_3.png" alt="gps" width="500px">

   - Google Maps 确认该地点为 Daniel Boone National Forest。

   <img src="../assets/difficulty2/meta_geostaking_4.png" alt="localisation zoom 3" width="500px">

   <img src="../assets/difficulty2/meta_geostaking_5.png" alt="localisation zoom 2" width="500px">

   <img src="../assets/difficulty2/meta_geostaking_6.png" alt="localisation zoom 1" width="500px">

### 重置密码

4. **回答安全问题并重置密码**：
   - 确认地点是 Daniel Boone National Forest 后，将其作为安全问题的答案。
   - 根据从图片元数据中获得的位置信息，成功重置 John 的密码。

   <img src="../assets/difficulty2/meta_geostaking_7.png" alt="request to reset" width="500px">

### 解决方案说明

本挑战通过将安全问题与图片元数据中嵌入的位置数据关联起来而解决。这表明，如果带有地理位置标签的图片中包含与安全问题答案相关的信息，看似无害的图片分享也可能导致安全漏洞。

## 修复建议

为降低共享图片元数据和敏感数据暴露带来的风险：

- **清除元数据**：鼓励用户在将照片上传到公共平台前移除元数据，或在服务器端自动清除这些数据。
- **避免使用容易推断的安全问题**：使用答案无法通过公开信息或 OSINT 技术直接推断的安全问题。更好的做法是不使用安全问题，而改用电子邮件进行密码重置。
