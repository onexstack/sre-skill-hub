# 技术中心 Technology Center

> 负责规划、实施和维护公司的信息技术基础设施，推动技术创新和数字化转型。

> **本文件用途**：仅作为部门索引与路由表，**不存储具体负责人信息**。所有负责人详情统一收敛在子文件。

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

| 子团队 | 详情文件锚点 | 路由关键词                                                                                                                                                                                                                                                                                              |
| --- | --- |----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 自动化工具 & 平台基座建设 (Ops Platform) | `### 自动化工具 & 平台基座建设 (Ops Platform)` | 运维自动化平台 / Jumpserver / 白屏运维 / 巡检平台 / 运维门户 / Spug                                                                                                                                                                                                                                                   |
| 基础设施运维 (Infrastructure Operations Group) | `### 基础设施运维组 (Infrastructure Operations Group)` | 公有云 (AWS/Aliyun/HuaweiCloud/BytePlus) / 大数据 (Spark/Flink/StarRocks) / 中间件实例 (RocketMQ/Kafka/ELK/Canal/ZK 实例侧) / IaC / Terraform / 公有云网络 / 服务器实例 / 对象存储 (S3) / 日志平台 (ELK/OpenObserve)                                                                                                               |
| 稳定性和业务交付组 (SRE) | `### 稳定性和业务交付组 (SRE)` | 主站稳定性 / CDN / GA / SEO / DNS / GTM / HttpDNS / 域名证书 / openresty-edge / 公有云 LB / BKE / 运维网关 / 服务部署发布 (EC2/EKS/Ingress) / Shark站 / 监控告警 (Flashcat/VM) / 内部系统 (Nginx/LDAP/Gitlab/JumpServer)                                                                                                          |
| DBA | `### DBA` | MySQL / RDS / Redis / MongoDB / TiDB / DocumentDB / Postgres / Neptune / Noshadow / Archery / B-DMS / DTS / DMS / Shark站数据库                                                                                                                                                                        |
| 基础架构 (Performance Optimisation Team) | `### 基础架构(Performance Optimisation Team)` | 中间件 SDK (Canal/Apollo/Eureka/xxljob) / 监控体系 (alter-hook/pside-car) / 全链路监控组件 (cat/skycat/rasp/skywalking) / 安全网关 (shenyu) / 业务网关 (upex-gateway) / Common包 / Redis SDK (Jedis/Lettuce) / upex-reactive-feign / RPC (Aeron/Netty) / Kafka SDK / RocketMQ SDK (SDK 侧) / ETCD SDK / 多泳道 / 三方风控 (顶象/极验) |

**专项小节（命中即用，跳过通读）**：

| 专项 | 详情文件锚点 | 路由关键词 |
| --- | --- | --- |
| Gitlab 专项 | `### Gitlab 专项负责人地图` | 权限管理 / SSH Key / Token / Runner 配置 / Code Review 配置 / 安全 Review / 后端 Review / GPG 签名 / GPG 加白 / Merge 报错 / 发布卡点 / 接口限制权限 / 发布检测白名单 |
| Lark 审批指引 | `### Lark 审批指引地图` | N20 数据库 / N34 云平台账号 (AK/SK/Role/SSO) / N17 EAA 应用入口 / N30 测试环境访问 / N102 LiteLLM / M27 Jfrog / M29 运维门户 / M82 云枢网络白名单 / G09 OpenSearch / G23 JumpServer 用户 / G20 JumpServer 资产授权 / G33 JumpServer 时长 / G27 JumpServer MFA / G131 BKE 集群权限 |

### 1.2 区块链部门 BlockChain Team

> 研究和应用区块链技术，支持多条公链集成，提升钱包安全性与易用性。
> **详情锚点**：详情文件 `## 区块链部门 BlockChain Team` 小节。
> 路由关键词：公链 / 节点 / 钱包 / 签名机 / MPC / Enclave / Pos / 资金托管 / 资金水位 / 链上对账 / 公共包升级 / 测试环境维护。

### 1.3 Security Team 安全部门

> 管理系统与数据安全，强化访问控制、数据保护、监控与风险防范。
> **详情锚点**：详情文件 `## Security Team 安全部门` 小节，按方向细分为以下子小节：

| 安全方向 | 详情文件锚点 | 路由关键词                                                         |
| --- | --- |---------------------------------------------------------------|
| IT 类 | `### IT 类` | Okta / Google / Yubikey 解绑                                    |
| 安全运营类 | `### 安全运营类` | EAA / WAF / Little Snitch / 安全网关 / HIDS / 邮件安全 / 假域名 / 云枢     |
| 应用安全类 | `### 应用安全类` | Apollo CR / JFrog / URL 鉴权 / XSS / CSP / openrasp / 极验 / 白盒扫描 |
| 移动终端安全类 | `### 移动终端安全类` | Akamai网络安全 / 设备风险拦截 / JSAPI / 安全数仓 / H5 跳转                    |
| 区块链安全 | `### 区块链安全` | 合约审计 / 上币审核 / 交易验证 / 应急响应 / Yubikey 技术支持                      |
| 数据安全 | `### 数据安全` | 数据安全评估 / DLP / 虚拟桌面 / 文档溯源 / 敏感权限治理 / 网关日志                    |
| 安全审计 | `### 安全审计` | 外部审计 / 三方 SaaS 评估                                             |
| 安全管理 | `### 安全管理` | 三方 SaaS / 社媒账号 / 各管理后台权限配置                                    |

---
