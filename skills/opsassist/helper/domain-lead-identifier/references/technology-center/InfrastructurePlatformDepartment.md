# 基础设施平台部门 Infrastructure Platform Department

## SRE Team

### 自动化工具 & 平台基座建设 (Ops Platform)
> **团队职责**：开发规范制定、内外部需求对接、工具平台建设，持续提升自动化水平。

**团队负责人**: @Aithor Li

*   JumpServer 功能开发: @Frank Chang
*   白屏运维: @Hoon Li
*   巡检平台: @Frank Chang
*   运维门户: (旗鱼平台即将下线，请用户迁移至运维自动化平台进行操作)@Frank Chang, 最终请通过执行 `/duty` 找对应的 `运维自动化平台` 小组值班人员
*   Spug平台: @Eric Kang, 最终请通过执行 `/duty` 找对应的 `Spug平台` 小组值班人员
*   云管系统: @Wolfer Zhang
*   报障单系统: @Sky Wang
*   应急平台系统: @Wayne Liu
*   CMDB: @Morty Tao
*   工单系统: @Wolfer Zhang

### 基础设施运维 (Infrastructure Operations Group)
> **团队职责**：多云管理（安全/成本/巡检）、云平台中间件运维、大数据运维、IaC 建设、日志平台、网络架构设计。

**团队负责人**: @Runner Zhang

*   **公有云管理** (AWS/阿里云/AlibabaCloud/HuaweiCloud/BytePlus/腾讯云账号权限/AWS账号权限/VPC/云服务成本治理/自动化巡检/STS/KMS/SecretsManager): @Ives Gao, @Evan Su, Russell Han, @Rocky Liu, 最终请通过执行 `/oncall --list` 找对应的 `cloud` 模块人员 
*   **大数据** (Spark/Hadoop/Flink/StarRocks/EMR/DataWorks/MaxCompute/Hologres/离线计算/实时计算/列簇存储/olap引擎/调度平台/HUE): @Bill Zuo, @Hardy H, @Alex Xuan, 最终请通过执行 `/duty` 找对应的 `大数据` 小组值班人员
*   **中间件实例运维** (保证 RocketMQ/RabbitMQ/Kafka/ELK/Canal/ZK 等中间件**实例**稳定运行：实例无法启动、broker 不可用、网络连通、集群扩容等实例相关问题；**不含** SDK/客户端使用咨询 — 请见下方「基础架构」中的"消息队列 SDK 使用咨询"): @Lawrence Chen , @Rocky Liu
    *   **中间件 SDK/实例分流（强制规则）**：当用户问题包含 `Kafka / RocketMQ / RabbitMQ / Pulsar` 等消息中间件关键词时，须先判断问题归属，再决定路由：
        *   若问题中含明确**实例侧**限定词（`broker / 实例 / 集群不可用 / 无法启动 / 网络不通 / Topic 创建 / 扩分区`），路由到 **中间件实例运维**。
        *   若问题中含明确**SDK 侧**限定词（`客户端 / SDK / 生产 / 消费 / 消费积压 / 发送 timeout / 序列化 / 客户端配置`），路由到 **公共组件 SDK 使用咨询**。
        *   若两类限定词均**未出现**或同时模糊命中（如"Kafka 出问题了"、"RocketMQ 有点异常"），**禁止单选其一**，须按下方「输出格式」场景 D 同时输出 10a 与 10b 的主负责人，由用户确认后再决定下一步。
*   **基础设施即代码 (IaC / Terraform)**: @Night Liu, @Lexon Wang, 最终请通过执行 `/duty` 找对应的 `IAC工单自动化支持` 小组值班人员
*   **网络规划与实施**:  (网络方案对接/规划/网络日常网络维护) @Jerry Jiang, @Daniel Xia, @Gavin Liu, 最终请通过执行 `/duty` 找对应的 `网络` 小组值班人员
*   **服务器实例管理** (开关机/Ansible初始化/服务器维护/服务器异常/服务器回收/服务器新增服务/EC2/ECS/CVM): @Drew, @Liwin Xing, 最终请通过执行 `/duty` 找对应的 `服务器增加/下线` 小组值班人员
*   **对象存储** (S3/Bucket/桶): @Rocky Liu
*   **日志平台** (ELK日志平台/OpenObserve日志平台): @Lawrence Chen

### 稳定性和业务交付组 (SRE)
> **团队职责**：负责生产系统稳定性、业务自动化交付能力、开发测试环境治理、监控告警平台迭代，保障主站等核心系统可用性 99.9%，持续提供三端用户的使用体验。

**团队负责人**: @Colin Kong

