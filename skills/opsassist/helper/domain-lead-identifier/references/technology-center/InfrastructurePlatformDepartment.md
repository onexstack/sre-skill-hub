# 基础设施平台部门 Infrastructure Platform Department

## SRE Team

### 自动化工具 & 平台基座建设 (Ops Platform)
> **团队职责**：开发规范制定、内外部需求对接、工具平台建设，持续提升自动化水平。
*   **团队总体负责人**: @Aithor Li
*   **核心开发成员**: @Allen Mu, @Aaron Cho, @Tisiphone Fu
*   **具体系统负责人**:
    *   运维自动化平台总体: @Aithor Li
    *   Jumpserver 自动化操作需求: @Downey Wang
    *   白屏运维平台: @Hoon Li
    *   巡检平台: @Frank Chang


### 基础设施架构组 
> **团队职责**：多云管理（安全/成本/巡检）、大数据运维、IaC 建设、网络架构设计。
*   **团队负责人**: @Runner Zhang
*   **公有云管理** (权限/网络/成本/AI自动化巡检): @Ives Gao | @Rocky Liu, @Evan Su
*   **大数据体系运维** (Hadoop/Flink/StarRocks/EMR等): @Bill Zuo | @Hardy H, @Alex Xuan
*   **大数据离线体系 & 阿里云 dataworks + maxcomputer & Hologres权限**: @Rocky Liu
*   **大数据监控**: @Bill Zuo, @Hardy H, @Alex Xuan
*   **大数据离/在线、olap、列簇存储基础架构设计与维护**: @Bill Zuo, @Hardy H, @Alex Xuan
*   **中间件运维** (保证 RocketMQ/RabbitMQ/Kafka/ELK/Canal/ZK 等组件稳定运行): @Lawrence Chen | @Rocky Liu
*   **基础设施即代码 (IaC / Terraform)**: @Night Liu | @Lexon Wang
*   **公有云网络规划与实施**: @Jerry Jiang | @Daniel Xia, @Gavin Liu
*   **EC2 实例管理** (开关机/Ansible初始化/维护): @Drew | @Liwin Xing
*   **网络安全风险控制**: @Runner Zhang


### 稳定性和业务交付组 Site Reliability Engineering
> **团队职责**：负责生产系统稳定性、业务自动化交付能力、开发测试环境治理、监控告警平台迭代，保障主站等核心系统可用性 99.9%，持续提供三端用户的使用体验。
*   **团队负责人**: @Colin Kong
*   **对接研发/安全/测试/运营等跨团队沟通**: @Ricardo.M.Wang
*   **CDN与网络通道** (Cloudflare/Akamai/GA/CN2、静态资源、SEO排错): @Klaus Liu, @Ken Cai
*   **公有云基础网络组件** (LB、域名、证书、磁盘卷、安全组): @Ricardo.M.Wang
*   **K8S 及云原生技术方案落地**: @Colin Kong 团队主导
*   **环境部署与发布** (开发/测试/灰度/生产环境性能优化): @Zed Wang, @Chris Liu, @Scout, @Evan Yu
*   **企业内部系统维护**:
    *   Nginx: @Cody Guo
    *   LDAP: @Ricardo.M.Wang
    *   Gitlab : @Ricardo.M.Wang
    *   Jumpserver: @Sunway Zhong
*   **内部自研运维平台**:
    *   BKE 平台开发: @Lutzow Guo, @Herry Wang
    *   SRE Helper 开发: @Toka Wu
*   **Shark站**： @Kevin Li


### DBA 组
> **团队职责**：数据库自动化运维平台建设、监控巡检、容灾备份方案落地、成本优化。
*   **团队负责人**: @George Wang
*   **云数据库 (RDS/Redis/MongoDB/TiDB等) 部署调优**: @Will Shen, @Gordon Yang, @Leo Lee
*   **数据库内部产品研发** (Noshadow监控 / Archery权限): @Chaos Wang
*   **B-DMS 系统研发与落地**: @Jet Lin, @Christina Liu
*   **自建数据库管理 / 数据迁移同步 (DTS/DMS)**: @George Wang 团队统一负责


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

### 公共组件与中间件找人专项指引

负责人：@Petter Li

