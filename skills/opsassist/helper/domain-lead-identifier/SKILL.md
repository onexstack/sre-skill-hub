---
name: domain-lead-identifier
description: 用于“找负责人/找对接人/找人/谁负责/值班人”场景。当用户需要了解某个工作、值班模块或问题由谁负责、需要对接哪个负责人（如咨询处理、升级求助、事件对接）时触发，通过严格流程解决“找人”类场景。
---

# 找人 (Find On-Call Person)

## 工作流程

### 步骤 1：查找值班人 (优先)

1. **获取模块列表**：静默执行 `/oncall --list` 获取所有可用的值班模块及其描述。
2. **匹配模块**：根据用户的具体问题描述，与获取到的列表进行语义匹配，找出最合适的 `<module>`。
   - *分支逻辑*：如果完全匹配不到合适的值班模块，请立即终止当前步骤，跳转至 **步骤 2**。
3. **获取值班人**：执行 `/duty <module>` 获取该模块的主备值班人。优先提取主值班人（列表的第一个人员）。
4. **工单创建判断**：
   - 仅当用户在上下文中**明确表达**了“拉群”、“建单”、“需要找他看问题”等意图时，直接提取问题描述作为 `<title>` (默认: "问题咨询")，执行 `/oncall <module> <title>`。
   - 若用户仅询问“谁负责”，则停止操作，直接汇报值班人结果。

### 步骤 2：基于文档查找负责人 (备选)**

如果 **步骤 1** 无法匹配到对应的值班模块，则阅读以下 Markdown 内容，根据内容查找负责人：
```markdown
# 组织架构与找人指南 (Who-to-Contact Directory)

> **文档说明**：本文档用于快速定位内部系统、基础设施、研发组件及各类故障场景的对应负责人。人员统一使用 `@姓名` 标识。

## 1. Gitlab 专项负责人地图

*   **权限管理 / SSH Key / Token / Runner 配置**: @Ricardo.M.Wang
*   **Code Review 配置及权限**: @Sec Ma
*   **安全 Review / 安全卡点**: @Julian Chen
*   **后端 Review / 后端卡点**: @Evan Lu
*   **GPG 签名问题**: @Victor You, @Ethan Xu
*   **GPG 加白名单**: @Ethan Xu
*   **Merge 报错 / 发布卡点报错排查**: @Andy Song
*   **接口限制权限临时申请**: @Allen Yan
*   **代码发布检测项白名单申请**: @Ethan Xu

---

## 2. SRE Team (站点可靠性工程团队)

### 2.1 自动化工具 & 平台基座建设 (Ops Platform)
> **团队职责**：开发规范制定、内外部需求对接、工具平台建设，持续提升自动化水平。
*   **团队总体负责人**: @Aithor Li
*   **核心开发成员**: @Allen Mu, @Aaron Cho, @Tisiphone Fu
*   **具体系统负责人**:
    *   运维自动化平台总体: @Aithor Li
    *   Jumpserver 自动化操作需求: @Frank Chang
    *   白屏运维平台: @Hoon Li
    *   巡检平台: @Frank Chang

### 2.2 基础架构 (Infrastructure Operations)
> **团队职责**：多云管理（安全/成本/巡检）、大数据运维、IaC 建设、网络架构设计。
*   **团队负责人**: @Runner Zhang
*   **公有云管理** (权限/网络/成本/AI自动化巡检): @Ives Gao | @Rocky Liu, @Evan Su
*   **大数据体系运维** (Hadoop/Flink/StarRocks/EMR等): @Bill Zuo | @Hardy H, @Alex Xuan
*   **大数据离线体系 & Hologres权限**: @Rocky Liu
*   **中间件运维** (RocketMQ/RabbitMQ/Kafka/ELK/Canal/ZK): @Lawrence Chen | @Rocky Liu
*   **基础设施即代码 (IaC / Terraform)**: @Night Liu | @Lexon Wang
*   **公有云网络规划与实施**: @Jerry Jiang | @Daniel Xia, @Gavin Liu
*   **EC2 实例管理** (开机/Ansible初始化/维护): @Drew | @Liwin Xing
*   **网络安全风险控制**: @Runner Zhang

### 2.3 稳定性和业务交付 (Site Reliability Engineering)
> **团队职责**：生产系统稳定性、自动化交付、各环境治理、监控告警平台迭代，保障核心可用性。
*   **团队负责人**: @Colin Kong
*   **对接研发/安全/测试/运营等跨团队沟通**: @Ricardo.M.Wang
*   **CDN与网络通道** (Cloudflare/Akamai/GA/CN2、静态资源、SEO排错): @Klaus Liu, @Ken Cai
*   **公有云基础网络组件** (LB、域名、证书、磁盘卷、安全组): @Ricardo.M.Wang
*   **K8S 及云原生技术方案落地**: @Colin Kong 团队主导
*   **环境部署与发布** (开发/测试/灰度/生产环境性能优化): @Zed Wang, @Chris Liu, @Scout, @Evan Yu
*   **企业内部系统维护**:
    *   Nginx: @Cody Guo
    *   LDAP: @Williams Jones
    *   Gitlab / Jumpserver: @Ricardo.M.Wang
*   **内部自研运维平台**:
    *   BKE 平台开发: @Lutzow Guo, @Herry Wang
    *   SRE Helper 开发: @Toka Wu

### 2.4 数据库管理 (DBA)
> **团队职责**：数据库自动化运维平台建设、监控巡检、容灾备份方案落地、成本优化。
*   **团队负责人**: @George Wang
*   **云数据库 (RDS/Redis/MongoDB/TiDB等) 部署调优**: @Will Shen, @Gordon Yang, @Leo Lee
*   **数据库内部产品研发** (Noshadow监控 / Archery权限): @Chaos Wang
*   **B-DMS 系统研发与落地**: @Jet Lin, @Christina Liu
*   **自建数据库管理 / 数据迁移同步 (DTS/DMS)**: @George Wang 团队统一负责

---

## 3. 基础架构组 (研发侧组件与中间件)

*   **Redis / Jedis / Lettuce**: @Aiden Yang, @Kelutral Lu
*   **RPC (Aeron / Netty)**: @Kelutral Lu, @Evan Lu
*   **upex-reactive-feign**: @Kelutral Lu, @Evan Lu
*   **网关** (业务网关 Cloud Gateway / 内部网关): @Evan Lu, @Aiden Yang
*   **消息队列** (Kafka / RocketMQ): @Aiden Yang
*   **ETCD**: @Kelutral Lu
*   **多泳道架构**: @Kent Zhang
*   **三方风控服务** (顶象 / 极验): @Evan Lu, @Aiden Yang
*   **监控与探针体系** (alter-hook / pside-car): @Sayeed Feng, @Andy Song
*   **基础服务模块** (Canal / Apollo / Eureka / XXL-Job / Skywalking等): @Sayeed Feng, @Lorgine Li
*   **公共安全模块** (安全网关Shenyu / 统一权限 / 安全日志): @Evan Lu, @Andy Song
*   **代码质量与工程体系** (CICD/Sonar/压测/混沌工程/安全扫描): @Evan Lu, @Kent Zhang

---

## 4. 常见问题排查与资源申请 (速查表)

| 问题场景 / 需求分类 | 对应处理人 / 团队 | 备注说明 |
| :--- | :--- | :--- |
| **测试环境报错** (如6616问题) | @Kevin li, @Herry Wang | 容器环境服务问题优先找 Kevin li |
| **容器 / K8s 相关问题** | @Kevin li | 需安排伙伴协助查看 |
| **EC2 服务器上传 / 下载文件** | @Rocky Liu, @Ives Gao | - |
| **EC2 安装外部软件审批** | @Richard Yang | 属于网络运营安全过审范畴 |
| **跨云厂商网络打通需求** | @Richard Yang | 需提前与网络运营安全沟通 |
| **监控告警规则调整 / 误报处理** | @Rock Yu, @Alex Yu | - |
| **下线机器需关闭监控告警** | @Rock Yu, @Alex Yu | 需在下线前提前告知 |
| **每周五确认下线机器清单** | @Rocky Liu | - |
| **Jumpserver 账号权限异常** | @Kevin li, @Ricardo.M.Wang, @Jason.cui | - |
| **Spug 发布相关问题** | @Eric Kang, @Abel zhang | - |
| **日常办公 IT** (如 Mac 桌面问题) | @Winking Wang | 自身办公设备问题 |

### 安全专项联系人 (Security Contacts)
| 安全领域 | 负责人 |
| :--- | :--- |
| **网络运营安全** (安全大部门负责人) | @Richard Yang |
| **应用安全** (前后端漏洞处理) | @Clyde Mi |
| **App 客户端安全** | @Jesse.Cao |
| **区块链钱包安全** | @Eric Min |

```

