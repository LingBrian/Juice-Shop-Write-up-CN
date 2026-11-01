# Juice-Shop 攻略：嵌套复活节彩蛋

## 挑战概述

**标题：** Nested Easter Egg  
**类别：** 密码学问题  
**难度：** ⭐⭐⭐⭐ (4/6)

本挑战要求解密另一个复活节彩蛋中隐藏的信息，通过高级密码分析发现应用中真正的复活节彩蛋。

## 使用的工具

- **Base64 解码器**：用于解码编码字符串。
- **ROT13 密码工具**：用于解密采用 ROT13 替换密码编码的消息。

## 方法与解法

### 步骤 1：解码 Base64

1. **初步解码**：
   - Easter Egg 挑战提供了 Base64 编码字符串 `L2d1ci9xcmlmL25lci9mYi9zaGFhbC9ndXJsL3V2cS9uYS9ybmZncmUvcnR0L2p2Z3V2YS9ndXIvcm5mZ3JlL3J0dA==`。
   - 使用 Base64 解码器解码后，得到另一段被隐藏的信息：`/gur/qrif/ner/fb/shaal/gurl/uvq/na/rnfgre/rtt/jvguva/gur/rnfgre/rtt`。

### 步骤 2：应用 ROT13 密码

2. **ROT13 解密**：
   - 解码后的信息看起来像乱码，说明可能使用了简单密码。
   - 尝试最常见的 ROT13 密码，将字母在字母表中移动 13 位。
   - 最终明文为：`/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg`。

### 步骤 3：访问嵌套复活节彩蛋

3. **找到嵌套彩蛋**：
   - 在浏览器中使用解密得到的路径访问 `127.0.0.1:3000/the/devs/are/so/funny/they/hid/an/easter/egg/within/the/easter/egg`。

   <img src="../assets/difficulty4/nested_easter_egg_1.png" alt="nested easter egg" width="500px">

   - 成功访问真正的嵌套复活节彩蛋，完成挑战目标。

## 解法说明

Nested Easter Egg 挑战要求结合技术与创造性进行解密。首先解码 Base64 字符串，得到看似无意义的文本；然后使用 ROT13 密码进一步解密这一简单但常见的文本转换。

本挑战展示了多层信息隐藏的方式：使用不止一种方法遮蔽信息，从而增加未经授权访问的难度。

## 修复建议

为防止数据被未经授权地解密：
- **多因子加密**：使用不同密钥或方法进行多层加密，以增强安全性。
- **加密最佳实践**：定期更新加密方法，并尽可能使用强度高且非标准的算法。
