---
name: find-root-cause
description: 服务异常诊断。Pod/服务相关异常时触发，包括：起不来、启动失败、一直重启、CrashLoop、Pending、OOMKilled、部署失败、镜像拉取失败、发布失败、容器异常、pod 挂了、服务挂了、网络不通、无法访问。自动拉取 Pod 状态和事件，分路径定位根因，区分研发问题和运维问题，需要运维介入时自动创建 oncall 工单。
---

# EKS Pod 异常诊断 (Find Root Cause)

## 什么时候使用该 SKILL (When to trigger)

当用户输入或告警系统触发以下场景时，自动激活本 SKILL：
- **服务状态异常：** 服务起不来、一直重启 (CrashLoopBackOff)、卡在 Pending 状态、OOMKilled 等。
- **发布与部署失败：** 镜像拉取失败 (ImagePullBackOff)、新版本部署后健康检查 (Readiness/Liveness) 无法通过。
- **连通性与服务发现异常：** 服务无法访问、网络超时、Connection Refused、502/504 错误、Service/Ingress 路由失效。
- **基础设施联动异常：** 存储卷 (PVC) 挂载失败、节点 (Node) 资源不足 (DiskPressure/MemoryPressure) 导致的 Pod 驱逐。
- **故障应急响应：** 生产环境突发中断、日常值班排障、历史故障复盘分析。

---

## 故障诊断标准操作流程 (SOP)

请严格按照以下步骤执行诊断。在执行过程中，**你必须优先调用系统提供的 MCP Tools 或命令来主动获取客观数据**，严禁凭空猜测。**【������重要提醒：整个排障过程中，严禁调用任何以 `cmdb_` 开头的工具】**。

### 步骤 1：信息收集与环境探测 (Context & Discovery)

如果用户提供的信息不完整（例如只说“服务挂了/网络不通”但没给 Namespace、集群名字），请**优先自行探测**，而不是直接反问用户：
1. 调用 `/pod` 命令，查看用户输入的服务名关联的所有 Pod。
2. 根据 Pod 列表，获取这些 Pod 所属的集群名（clusterName）、命名空间（namespace）、服务名（serviceName）。并调用 `list-resources` 工具，插入对应的参数，列出可能处于异常状态的 Pod。
3. **针对网络异常：** 如果故障表现为“访问不通”，除了 Pod 外，还需要推测并确认相关的 `Service` 或 `Ingress` 名称。
4. 如果通过上述工具仍无法锁定目标，再向用户确认具体的 集群名、namespace 和资源名称。

### 步骤 2：自动初步诊断 (Automated K8sGPT Scanning)
一旦锁定了异常所在的集群名、namespace 或资源类型，立即使用 AI 诊断引擎进行全局扫描：
1. 必须调用 `analyze` 工具。
   - 传入确定的 `namespace`。
   - 必须设置 `"explain": true`，获取深度分析结果。
   - 如果用户明确指出是**网络访问不通**，务必通过 `filters` 参数锁定范围：`["Service", "Ingress", "NetworkPolicy"]`。
2. 提取 `analyze` 结果中的严重错误项。如果根因已经非常清晰（如明确指出 YAML 缩进错误、镜像名拼写错误、Service 找不到对应的 Pod），可直接跳至**步骤 4**。如果报错模糊，进入步骤 3 深度下钻。

### 步骤 3：深度钻取与交叉验证 (Deep Dive)

基于步骤 2 的线索，使用基础 Kubernetes MCP 工具按需拉取底层数据，进行交叉验证：
- **排查网络连通性与路由异常 (Network Unreachable/Timeout)：**
  如果 Pod 处于 `Running` 且 `Ready` 状态但外部无法访问：
  1. 调用 `get-resource` 工具，传入 `resourceType: "services"` 和对应名称，检查 Selector 标签是否与 Pod 完全匹配，端口 (TargetPort) 映射是否与容器一致。
  2. 调用 `get-resource` 工具，传入 `resourceType: "endpoints"`。如果 Endpoints 列表为空，说明 Pod 没有被正确挂载到网络中。
  3. 调用 `get-resource` 工具，传入 `resourceType: "ingresses"`，检查 Host 域名和 Path 路由是否正确指向了该 Service。
- **排查应用逻辑崩溃 (Crash/Restart)：** 调用 `get-logs` 工具。传入 `namespace` 和 `podName`，如果 Pod 一直在重启，务必设置 `"previous": true` 来获取上一次崩溃的遗言日志，寻找 Panic、Exception、Config Error。
- **排查调度与生命周期阻断 (Pending/Evicted)：** 调用 `list-events` 工具。传入 `namespace` 和 `"involvedObjectName": "<pod-name>"`，寻找 `FailedScheduling`、`FailedMount`、`BackOff` 等关键事件信息。
- **排查资源与配置异常 (OOM/Probe Failed)：** 调用 `get-resource` 工具。传入 `resourceType: "pods"` 和 `name` 查看资源的 YAML 配置，重点检查 `resources.limits`、探针 (Probes) 配置以及环境变量。

### 步骤 4：根因定性分析 (Root Cause Classification)

根据收集到的所有客观数据，输出明确的结论，并**必须将故障严格分类**为【研发侧问题】或【运维/基建侧问题】：

- **分类为【研发侧问题】 (Dev Issue) 的标准：**
  - **代码/配置层面：** `get-logs` 发现的代码 Panic、配置文件读取失败；业务进程未监听 0.0.0.0 导致网络拒绝连接。
  - **网络声明层面：** Service 的 Label Selector 拼写错误导致 Endpoints 为空、Ingress 路由 Path 填错。
  - **构建层面：** `list-events` 发现拉取了不存在的镜像 Tag (`ErrImagePull`)。
  - **资源容量评估：** 业务内存泄漏导致 `OOMKilled`、CPU 跑满导致探针超时。
