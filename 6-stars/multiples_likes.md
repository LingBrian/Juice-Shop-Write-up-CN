# Juice-Shop Write-up: 多次点赞

## 挑战概述

**标题：** Multiple Likes  
**类别：** 反自动化失效  
**难度：** ⭐⭐⭐⭐⭐⭐ (6/6)

本挑战要求利用应用处理商品评论点赞点击时的并发问题，使单个用户能够同时对同一条评论多次点赞。

## 使用工具

- **Burp Suite：** 用于拦截和操纵 HTTP 请求。
- **Python（`requests` 和 `threading` 模块）：** 用于编写脚本并发发送点赞请求，以绕过速率限制或其他反自动化措施。

## 方法与解题过程

### 步骤 1：捕获点赞请求

使用 Burp Suite 捕获用户在 Juice Shop 中为商品评论点赞时发送的 HTTP POST 请求。识别到的关键参数如下：
- **URL：** `http://127.0.0.1:3000/rest/products/reviews/like`
- **方法：** POST
- **数据：** `{ "id": "review_id" }`（其中 `review_id` 是被点赞评论的标识符）

<img src="../assets/difficulty6/multiple_likes_2.png" alt="like request" width="500px">

### 步骤 2：初步测试

最初，我尝试使用 Burp 的 Repeater 工具手动多次重发捕获到的请求，但只成功登记了两次点赞，未达到完成挑战所需的三次。这可能表明服务器端存在节流或去重逻辑。

<img src="../assets/difficulty6/multiple_likes_3.png" alt="only two like" width="500px">

虽然 Burp 的 Repeater 很快，但显然还不够快，因此我需要想办法提高速度。

### 步骤 3：编写并发请求脚本

为了克服手动重发时遇到的限制，我使用 Python 编写了脚本。我的思路是采用多线程，向点赞端点同时发送请求，从而提高处理速度。

<img src="../assets/difficulty6/multiple_likes_4.png" alt="python scripts" width="500px">

**获取脚本：** 使用的脚本位于此仓库的 tools 文件夹中：
- [文件夹链接](https://github.com/whyiest/juice-shop-write-up/tree/main/tools)
- [脚本链接](https://github.com/whyiest/juice-shop-write-up/tree/main/tools/multiple_likes.py)

记得在脚本中填写三个必需部分：Cookies、Email，以及最重要的 bearer token。可以在 Juice Shop 中执行操作并通过 Burp Suite 或其他工具捕获请求来获取这些信息。

<img src="../assets/difficulty6/multiple_likes_5.png" alt="python result" width="500px">

### 步骤 4：验证成功

运行脚本后，我在应用中确认同一条评论在同一时间收到了来自同一用户的多次点赞（本例中为 5 次）。

<img src="../assets/difficulty6/multiple_likes_6.png" alt="comment result" width="500px">

## 解题说明

同一用户会话对同一条评论成功提交多次点赞，说明 Juice Shop 的并发处理和速率限制存在缺陷。通过在服务器处理并应用限制前同时发送多个点赞请求，成功利用了该问题。

## 修复建议

- **实施适当的速率限制：** 在服务端按用户跟踪并限制操作频率，尤其是会改变数据状态的操作。
- **处理并发：** 改进服务器对并发请求的处理，确保点赞等操作不会被滥用。例如，可使用锁或信号量。