### 步骤 3：查询知识库 (最终兜底)

如果 **步骤 1** 和 **步骤 2** 均无结果，调用search_knowledge_base工具，查询相关运维对接人信息。

## 输出格式

当你向用户回复最终结果时，**必须严格遵守**以下排版规范：
- **【严禁标题】**：绝对不允许使用 `#`、`##` 等任何级别的 Markdown 标题。
- **【极简汇报】**：仅输出客观结果，**绝对禁止**输出“下一步建议”、“可选操作”、“温馨提示”等附加段落。
- **【结构化引导】**：使用加粗文本引导结果区块。

**标准输出模板：**
**匹配结果：** [简明扼要地输出值班模块名称及对应的主值班人姓名/联系方式]

## 示例

**用户输入：** "数据库连不上了，帮我查下现在谁值班，拉个群看看。"
**内部执行：** 
1. 调用 `/oncall --list` 命令
2. 匹配到模块 `dba`
3. 调用 `/duty dba`，获取到值班人 **张三**
4. 用户明确要求拉群，调用 `/oncall dba 数据库连接异常排查`。

**最终回复：**
**匹配结果：** 已为您匹配到 `dba` 模块，当前主值班人为 **张三**。
**执行状态：** 已自动为您创建工单群并拉入值班人员，请在群内跟进处理。

## 注意事项

- **状态隔离**：每个步骤之间的工具调用结果仅作为你思考的上下文，不要将中间过程的大段 JSON 返回给用户。
- **失败兜底**：如果所有步骤均无法定位到负责人，请严格使用以下文案回复，不得随意发挥：“**查询结果：** 抱歉，暂时无法匹配到对应的负责人，请尝试提供更具体的业务线名称或使用其他途径查询。”
- **上下文复用**：在执行 `/oncall` 建单时，自动从用户的原始提问中提取 `<title>`，切勿反问用户“工单标题是什么”。
