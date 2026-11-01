# OWASP Juice Shop 解题指南（中文版）

> 中文翻译版。原项目：[Whyiest/Juice-Shop-Write-up](https://github.com/Whyiest/Juice-Shop-Write-up)。本仓库将原项目 README、挑战解题文档及相关说明翻译为简体中文；项目代码、命令、链接和文件结构尽量保持原样。此为非官方翻译，与 OWASP Juice Shop 官方项目无关。

<img src="./assets/public/logo.png" width="400">

欢迎访问本 GitHub 仓库。本仓库专门收录 OWASP Juice Shop CTF 挑战的详细解题指南。OWASP Juice Shop 是一款刻意设计为存在安全漏洞的 Web 应用，用于安全培训、演示和测试安全工具与技术。本仓库旨在为每项挑战提供分步解决方案、所利用漏洞的详细说明以及建议的修复措施。

## 目录
- [仓库结构](#仓库结构)
- [挑战等级](#挑战等级)
- [建议](#建议)
- [贡献](#贡献)
- [挑战列表](#挑战列表)

## 仓库结构

- **Challenges（挑战）**：按难度等级将解法整理到不同文件夹中，等级从 1 星到 6 星（⭐-⭐⭐⭐⭐⭐⭐）。每个挑战文件夹都包含详细的解题说明，介绍漏洞利用思路以及如何保护应用。
- **Tools（工具）**：包含为解决 Juice Shop 挑战而开发和使用的脚本及自动化工具。
- **Assets（资源）**：存放各个挑战说明中使用的图片，用于展示文中讨论的步骤、结果和重要概念。
- **Files（文件）**：包含挑战期间从 Juice Shop 网站下载的文件。它们是在解决各项挑战时获取的，可供参考，以节省 CTF 解题时间。
- **Achievements Backup（成就备份）**：`Tools` 文件夹中的 `all_achievements.json` 是 Juice Shop CTF 进度状态的备份。导入后可自动验证大多数挑战。该文件仅供参考和学习。

## 挑战等级

进入对应文件夹，查看该难度等级的挑战与解法：

- [⭐ 1 星挑战](https://github.com/whyiest/juice-shop-write-up/tree/main/1-star)
- [⭐⭐ 2 星挑战](https://github.com/whyiest/juice-shop-write-up/tree/main/2-stars)
- [⭐⭐⭐ 3 星挑战](https://github.com/whyiest/juice-shop-write-up/tree/main/3-stars)
- [⭐⭐⭐⭐ 4 星挑战](https://github.com/whyiest/juice-shop-write-up/tree/main/4-stars)
- [⭐⭐⭐⭐⭐ 5 星挑战](https://github.com/whyiest/juice-shop-write-up/tree/main/5-stars)
- [⭐⭐⭐⭐⭐⭐ 6 星挑战](https://github.com/whyiest/juice-shop-write-up/tree/main/6-stars)

## 建议

- **配套指南**：建议结合阅读 [OWASP Juice Shop 官方配套指南](https://pwning.owasp-juice.shop/companion-guide/latest/)，以获取更多背景信息和说明。
- **先尝试再查阅**：本仓库是有价值的参考资料，但建议先自行尝试解决挑战，再查看解题说明。这样能最大限度地提升学习效果，并加深对 Web 应用安全的理解。
- **使用流程**：进入当前挑战对应的文件夹，阅读解题说明以了解漏洞和修复步骤，并根据需要参考相关脚本或文件。

**注意**：部分挑战说明没有收录全部截图。如果需要更直观的参考，可以查看 `assets` 文件夹，其中可能包含该挑战的其他截图。

## 挑战列表

# 目录

### ➤ 难度：1 星（⭐）

- [Bonus Payload](1-star/bonus_payload.md) - 跨站脚本（XSS）
- [Bully Chatbot](1-star/bully_chatbot.md) - 应用逻辑
- [Confidential Document](1-star/confidential_document.md) - 敏感数据泄露
- [DOM XSS](1-star/dom_xss.md) - 跨站脚本（XSS）
- [Error Handling](1-star/error_handling.md) - 错误处理
- [Exposed Metrics](1-star/exposed_metrics.md) - 敏感数据泄露
- [Mass Dispel](1-star/mass_dispel.md) - 输入验证不当
- [Missing Encoding](1-star/missing_encoding.md) - 输入验证不当
- [Outdated Allowlist](1-star/outdated_allowlist.md) - 未验证的重定向
- [Privacy Policy](1-star/privacy_policy.md) - 信息泄露
- [Repetitive Registration](1-star/repetitive_registration.md) - 输入验证不当
- [Scoreboard](1-star/scoreboard.md) - 信息泄露
- [Web3 Interface](1-star/web3_interface.md) - 访问控制失效
- [Zero Star](1-star/zero_star.md) - 输入验证不当

### ➤ 难度：2 星（⭐⭐）

- [Admin Section](2-stars/admin_section.md) - 访问控制失效
- [Deprecated Interface](2-stars/depreceted_interface.md) - 安全配置错误
- [Empty User Registration](2-stars/empty_user_registration.md) - 输入验证不当
- [Five-Star Feedback](2-stars/five_star_feedback.md) - 访问控制失效
- [Login Admin](2-stars/login_admin.md) - SQL 注入
- [Login MC SafeSearch](2-stars/login_mcsafesearch.md) - 开源情报（OSINT）
- [Meta GeoStaking](2-stars/meta_geostaking.md) - 敏感数据泄露
- [NFT Takeover](2-stars/nft_takeover.md) - 敏感数据泄露
- [Password Strength](2-stars/password_strength.md) - 身份验证失效
- [View Basket](2-stars/view_basket.md) - 访问控制失效
- [Visual GeoStaking](2-stars/visual_geostaking.md) - 敏感数据泄露
- [Weird Crypto](2-stars/weird_crypto.md) - 密码学问题
- [White Hat](2-stars/white_hat.md) - 开源情报（OSINT）
- [Reflected XSS](2-stars/reflected_xss.md) - 反射型跨站脚本（XSS）

### ➤ 难度：3 星（⭐⭐⭐）

- [Admin Registration](3-stars/admin_registration.md) - 输入验证不当
- [Bjoern’s Favorite Pet](3-stars/bjoern_favorite_pet.md) - 开源情报（OSINT）
- [CAPTCHA Bypass](3-stars/captcha_bypass.md) - 自动化防护失效
- [CSRF](3-stars/csrf.md) - 访问控制失效
- [Database Schema](3-stars/database_schema.md) - 信息泄露
- [Deluxe Fraud](3-stars/deluxe_fraud.md) - 输入验证不当
- [Forged Feedback](3-stars/forged_feedback.md) - 访问控制失效
- [Forged Review](3-stars/forged_review.md) - 访问控制失效
- [GDPR Data Erasure](3-stars/gdpr_data_erasure.md) - 身份验证失效
- [Login Amy](3-stars/login_amy.md) - 暴力破解 / 密码学问题
- [Login Bender](3-stars/login_bender.md) - 注入漏洞
- [Login Jim](3-stars/login_jim.md) - SQL 注入（SQLi）
- [Manipulate Basket](3-stars/manipulate_basket.md) - 访问控制失效
- [Mint the Honey Pot](3-stars/mint_the_honey_pot.md) - 输入验证不当
- [Payback Time](3-stars/payback_time.md) - 业务逻辑错误
- [Privacy Policy Inspection](3-stars/privacy_policy_inspection.md) - 通过隐匿实现安全
- [Product Tampering](3-stars/product_tampering.md) - 访问控制失效
- [Reset Jim's Password](3-stars/reset_jim_password.md) - 开源情报（OSINT）
- [Upload Size](3-stars/upload_size.md) - 输入验证不当

### ➤ 难度：4 星（⭐⭐⭐⭐）

- [Access Log](4-stars/access_log.md) - 敏感数据泄露
- [Allowlist Bypass](4-stars/allowlist_bypass.md) - 访问控制失效
- [Christmas Special](4-stars/christmas_special.md) - 注入漏洞
- [Easter Egg](4-stars/easter_egg.md) - 访问控制失效
- [Ephemeral Accountant](4-stars/ephemeral_accountant.md) - 注入漏洞
- [Expired Coupon](4-stars/expired_coupon.md) - 输入验证不当
- [Forgotten Sales Backup](4-stars/forgotten_sales_backup.md) - 敏感数据泄露
- [GDPR Data Theft](4-stars/gdpr_data_theft.md) - 敏感数据泄露
- [Leaked Unsafe Product](4-stars/leaked_unsafe_product.md) - 注入漏洞
- [Legacy Typosquatting](4-stars/legacy_typosquatting.md) - 密码学问题
- [Login Bender](4-stars/login_bender.md) - 身份验证失效
- [Login Bjoern](4-stars/login_bjoern.md) - 身份验证失效
- [Login Uvogin](4-stars/login_uvogin.md) - 身份验证失效
- [Nested Easter Egg](4-stars/nested_easter_egg.md) - 密码学问题
- [NoSQL Manipulation](4-stars/nosql_manipulation.md) - 注入漏洞
- [Poison Null Bytes](4-stars/poison_null_bytes.md) - 注入漏洞
- [Steganography](4-stars/steganography.md) - 通过隐匿实现安全
- [User Credentials](4-stars/user_credentials.md) - 注入漏洞
- [Vulnerable Library](4-stars/vulnerable_library.md) - 易受攻击的组件
- [Server Side XSS Protection](4-stars/server_side_xss_protection.md) - 跨站脚本（XSS）
- [NoSQL Dos](4-stars/nosql_dos.md) - NoSQL 注入 / 拒绝服务（DoS）
- [X-Header XSS](4-stars/x_header_xss.md) - 跨站脚本（XSS）

### ➤ 难度：5 星（⭐⭐⭐⭐⭐）

- [Blockchain Hype](5-stars/blockchain_hype.md) - 通过隐匿实现安全
- [Change Bender's Password](5-stars/change_bender_password.md) - 身份验证失效
- [Cross-Site Imaging](5-stars/cross_site_imaging.md) - 跨站请求伪造（CSRF）
- [Email Leak](5-stars/email_leak.md) - 安全配置错误
- [Extra Language](5-stars/extra_language.md) - 本地化
- [Frontend Typosquatting](5-stars/frontend_typosquatting.md) - 不安全的反序列化
- [Kill Chatbot](5-stars/kill_chatbot.md) - 应用逻辑
- [Leaked Access Log](5-stars/leaked_access_log.md) - 敏感数据泄露
- [Reset Bjoern's Password](5-stars/reset_bjoern_password.md) - 身份验证失效
- [Reset Morty's Password](5-stars/reset_morty_password.md) - 安全配置错误
- [Retrieve Blowprint](5-stars/retrieve_blueprint.md) - 信息泄露
- [Supply Chain Attack](5-stars/supply_chain_attack.md) - 易受攻击的组件
- [Two-Factor Authentication Bypass](5-stars/two_factor_authentification.md) - 身份验证失效
- [Unsigned JWT](5-stars/unsigned_jwt.md) - 身份验证失效
- [Local File Read](5-stars/local_file_read.md) - 服务端注入
- [NoSQL Exfiltration](5-stars/nosql_exfiltration.md) - NoSQL 注入
- [Blocked RCE Dos](5-stars/blocked_rce_dos.md) - 不安全的反序列化

### ➤ 难度：6 星（⭐⭐⭐⭐⭐⭐）

- [Forged Coupon](6-stars/forged_coupon.md) - 密码学
- [Forged Signed JWT](6-stars/forged_signed_jwt.md) - 易受攻击的组件
- [Imaginary Challenge](6-stars/imaginary_challenge.md) - 密码学问题
- [Login Support Team](6-stars/login_support_team.md) - 安全配置错误
- [Multiples Likes](6-stars/multiples_likes.md) - 自动化防护失效
- [Premium Paywall](6-stars/premium_paywall.md) - 访问控制
- [SSRF](6-stars/ssrf.md) - 服务端请求伪造
- [SSTi](6-stars/ssti.md) - 服务端模板注入
- [Wallet Depletion](6-stars/wallet_depletion.md) - 密码学问题
- [Video XSS](6-stars/video_xss.md) - 跨站脚本（XSS）
- [Arbitrary File Write](6-stars/arbitrary_file_write.md) - 易受攻击的组件
- [Successful Rce Dos](6-stars/successful_rce_dos.md) - 远程代码执行（RCE）/ 拒绝服务（DoS）

## 贡献

尽管本仓库已经涵盖了绝大多数挑战，但由于一些技术限制，仍有部分挑战尚未完成。欢迎通过贡献来改进解题说明、脚本或其他资源。请提交 Pull Request，分享你建议的更改或增强内容。

本仓库由社区维护，并非 OWASP Juice Shop 项目的官方组成部分。它是一个协作平台，供安全爱好者和专业人士分享知识、提升 Web 应用安全技能。

祝你攻关愉快！