*   **Jedis / Lettuce**: @Aiden Yang, @Kelutral Lu
*   **RPC (Aeron / Netty)**: @Kelutral Lu, @Evan Lu
*   **upex-reactive-feign**: @Kelutral Lu, @Evan Lu
*   **网关** (业务网关 Cloud Gateway / 内部网关): @Evan Lu, @Aiden Yang
*   **消息队列** (Kafka / RocketMQ 使用问题咨询): @Aiden Yang
*   **ETCD**: @Kelutral Lu
*   **common 包**: @Kelutral Lu
*   **多泳道架构**: @Kent Zhang
*   **nacos**: @Sayeed Feng
*   **三方风控服务** (顶象 / 极验): @Evan Lu, @Aiden Yang
*   **监控与探针体系** (alterhook / pside-car): @Sayeed Feng, @Andy Song
*   **基础服务模块** (Canal / Apollo / Eureka / XXL-Job / Skywalking / Nacos 等): @Sayeed Feng, @Lorgine Li
*   **公共安全模块** (安全网关Shenyu / 统一权限 / 安全日志): @Evan Lu, @Andy Song
*   **代码质量与工程体系** (CICD/Sonar/压测/混沌工程/安全扫描): @Evan Lu, @Kent Zhang


### 常见问题排查（速查表 + 历史工单 22 类高频问题）