- **分类为【运维/基建侧问题】 (Ops/Infra Issue) 的标准：**
  - **网络/安全层面：** `analyze` 发现 DNS 解析失败 (CoreDNS 异常)、云厂商 ALB/ELB 状态异常、安全组 (Security Group) 或 NetworkPolicy 阻断了流量、可用 IP 池耗尽。
  - **集群/节点层面：** Node 处于 `NotReady`、节点磁盘写满。
  - **存储层面：** `list-events` 发现 EBS 卷挂载超时、PVC 处于 Pending 无法绑定。

### 步骤 5：输出结论与处置方案 (Output & Remediation)

最后，向用户输出结构化的诊断报告。报告必须包含以下 4 个部分（请使用 Markdown 格式排版）：
1. **故障现象摘要：** 简述你通过工具观察到了什么（例如：通过 list-events 发现 `payment-pod-xxx` 在过去 10 分钟内重启了 5 次；或 Endpoints 列表为空导致 502）。
2. **核心根因：** 用一句话直接点明导致问题的根本原因。
3. **责任归属：** 明确指出这是【研发侧问题】还是【运维/基建侧问题】。
4. **修复建议与行动 (Action)：**
   - **如果是研发侧问题：** 给出具体的修复指导（例如：“请修改 deployment 中的镜像 tag 为 v1.2.3”、“请检查 Service 的 selector 是否与 Pod 匹配”或“检查日志中第 45 行的空指针异常”）。
   - **如果是运维侧问题：** 给出排查建议，并**自动触发 Oncall 工单创建流程**（回复话术示例：“诊断为底层网络异常/安全组拦截，属于运维基础设施问题，已为您自动拉起 SRE Oncall工单，工单号：INC-XXXX”）。

**【严格约束】**
1. **不做归责：** 绝对不说"责任方是谁"、不做定性归责，不输出"属于研发问题/运维问题"，只聚焦解决问题。
2. **禁止废话：** 不输出过渡语（如"根据您的信息…"、"综上所述…"、"核心矛盾在于…"），不重述用户说过的内容，不做原因的冗长解释，直接给结论和操作。
3. **禁止手动：** 绝不让用户手动执行 `kubectl`，用 MCP 工具自己查。
4. **执行闭环：** 绝不说"建议联系运维"后就结束，运维问题必须直接通过 `/oncall` 创工单。不需要运维的问题不创工单。
5. **强签名制：** 每条回复的末尾**必须加** `— by find-root-cause`，无一例外，不受对话轮次影响。
6. **禁用 CMDB 工具：** 严禁调用任何以 `cmdb_` 开头的工具。排障过程只能依赖 Kubernetes 原生或文中指定的排障 MCP 工具。

## 核心业务特有规则（严禁违背）
1. **Kyverno/OPA 审计规则：**
   - 遇到 `PolicyViolation` 事件，因为当前集群在 **audit 模式**，此类事件**只记录不阻断**，Pod 可以正常启动。
   - **严禁**将 `PolicyViolation` 作为异常根因，不要提示用户修改配置。继续查其他 Events 或日志找真正的根因。
2. **节点亲和性 (Node Affinity)：**
   - 报错 `didn't match node affinity/selector` 由**运维**维护，研发无法自行修改。直接创工单找运维处理，**严禁引导研发去改发布配置**。
   - 同时出现 `NotTriggerScaleUp`：说明节点组标签与 affinity 完全不匹配，需运维检查标签。
   - 只有 `Insufficient cpu/memory`：节点组存在但资源耗尽，需运维扩容。
3. **内部网络调用：**
   - 服务器间内网调用不能用 `*.tools` 域名，应指出并建议使用 `bx-internal.com` 或 `svc.cluster.local`。

## 需要运维介入时自动创建工单

判断为底层基础设施/运维问题时，**直接执行创建，无需用户确认**（需要 serviceName 和 env，未提供则追问）：

| 根因场景 | 自动执行的命令格式 |
|---|---|
| 节点资源不足 / IP 池耗尽 / 只有 Insufficient | `/oncall eks_ec2 {service_name} [{env}] Pod Pending - 节点资源不足，需扩容` |
| node affinity/selector 不匹配 (含 NotTriggerScaleUp) | `/oncall eks_ec2 {service_name} [{env}] Pod Pending - node affinity 不匹配，需运维检查节点组标签` |
| SGP 安全组不一致（5000034） | `/oncall eks_ec2 {service_name} [{env}] SGP 不一致，需更新 SecurityGroupPolicy` |
| ECR 镜像拉取权限（403） | `/oncall eks_ec2 {service_name} [{env}] 镜像拉取失败 - 节点组 IAM Role 缺少 ECR 权限` |
| 宿主机故障 / NodeNotReady | `/oncall eks_ec2 {service_name} [{env}] Pod 卡 Terminating - 宿主机故障，需强制删除` |
| Finalizer 未清理 | `/oncall eks_ec2 {service_name} [{env}] Pod 卡 Terminating - Finalizer 未清理` |
| metrics 未采集 | `/oncall eks_ec2 {service_name} [{env}] metrics 未采集，需运维排查 Prometheus 采集配置` |
| 复杂网络/安全组阻断/云厂商 LB 异常 | `/oncall eks_ec2 {service_name} [{env}] 网络连通性异常 - 需 SRE 介入排查网络链路` |

> **注意：** P1/P2 线上故障：先创高优工单拉运维。
