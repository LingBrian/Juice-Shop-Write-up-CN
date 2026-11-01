# Juice-Shop Write-up：Exposed Metrics Challenge

## 挑战概述

**Title:** Exposed Metrics\
**Category:** Sensitive Data Exposure\
**Difficulty:** ⭐ (1/6)

本挑战需要发现 Web 应用中暴露的 metrics 端点。

## 使用的工具

- Web browser
- Prometheus 监控知识

## 方法与解决方案

完成 Exposed Metrics 挑战的步骤如下：

1. **查阅应用文档**：已知 Juice Shop 使用 Prometheus 收集 metrics。查阅 Prometheus 官方文档，了解其默认配置。结果发现 Prometheus 通常在 `/metrics` 端点暴露 metrics。
2. **访问端点**：在浏览器中访问 `http://127.0.0.1:3000/metrics`，根据 Prometheus 默认配置查找 metrics 端点。
3. **分析暴露的数据**：访问该 URL 后显示了 metrics 数据，包括：
   - 各项服务的启动时间。
   - CPU 和内存使用统计。
   - 详细的运行时和系统健康数据。
   - 文件上传信息，包括类型以及成功/失败次数。

   <img src="../assets/difficulty1/exposed_metrics_1.png" alt="metrics" width="500px">

## 解决方案说明

访问 Prometheus 默认 metrics 端点后，发现了有价值的运行数据。这会带来显著的安全风险，因为系统内部信息遭到暴露，可能帮助恶意人员策划攻击。

## 修复建议

### 立即采取的措施
- **限制访问**：实施访问控制，仅允许具有管理员权限的已认证用户查看 `/metrics` 端点。

### 长期策略
- **网络配置**：确保 metrics 端点无法通过公共互联网访问。如有需要，只允许通过内部网络或 VPN 访问。
- **加强身份验证**：使用强身份验证保护此类端点，并考虑为管理员访问实施双因素身份验证等额外安全措施。
