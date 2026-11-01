# Juice Shop Write-up：Missing Encoding Challenge

## 挑战概述

**Title:** Missing Encoding\
**Category:** Improper Input Validation\
**Difficulty:** ⭐ (1/6)

本挑战要求获取 Web 应用中一张因 URL 编码问题而无法加载的特定图片。解决过程需要了解 Web browser 如何处理 URL，以及正确进行 URL 编码的重要性。

## 使用的工具

- 带开发者工具的 Web browser
- 在线 URL 编码工具

## 方法与解决方案

为解决“Missing Encoding”挑战，我采取了以下步骤：

1. **发现问题**：注意到应用照片墙上有一张图片无法加载。使用浏览器开发者工具检查该元素后发现，图片标签的 `src` 属性中含有未进行 URL 编码的特殊字符（emoji）。

<img src="../assets/difficulty1/missing_encoding_1.png" alt="code image" width="500px">

2. **理解问题**：图片源 URL 中的特殊字符没有正确进行 URL 编码，导致浏览器无法获取图片。
3. **使用 URL 编码工具**：使用在线 URL 编码工具，对含特殊字符的 URL 进行正确编码。需要编码的字符包括 emoji 等；如果未正确编码，浏览器通常无法正确处理它们。得到的 URL 是：assets%2Fpublic%2Fimages%2Fuploads%2F%F0%9F%98%BC-%23zatschi-%23whoneedsfourlegs-1572600969477.jpg
4. **替换 URL**：复制编码后的 URL，并使用浏览器开发者工具将图片标签 `src` 属性中的原 URL 替换掉。
5. **重新加载并验证**：刷新页面或重新加载图片，确认图片现在可以正常显示。

<img src="../assets/difficulty1/missing_encoding_2.png" alt="image loaded" width="500px">

## 解决方案说明

图片的原始 URL 包含 emoji 和其他特殊字符，但这些字符应当进行 URL 编码。Web browser 需要正确编码的 URL 才能解析它们。解决方法是对 URL 进行编码，使浏览器能够正确解析并在服务器上定位图片。

## 修复建议

主要的修复措施是确保所有 URL，尤其是可能包含非标准字符的 URL，在嵌入网页之前都进行正确的 URL 编码。通常可以通过以下方式实现：

- **后端方案**：确保服务器端代码在向客户端发送 URL 前先对其编码。
- **客户端检查**：在 HTML 文档中使用 URL 前，实施客户端检查或函数，按需对 URL 进行编码。
