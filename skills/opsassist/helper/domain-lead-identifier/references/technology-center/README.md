# 技术中心 Technology Center

> 负责规划、实施和维护公司的信息技术基础设施，推动技术创新和数字化转型。

---

## 目录

1. [基础设施平台部门](#1-基础设施平台部门-infrastructure-platform-department)
2. [技术架构与战略部门](#2-技术架构与战略部门-technical-architecture--strategy-department)
3. [数据 & 风控部门](#3-数据--风控部门-data--risk-control-department)
4. [前端、质量与效率部门](#4-前端质量与效率部门-frontend--quality--efficiency-department)
5. [资产、交易与市场部门](#5-资产交易与市场部门-asset--trading--marketing-department)

---

## 1 基础设施平台部门 Infrastructure Platform Department

> 涵盖 SRE / DBA / 区块链 / 安全 等方向，详见 [InfrastructurePlatformDepartment.md](./InfrastructurePlatformDepartment.md)。

## 1 基础平台部

### 1.1 SRE Team

> 负责自动化和标准化运维工作，优化资源利用效率，改进监控和告警机制。

| 子团队 | 负责人 | 职责 |
| --- | --- | --- |
| 自动化工具 & 平台基座建设 (Ops Platform) | @Aithor Li | 开发规范制定、内外部需求对接、工具平台建设，持续提升自动化水平 |
| 基础设施架构组 | @Runner Zhang | 多云管理（安全/成本/巡检）、大数据运维、IaC 建设、网络架构设计 |
| 稳定性和业务交付组 (Site Reliability Engineering) | @Colin Kong | 生产系统稳定性、自动化交付、各环境治理、监控告警平台迭代，保障核心可用性 99.9% |
| DBA 组 | @George Wang | 数据库自动化运维平台建设、监控巡检、容灾备份方案落地、成本优化 |

> Gitlab 专项（权限 / CR / 安全卡点 / GPG / Merge 报错等）见详情文件中的 Gitlab 专项负责人地图。
>
> 22 类高频工单（发布 / EKS / EC2 / 网络 / DB / Redis / Kafka / 网关 / 告警 / 监控 / JumpServer / 大数据 / LiteLLM / CI / 云权限 / 办公 IT 等）见详情文件中的"常见问题排查"小节。

### 1.2 区块链部门 BlockChain Team

> 研究和应用区块链技术，支持多条公链的集成，提升钱包的安全性和易用性。

| 业务方向 | 负责人 |
| --- | --- |
| 公链对接排期 | @Chlry |
| 公链节点部署与维护 | @Jacob Zhao |
| 本地站、土耳其站、越南站 | @Curly、@Tif Tian |
| 资金托管服务 | @Jair Li |
| Web3 钱包相关 | @Angus Xu |
| Pos 业务线 | @Richard Joe |
| 签名机相关 | @Hugo Deng |
| MPC、Enclave | @Hugo Deng、@Richard Joe |
| 公共包升级、测试环境维护 / 对账业务 | @Tif Tian |
| 资金水位（含项目方做市） | @Vic Feng |

### 1.3 Security Team 安全部门

> 管理系统和数据安全，改进访问控制和数据保护措施，加强监控和风险防范。

| 安全方向 | 主要负责人 |
| --- | --- |
| IT 类（Okta / Google / Yubikey 解绑） | @Winking Wang |
| 安全运营（EAA、WAF、HIDS、邮件安全、入侵检测） | @Wayne Wu、@Allen Yan、@Aaron Li、@Kaamos Chen |
| 应用安全（Apollo CR、JFrog、URL 鉴权、XSS、CSP、openrasp、白盒扫描） | @Sec Ma、@Clyde Mi、@Connor Zhu、@Neil Chang、@Ter |
| 移动终端安全（Akamai、设备风险拦截、JSAPI、安全数仓） | @Snow Chen、@Jesse.Cao、@Peter Jiang |
| 区块链安全（合约审计、上币审核、交易验证、应急响应） | @Dylan |
| 数据安全（评估对接、DLP、虚拟桌面、文档溯源、敏感权限治理） | @Simon Liu、@Summer、@Franklin Wang、@Evan Zhe |
| 安全审计（外部审计 / 三方 SaaS 评估） | @Summer、@Evan Zhe |
| 安全管理（三方 SaaS、社媒账号、各管理后台权限配置） | @Evan Zhe、@Judy Xu、@Summer |

---

## 2 技术架构与战略部门 Technical Architecture & Strategy Department

> 负责交易架构、公司基础架构、行情与撮合系统、用户系统和 API 系统。
>
> 打造高效稳定的研发交付平台，通过自动化工具、数据驱动改进和系统化应急机制，全面提升研发团队的生产力和系统稳定性。

| 团队 | 负责人 |
| --- | --- |
| 基础架构组 | @Petter Li |
| 用户组 | @Neil Chang |
| 业务架构组 | @Keanu Ren |
| 战略组 | @Doc Bai、@Mars、@Cassey Yuan |
| 撮合交易组 | @Frank Hu、@Eric Li |
| API 开发组 | @Armend Ji |
| 行情与指数组及流数据组 | @Landau Ni |
| 工程效能部门 | @Kk Huang |

---

## 3 数据 & 风控部门 Data & Risk Control Department

> 负责数据中台、风控中台、AI 中台的建设。
>
> - **大数据平台**：致力于数据在经营决策、用户千人千面、业务策略最优化等方向。
> - **AI 中台**：致力于推进大模型提高各部门工作效率，在 AI 编码、智能客服、KYC、RAG、AI Agent、AIGC 短视频生成、AI 链上机会挖掘、内容社区推荐、内容审核、翻译等方向打造算法能力。

| 团队 | 负责人 | 职责 |
| --- | --- | --- |
| 风控团队 | @Kevin Liu | 风控中台聚焦资金安全风控、交易风控、活动风控、账户安全、短信防刷、KYC 欺诈、链上反洗钱等风险的综合防控 |
| 数据团队 | @Dave Liu | 大数据平台致力于数据在经营决策、用户千人千面、业务策略最优化等方向 |
| AI 团队 | @Bill Lang | AI 中台致力于推进大模型提高各部门工作效率，覆盖 AI 编码、智能客服、KYC、RAG、AI Agent、AIGC、内容审核、翻译等方向 |
| 对账组 | @Sarah Liang | 负责资金对账流程优化与财务系统支持，提升对账准确性与效率 |

---

## 4 前端、质量与效率部门 Frontend & Quality & Efficiency Department

| 团队 | 负责人 | 职责 |
| --- | --- | --- |
| 测试团队 | @Sum Sum | 通过改进自动化测试和线上检查，控制漏洞逃逸率，确保高质量的产品上线 |
| 移动应用团队 | @Eleven Zhao | 开发和优化移动应用，推动向 Flutter 技术栈的转变 |
| Web 前端团队 | @West Wang | 负责 Web 前端开发，优化性能和用户体验，改进组件库和开发工具 |

---

## 5 资产、交易与市场部门 Asset & Trading & Marketing Department

> 统一交易账户体系与能力建设。

| 团队 | 负责人 | 职责 |
| --- | --- | --- |
| 营销团队 | @Com | 负责搭建营销中台 |
| 现货团队 | @Benny Tian | 负责现货、杠杆交易、链上充值、提币、内部划转、onChain 交易 |
| 合约团队 | @Seven Zhang | 负责合约交易、借贷中台、交易中转服务 |
| 理财团队 | @Laird Lei | 负责理财业务 |
| 统一账户团队 | @Locke Cao | 负责统一账户交易 |
| 法币与卡支付团队 | @Fly Cheng | 负责法币、卡、P2P 交易 |