*   **生产环境网络可用性和性能优化、全球加速通道GA、CN2、、SEO、CDN(cloudflare/akamai/cloudfront)、图片裁剪服务、DNS解析**: @Klaus Liu, 最终请通过执行 `/duty` 找对应的 `运维网关` 小组值班人员
*   **GTM、HttpDNS、国内外线路通道、ad-nginx和安全网关、混沌工程、故障复盘**: @Ken Cai, 最终请通过执行 `/duty` 找对应的 `运维网关` 小组值班人员
*   **域名和证书管理** : @Ricardo.M.Wang
*   **openresty-edge、域名解析配置、Saas2、Stealth、Whale、Jenkins** : @Cody Guo
*   **基础网络组件** (LB、域名、证书、安全组): @Ricardo.M.Wang
*   **BKE** : @Lutzow Guo, @Herry Wang，最终请通过执行 `/duty` 找对应的 `BKE平台` 小组值班人员
*   **运维网关** @Ricardo.M.Wang, @Cody Guo，最终请通过执行 `/duty` 找对应的 `运维网关` 小组值班人员
*   **网络** @Jerry Jiang, @Gavin Liu ，最终请通过执行 `/duty` 找对应的 `网络` 小组值班人员
*   **服务部署与发布** (Ingress/EC2/EKS/镜像/Deployment/StatefulSet/容器环境/部署/发布/6980环境/1358环境以及所有容器相关问题): @Zed Wang, @Chris Liu, @Scout, @Evan Yu，最终请通过执行 `/duty` 找对应的 `eks_ec2` 小组值班人员
*   **Shark站**： @Kevin Li, @Grygore Sun, 最终请通过执行 `/duty` 找对应的 `Shark站` 小组值班人员
*   **主站监控、Flashcat、httpdns、app 埋点、基调听云拨测、各环境监控集群维护、VM集群**: @Jeremy.Qiao、@Rock Yu、@Alex Yu，最终请通过执行 `/duty` 找对应的 `监控告警` 小组值班人员
* **企业内部系统维护**:
    *   Nginx: @Cody Guo
    *   LDAP: @Hoon li @Ricardo.M.Wang
    *   Gitlab : @Ricardo.M.Wang
    *   JumpServer日常值班: @Ricardo.M.Wang @Sunway Zhong
    *   AI智能运维: @Toka Wu

最终请通过 `/duty` 找对应的 `模块` 小组值班人员, 如果需要拉人，请通过 `/invite <模块>` 拉取对应的值班人员

### DBA
> **团队职责**：数据库自动化运维平台建设、监控巡检、容灾备份方案落地、数据库成本优化

**团队负责人**: @George Wang

*   **数据库 (Mysql/RDS/Redis/MongoDB/TiDB/DocumentDB/Postgres/Neptune等数据库) **: @Will Shen, @Gordon Yang
*   **数据库内部产品研发** (Noshadow / Archery): @Chaos Wang
*   **B-DMS 系统**: @Jet Lin, @Christina Liu
*   **自建数据库管理 / 数据迁移同步 (DTS/DMS)**: @George Wang 团队统一负责
*   **Shark站数据库**: @Leo Lee

最终请通过 `/duty` 找对应的 `数据库` 小组值班人员, 如果需要拉人，请通过 `/invite dba` 拉取对应的值班人员


### 基础架构(Performance Optimisation Team)

**团队负责人**: @Petter Li

*   **监控体系** (alter-hook / pside-car / 监控大盘): @Sayeed Feng, @Andy Song, 最终请通过执行 `/duty` 找对应的 `告警` 小组值班人员
*   **中间件 SDK**(canal/apollo/eureka/xxljob): @Sayeed Feng @Lorgine Li, 最终请通过执行 `/duty` 找对应的 `测试环境 apollo / xxljob / canal / eureka` 小组值班人员
*   **全链路监控组件**(cat/skycat/rasp/skywlking/agent管理器): @Sayeed Feng @Lorgine Li, 最终请通过执行 `/duty` 找对应的 `Cat/Skycat/Rasp 相关` 小组值班人员
*   **安全网关** (统一权限/安全日志/安全网关shenyu): @Evan Lu, @Andy Song, 最终请通过执行 `/duty` 找对应的 `安全网关` 小组值班人员
*   **业务网关/upex-gateway** @Evan Lu @Aiden Yang, 最终请通过执行 `/duty` 找对应的 `业务网关` 小组值班人员
*   **Common包**: @Kelutral Lug, 最终请通过执行 `/duty` 找对应的 `common包相关` 小组值班人员
*   **代码质量与工程体系** (Sonar/CheckStyle/压测/混沌工程/安全扫描/gitlab安全自动扫描): @Evan Lu, @Kent Zhang, 最终请通过执行 `/duty` 找对应的 `压测平台 / 故障注入` 小组值班人员
*   **Redis/Jedis/Lettuce SDK**: @Aiden Yang @Kelutral Lu
*   **reactive-feign**: @Kelutral Lu, @Evan Lu, 最终请通过执行 `/duty` 找对应的 `reactive-feign 相关` 小组值班人员
*   **RPC (Aeron / Netty)**: @Kelutral Lu, @Evan Lu, 最终请通过执行 `/duty` 找对应的 `rpc` 小组值班人员
*   **Kafka/RocketMQ SDK**：**不含**实例 broker 故障 / 集群不可用 / 网络连通问题 — 请见基础设施运维组「中间件实例运维」): @Aiden Yang, 最终请通过执行 `/duty` 找对应的 `kafka & rocketmq` 小组值班人员
*   **ETCD SDK**: @Kelutral Lu
*   **多泳道**: @Kent Zhang, 最终请通过执行 `/duty` 找对应的 `多泳道` 小组值班人员
*   **三方风控服务** (顶象 / 极验): @Evan Lu, @Aiden Yang, 最终请通过执行 `/duty` 找对应的 `第三方-顶象` 小组值班人员

