# Juice-Shop 攻略：铸造蜂蜜罐 NFT

## 挑战概述

**标题：** 铸造蜂蜜罐  
**类别：** 输入验证不当  
**难度：** ⭐⭐⭐ (3/6)

本挑战要求在 OWASP Juice Shop 的“Bee Haven”中积累一种名为 BEEs 的数字货币，以铸造蜂蜜罐 NFT。

## 使用的工具

- **MetaMask**：用于管理 Ethereum 钱包的浏览器扩展，是处理区块链交易所必需的工具。
- **Web 浏览器**：访问并使用 OWASP Juice Shop 应用。

## 方法与解决方案

### 步骤 1：访问“Bee Haven”

1. **前往 Bee Haven**：
   - 在 main.js 中搜索“bee”，找到页面路径：

    <img src="../assets/difficulty3/mint_the_honey_pot_1.png" alt="path" width="500px">

   - 按照 main.js 路由配置中发现的路径 `/bee-haven`，访问 OWASP Juice Shop 中的 Bee Haven。

    <img src="../assets/difficulty3/mint_the_honey_pot_2.png" alt="bee haven page" width="500px">

### 步骤 2：收集 BEES

2. **领取 BEES**：
   - 首先尝试点击 Bee Haven 页面上的领取按钮，领取 1000 BEES。
   - 应用提示需要从测试网水龙头存入 ETH 才能领取 BEES，表明此操作需要与区块链交互。

    <img src="../assets/difficulty3/mint_the_honey_pot_4.png" alt="error bee" width="500px">

### 步骤 3：解决水龙头问题

3. **获取测试 ETH**：
   - 由于传统水龙头要求钱包此前有过活动记录，我很难找到可获取水龙头 ETH 的网站。
   - 最后，我找到一个无需前置条件即可获取测试 ETH 的网站。

    <img src="../assets/difficulty3/mint_the_honey_pot_5.png" alt="faucet" width="500px">

   - 使用获得的测试 ETH 执行交易，即可启用 BEES 领取功能。

    <img src="../assets/difficulty3/mint_the_honey_pot_6.png" alt="obtained bee" width="500px">

### 步骤 4：铸造蜂蜜罐 NFT

4. **铸造 NFT**：
   - 通过 Bee Haven 功能积累 1000 BEES 并与区块链交互后，点击 Mint 按钮创建蜂蜜罐 NFT。

    <img src="../assets/difficulty3/mint_the_honey_pot_7.png" alt="mint the pot" width="500px">
   
   - 确认 NFT 已成功铸造并添加到你的区块链资产中。
