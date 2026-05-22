# 技术中心 Technology Center

> 负责规划、实施和维护公司的信息技术基础设施，推动技术创新和数字化转型。

> **本文件用途**：仅作为部门索引与路由表，**不存储具体负责人信息**。所有负责人详情统一收敛在子文件，避免双写。

---

## 目录

- [1. 基础设施平台部门 Infrastructure Platform Department](#1-基础设施平台部门-infrastructure-platform-department)
  - [1.1 SRE Team](#11-sre-team)
  - [1.2 区块链部门 BlockChain Team](#12-区块链部门-blockchain-team)
  - [1.3 Security Team 安全部门](#13-security-team-安全部门)

---

## 1 基础设施平台部门 Infrastructure Platform Department

> 涵盖 SRE / 区块链 / 安全 三大方向。
> **详情文件**：[InfrastructurePlatformDepartment.md](./InfrastructurePlatformDepartment.md)

### 1.1 SRE Team

> 负责自动化与标准化运维、资源效率优化、监控告警机制改进。

**子团队 → 详情锚点**：

| 子团队 | 详情文件锚点 |
| --- | --- |
| 自动化工具 & 平台基座建设 (Ops Platform) | `### 自动化工具 & 平台基座建设 (Ops Platform)` |
| 基础设施架构组 | `### 基础设施架构组` |
| 稳定性和业务交付组 (SRE) | `### 稳定性和业务交付组 Site Reliability Engineering` |
| DBA 组 | `### DBA 组` |

**专项小节（命中即用，跳过通读）**：

| 专项 | 详情文件锚点 |
| --- | --- |
| Gitlab 权限 / Code Review / GPG / 安全卡点 / Merge / 接口限制 / 发布检测白名单 | `### Gitlab 专项负责人地图` |
| 公共组件与中间件（Jedis / RPC / 网关 / Kafka / RocketMQ / ETCD / nacos / Apollo / Skywalking / 风控 / 多泳道 / 探针） | `### 公共组件与中间件找人专项指引` |
| 22 类高频工单（发布 / EKS / EC2 / 网络 / 域名 / DB / Redis / Canal / Kafka / Apollo / 网关 / 告警 / 监控 / JumpServer / 大数据 / LiteLLM / CI / 云权限 / 办公 IT 等） | `### 常见问题排查（速查表 + 历史工单 22 类高频问题）` |

### 1.2 区块链部门 BlockChain Team

> 研究和应用区块链技术，支持多条公链集成，提升钱包安全性与易用性。
> **详情锚点**：详情文件 `## 区块链部门 BlockChain Team` 小节。
> 路由关键词：公链 / 节点 / 钱包 / 签名机 / MPC / Enclave / Pos / 资金托管 / 资金水位 / 链上对账 / 公共包升级 / 测试环境维护。

### 1.3 Security Team 安全部门

> 管理系统与数据安全，强化访问控制、数据保护、监控与风险防范。
> **详情锚点**：详情文件 `## Security Team 安全部门` 小节，按方向细分为以下子小节：

| 安全方向 | 详情文件锚点 | 路由关键词 |
| --- | --- | --- |
| IT 类 | `### IT 类` | Okta / Google / Yubikey 解绑 |
| 安全运营类 | `### 安全运营类` | EAA / WAF / Little Snitch / 安全网关 / HIDS / 邮件安全 / 假域名 / 云枢 |
| 应用安全类 | `### 应用安全类` | Apollo CR / JFrog / URL 鉴权 / XSS / CSP / openrasp / 极验 / 白盒扫描 |
| 移动终端安全类 | `### 移动终端安全类` | Akamai / 设备风险拦截 / JSAPI / 安全数仓 / H5 跳转 |
| 区块链安全 | `### 区块链安全` | 合约审计 / 上币审核 / 交易验证 / 应急响应 / Yubikey 技术支持 |
| 数据安全 | `### 数据安全` | 数据安全评估 / DLP / 虚拟桌面 / 文档溯源 / 敏感权限治理 / 网关日志 |
| 安全审计 | `### 安全审计` | 外部审计 / 三方 SaaS 评估 |
| 安全管理 | `### 安全管理` | 三方 SaaS / 社媒账号 / 各管理后台权限配置 |

---