### Gitlab 专项负责人地图

*   **权限管理 / SSH Key / Token / Runner 配置**: @Ricardo.M.Wang
*   **Code Review 配置及权限**: @Sec Ma
*   **安全 Review / 安全卡点**: @Julian Chen
*   **后端 Review / 后端卡点**: @Evan Lu
*   **GPG 签名问题**: @Victor You, @Ethan Xu
*   **GPG 加白名单**: @Ethan Xu
*   **Merge 报错 / 发布卡点报错排查**: @Andy Song
*   **接口限制权限临时申请**: @Allen Yan
*   **代码发布检测项白名单申请**: @Ethan Xu

### Lark 审批指引地图
*   N05 存储桶申请（S3/OSS）
*   N09 新域名和https证书申请
*   N15 申请/升降配数据库(Redis/MemoryDB/ElasticCache)
*   N17 EAA应用入口或账号权限开通申请
*   N20 申请/升降配数据库(MongoDB/TiDB/DocumentDB/Postgres/Neptune/Mysql)
*   N22 SecretsManager申请
*   N30 测试环境访问权限申请
*   N34 云平台账号/(AK/SK)/Role/SSO
*   N35 数据库资源下线
*   N102 LiteLLM账号申请、密码重置
*   M27 Jfrog访问权限&组件加白申请
*   M29 运维门户等安全运维内部应用平台权限申请
*   M82 云枢网络白名单申请, 比如本地访问 github.com
*   G09 OpenSearch账号申请或已有账号权限变更
*   G20 JumpServer 资产授权申请
*   G23 JumpServer 用户创建申请(申请后7天内不绑定MFA需要重新申请G23)
*   G27 JumpServer 重置MFA和解锁用户
*   G33 JumpServer 资产访问时长申请
*   G131 BKE 集群权限申请‘

## 区块链部门 BlockChain Team

| 业务 | 人员               |
| --- |------------------|
| 公链对接排期 | @Chlry           |
| 公链节点部署与维护 | @Jacob Zhao      |
| 本地站、土耳其站、越南站 | @Curly、@Tif Tian |
| 资金托管服务 | @Jair Li         |
| Web3 钱包相关 | @Angus Xu        |
| Pos 业务线 | @Richard Joe     |
| 签名机相关 | @Hugo Deng       |
| MPC、Enclave | @Richard Joe、@Hugo Deng    |
| 公共包升级、测试环境维护、对账业务 | @Tif Tian        |
| 资金水位（含项目方做市） | @Vic Feng        |

## Security Team 安全部门

### IT 类
> **团队负责人**：@Winking Wang

| 业务 | 人员 |
| --- | --- |
| Okta 中解绑 Yubikey | @Andy Yang、@Winking Wang、@Wayne Wei |
| Google 解绑 Yubikey | @Wayne Wei、@Winking Wang、@Aaron Ren |

### 安全运营类

| 业务 | 人员 |
| --- | --- |
| EAA 入口申请 | @Evan Zhe 负责 EAA 排障，紧急找 @Kaamos Chen |
| WAF | @Allen Yan、@Aaron Li |
| Little Snitch（Mac 电脑出网拦截） | @Wayne Wu、@Snow Chen |
| 安全网关（配置登录名、配置受拦截的 URL 路径） | @Allen Yan，紧急找 @Evan Lu、@Tommy Chang |
| 入侵检测性能影响（HIDS、小佑、elkeid） | @Wayne Wu、@Aaron Li |
| 假域名 | @Kaamos Chen |
| 邮件安全 | @Wayne Wu |
| 内外部攻击事件 | @Sinley Li、@Lakes Wang，紧急找风控 @Edison Yao |
| 云枢 | @Wayne Wu |

