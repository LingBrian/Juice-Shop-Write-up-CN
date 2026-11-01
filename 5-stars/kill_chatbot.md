# Juice-Shop Write-up: 关闭聊天机器人

## 挑战概述

**标题：** Kill Chatbot  
**类别：** 应用逻辑  
**难度：** ⭐⭐⭐⭐⭐ (5/6)

本挑战要求通过利用应用漏洞操纵聊天机器人的行为，从而禁用 Juice Shop 平台上的聊天机器人功能，而不是直接攻击服务器或服务。

## 使用工具

- **Web 浏览器：** 用于与聊天机器人交互并检查网页元素。
- **开发者工具：** 用于分析客户端代码和网络请求。

## 方法与解题过程

### 步骤 1：了解聊天机器人的实现

初步分析网页及其功能后，我发现名为“juicy-chat-bot”的聊天机器人似乎由外部来源加载的脚本驱动。观察和代码审查结果表明，机器人的运行可能受一些可操纵的条件或标志控制。

<img src="../assets/difficulty5/kill_chatbot_4.png" alt="hint to external" width="500px">

### 步骤 2：代码分析

管理聊天机器人的 JavaScript 代码中包含控制其在线状态和响应机制的函数。它似乎通过 token 管理会话和用户交互，并可能通过条件判断决定是否响应。

<img src="../assets/difficulty5/kill_chatbot_3.png" alt="js code" width="500px">

我们还发现了一些预设问题（与本挑战无关，但之后可能有用）：

<img src="../assets/difficulty5/kill_chatbot_6.png" alt="hint to external" width="500px">

### 步骤 3：利用脚本执行

关键发现是聊天机器人使用 `vm2` 的 `VM` 模块执行动态脚本。该模块虽然旨在提供 JavaScript 沙箱执行环境，但如果处理不当，可能容易受到某些注入攻击。

我们在这里找到一个可能的入口点：

```javascript
addUser (token, name) {
    this.factory.run(`users.addUser("${token}", "${name}")`)
  }
```

这段代码看起来很像“exec”函数。虽然仓库中没有显示执行代码，但我在网上了解到，它通常可能类似于：

```javascript
const { VM } = require('vm2');
this.factory.run = function(script) {
    let vm = new VM();
    vm.run(script);
};
```

### 步骤 4：代码注入

聊天机器人会要求用户输入姓名，然后由 `factory.run()` 方法处理，并执行包含用户输入姓名的脚本。如果用户输入未经清理就嵌入脚本，就可能存在注入机会。如前所述，我们可以推测此函数在 JS 中类似于“exec”调用，因此可以尝试相关利用方式。

### 成功执行载荷

通过操纵机器人姓名提示的输入，我构造了一个注入内容，干扰了处理聊天机器人的进程：

```plaintext
testname"); process=null; users.addUser("1234", "malicioususer")
```

将此载荷放入：
```plaintext
this.factory.run(‘users.addUser(“${token}”, “${name}”)’)
```

最终得到：
```plaintext
this.factory.run(‘users.addUser(“blabla”, “testname”); process=null; users.addUser(“1234”, "malicioususer”)’)
```

该载荷试图取消关键进程变量或改变沙箱中的执行流程，从而停止聊天机器人的运行或破坏其状态。

### 解题说明

通过利用聊天机器人脚本处理逻辑中的脚本执行漏洞，成功干扰了聊天机器人。注入破坏进程状态的代码片段后，聊天机器人停止响应，达成挑战要求。这类漏洞凸显了使用未经清理的输入动态构造脚本并执行的风险。

### 安全建议

- **清理输入：** 将用户输入放入动态执行脚本或数据库查询前，务必进行清理。
- **安全执行脚本：** 实现动态脚本执行等功能时，应遵循安全编码实践。必须正确配置 `vm2` 等库，防止逃逸沙箱。