| # | 问题类别 / 场景 | 典型工单示例 / 备注 | 主负责人 | 备份负责人 |
| :-- | :--- | :--- | :--- | :--- |
| 1 | 应用发布/部署失败（Spug 平台） | "执行部署任务失败"、"spug 发布失败"、"测试环境 spug 部署失败"、"exec format error" | @Doge Huang / @Lucinda He / @Eric Kang / @Abel zhang | @Lucinda He / @Doge Huang / @Abel zhang / @Eric Kang |
| 2 | 新服务 / 旗鱼工单后 Spug 看不到、权限申请 | "旗鱼通过后 spug 看不到服务"、"申请发布权限找不到服务" | @Doge Huang / @Lucinda He / @Eric Kang | @Abel zhang / @Doge Huang / @Lucinda He |
| 3 | EKS / 容器 / K8s（Pod 启动、重启、调度） | "pod 一直重启"、"FailedScheduling"、"健康检查超时"、"OOMKilled"；容器环境服务问题优先找 Scout | @Chris Liu / @David Xu / @Evan Yu / @Zed Wang / @Scout | @David Xu / @Chris Liu / @Evan Yu / @Scout / @Zed Wang |
| 4 | EC2 资源（机器申请/扩缩容/磁盘扩容/下线/文件上传下载） | "ec2 磁盘扩容"、"/dev/shm 扩容"、"换机型"、"ec2 下线"、"EC2 服务器上传/下载文件" | @Chris Liu / @David Xu / @Liwin Xing / @Rocky Liu / @Ives Gao | @David Xu / @Chris Liu / @Drew |
| 4a | EC2 安装外部软件审批 | 属于网络运营安全过审范畴 | @Richard Yang | - |
| 4b | 每周五确认下线机器清单 | - | @Rocky Liu | - |
| 5 | 网络打通 / 安全组（G16、G19、跨 VPC、出口 IP 查询、跨云厂商打通） | "G16 工单"、"安全组打通"、"查询出网 IP"、"内网 deny 流量"；跨云厂商网络打通需提前与网络运营安全沟通 (@Richard Yang) | @Klaus Liu / @Daniel Xia / @Jerry Jiang / @Gavin Liu | @Ken Cai / @Jerry Jiang / @Gavin Liu / @Daniel Xia |
| 6 | 域名 / Ingress / NLB（M28 工单、ingress 配置、域名 404/502） | "ingress 地址查询"、"M28 工单"、"web-xxx 502"、"接口 404" | @David Xu / @Klaus Liu / @Aiden Yang | @Chris Liu / @Ken Cai / @Evan Lu / @Evan Yu |
| 7 | MySQL / 数据库权限、慢 SQL、扩容、archery 审批 | "申请数据库权限"、"慢查询"、"archery 401/408"、"建索引" | @George Wang / @Gordon Yang / @Will Shen | @Gordon Yang / @George Wang / @Will Shen（三人轮值） |
| 8 | Redis 连接 / 扩容 / 集群模式确认 | "redis 连接失败"、"redis 扩容"、"测试环境 redis 不通" | @Gordon Yang / @George Wang / @Will Shen | @George Wang / @Gordon Yang / @Will Shen |
| 9 | Canal / Binlog 同步异常 | "canal 任务无法启动"、"binlog 同步问题"、"canal 延迟" | @Andy Song / @Sayeed Feng / @Lawrence Chen | @Sayeed Feng / @Andy Song / @Kyle Yang / @Rocky Liu |
| 10 | Kafka / RocketMQ（消费积压、Topic、消息发送失败） | "kafka topic 告警"、"rocketmq 不消费"、"消息发送 timeout" | @Lawrence Chen / @Aiden Yang / @Sayeed Feng | @Rocky Liu / @Evan Lu / @Andy Song |
| 11 | Apollo（配置中心连接 / 权限 / portal 登录） | "apollo 读取 404"、"apollo portal 部署失败"、"权限申请" | @Andy Song / @Sayeed Feng / @Lawrence Chen | @Sayeed Feng / @Andy Song / @Rocky Liu |
| 12 | 网关(鉴权、路由、超时、403/404) | "网关 502"、"接口 403"、"errorCode 多语言"、"路由不生效" | @Aiden Yang / @Evan Lu | @Evan Lu / @Aiden Yang（互为主备） |
| 13 | 告警规则（alerthook、值班人、阈值调整、误报处理、下线机器关闭告警、电话） | "alerthook 找不到人"、"修改阈值"、"批量去除告警人"、"P1 没打电话"；下线机器需在下线前提前告知 | @Sayeed Feng / @Andy Song / @Alex Yu / @Rock Yu | @Andy Song / @Kyle Yang / @Sayeed Feng / @Rock Yu |
| 14 | 监控大盘 / Grafana / Prometheus 指标采集 | "grafana 看不到指标"、"prometheus 上报异常"、"pod 监控面板" | @Alex Yu / @Rock Yu / @Jeremy.Qiao | @Rock Yu / @Jeremy.Qiao / @Alex Yu |
| 15 | JumpServer / 堡垒机（登录失败、被禁用、账号权限异常、机器授权） | "jumpserver 登录不上"、"账号被禁用"、"无法选机器" | @Ricardo.M.Wang / @Toka Wu / @Frank Chang / @Sunway Zhong / @Jason.cui | @Cody Guo（部分单无备份） |
| 16 | 大数据 / StarRocks / DataWorks / Flink | "starrocks publish partition 失败"、"dataworks 同步异常"、"flink 节点组" | @Bill Zuo / @Alex Xuan / @Hardy H | @Alex Xuan / @Hardy H / @Bill Zuo |
| 17 | LiteLLM（生产/测试配置、超时、重启、加模型） | "litellm 配置修改"、"litellm 504"、"重启 litellm service" | @Evan Yu / @David Xu / @Chris Liu / @Scout / @Zed Wang | @David Xu / @Chris Liu / @Evan Yu / @Zed Wang / @Scout |
| 18 | GitLab / CI / CodeReview / SonarQube 流水线 | "gitlab 不能合并"、"sonar 检测不过"、"CI 缓存"、"GPG code review 不生效" | @Evan Lu / @Andy Song | @Andy Song / @Evan Lu |
| 19 | AWS / 阿里云权限与 Role（KMS、SecretsManager、AssumeRole、ECR） | "AssumeRole 失败"、"KMS Decrypt 无权限"、"SecretsManager 申请"、"阿里云权限" | @Rocky Liu / @Evan Su / @Ives Gao | @Ives Gao / @Rocky Liu / @Evan Su |
| 20 | 办公 / VPN / EAA / 二次验证 / Yubikey / Cursor | "eaa 登录失败"、"yubikey 不能用"、"cursor 连不上"、"yapi 登录不上" | @Toka Wu / @Lutzow Guo / @Colin Kong | @Colin Kong / @Herry Wang / @Toka Wu |
| 21 | 测试环境报错（如 6616 问题） | 容器环境服务问题优先找 Scout | @Kevin Li / @Herry Wang | - |
| 22 | 日常办公 IT（如 Mac 桌面问题） | 自身办公设备问题 | @Winking Wang | - |

## 区块链部门 BlockChain Team

| 业务 | 人员 |
| --- | --- |
| 公链对接排期 | @Chlry |
| 公链节点部署与维护 | @Jacob Zhao |
| 本地站、土耳其站、越南站 | @Curly、@Tif Tian |
| 资金托管服务 | @Jair Li |
| Web3 钱包相关 | @Angus Xu |
| Pos 业务线 | @Richard Joe |
| 签名机相关 | @Hugo Deng |
| MPC、Enclave | @Hugo Deng、@Richard Joe |
| 公共包升级、测试环境维护、对账业务 | @Tif Tian |
| 资金水位（含项目方做市） | @Vic Feng |

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
| Akamai EdgeWorker（appapi.bitgetapp.com 大规模 403） | @Snow Chen |
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