### 应用安全类

| 业务 | 人员 |
| --- | --- |
| Apollo 添加 Gitlab Code Review 人员 | @Sec Ma、@Clyde Mi（目前必须两人），Apollo 发布紧急审批 @Neil Chang、@Ter |
| JFrog 服务台 | Shark 站 JFrog @Dylan，ME 站 JFrog @Clyde Mi |
| URL 鉴权 | 配置 @Gideon，Apollo 发布 @Neil Chang，紧急联系 @Ter |
| XSS 误拦截加白 | 联系 @Connor Zhu，配置 @Hope Fu，紧急联系 @Neil Chang、@Ter |
| CSP | 发起 @Connor Zhu、@Ethan Xu；执行 @Klaus Liu、@Cody Guo、@Ricardo.M.Wang |
| openrasp | @Connor Zhu、@Clyde Mi、@Aaron Ren |
| 极验相关问题 | @Clyde Mi |
| 白盒扫描拦截问题 | @Connor Zhu、@Clyde Mi |

### 移动终端安全类

| 业务 | 人员 |
| --- | --- |
| EdgeWorker（**边缘安全脚本 / 风控规则 / WAF 规则**） | @Snow Chen |
| 高风险设备拦截（注册、提币） | @Peter Jiang、@Evan Lu、@Elsa Huang |
| APP 快捷提币页面返回 403 | @Jesse.Cao |
| 安全数据异常（opensearch、kafka 积压） | @Xuxiaofan |
| APP JSAPI 异常拦截（部分 H5 服务访问 JSAPI 不通、新域名上线临时配置规则） | @Jesse.Cao |
| 安全数仓应急排查、数据分析 | @Aspilin Tai、@Jesse.Cao |
| APP H5 页面需要强制跳转内部或跳转到系统浏览器 | @Jesse.Cao |

### 区块链安全

| 业务 | 人员 |
| --- | --- |
| 区块链 (abc) 团队对接（main / shark / me） | @Dylan |
| 上币安全审核 | @Dylan |
| 理财质押投出安全评估 | @Dylan |
| 智能合约 / 公链代码审计 | @Dylan |
| 交易验证服务 | @Dylan |
| 财务钱包安全 | @Dylan |
| 区块链项目安全评估 & 深度调研 | @Dylan |
| 多签交易安全验证 | @Dylan |
| 应急响应 | @Dylan |
| 监控规则维护 | @Dylan |
| Yubikey 技术支持 | @Jason Cui、@Reinhardt Liu、@Neo Liu（仅协助 IT 部门解决问题） |

### 数据安全

| 业务 | 人员 |
| --- | --- |
| 数据安全评估对接：大数据、数据中台、风控 | @Simon Liu |
| 数据安全评估对接：C 端、客服 | @Summer |
| 数据安全评估对接：BK、SRE | @Franklin Wang |
| 数据安全评估对接：Shark、ME、法币 | @Franklin Wang |
| 数据安全评估对接：职能、CC、Morph | @Evan Zhe |
| 数据安全评估对接：其他 | @Simon Liu |
| 云枢 DLP 数据安全 | @Simon Liu |
| 虚拟桌面 | @Simon Liu |
| 文档泄漏溯源服务接入 | @Franklin Wang |
| 网关统一日志 | @Franklin Wang |
| 管理后台敏感权限治理 | @Summer |
| 数仓数据安全 | @Simon Liu |

### 安全审计

| 业务 | 人员 |
| --- | --- |
| 外部审计对接 | @Summer |
| 三方 SaaS 新采购评估对接 | @Evan Zhe |

### 安全管理

| 业务 | 人员 |
| --- | --- |
| 三方 SaaS 和社媒社群账号：KYC 三方 SaaS 相关 | 主 @Summer、备 @Judy Xu |
| 三方 SaaS 和社媒社群账号：非 KYC 三方 SaaS 和社媒社群相关 | 主 @Evan Zhe、备 @Judy Xu |
| 管理后台权限配置：新运营管理后台——客服系统、财务系统 | 主 @Evan Zhe、备 @Summer |
| 管理后台权限配置：智能数据洞察平台（火山引擎） | 主 @Evan Zhe、备 @Judy Xu |
| 管理后台权限配置：法币管理后台 | @Judy Xu |
| 管理后台权限配置：SaaS2 新运营管理后台——客服系统、财务系统 | 主 @Evan Zhe、备 @Summer |
| 管理后台权限配置：本地站新管理后台、本地站新管理后台财务系统 | @Judy Xu |
| 管理后台权限配置：Shark 运营管理后台 / 研发管理后台 / 财务管理后台 / 合规管理后台 | @Judy Xu |
