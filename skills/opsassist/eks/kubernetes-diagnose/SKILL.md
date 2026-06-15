---
name: kubernetes-diagnose
description: 服务异常诊断。Pod/服务相关异常时触发，包括：起不来、启动失败、一直重启、CrashLoop、Pending、OOMKilled、部署失败、镜像拉取失败、发布失败、容器异常、pod 挂了、服务挂了、网络不通、无法访问。自动拉取 Pod 状态和事件，分路径定位根因，无需手动操作，需要运维介入时自动创建 oncall 工单。
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

请严格按照以下步骤执行诊断。在执行过程中，**你必须优先调用系统提供的 MCP Tools 或命令来主动获取客观数据**，严禁凭空猜测。**【重要提醒：整个排障过程中，严禁调用任何以 `cmdb_` 和 `cmdb-`开头的工具】**。

### 步骤 1：信息收集与环境探测 (Context & Discovery)

1. **确认环境上下文：** 排障前必须明确故障发生的环境（如 test, prod 等）。请根据上下文自行判断，如果信息不足，请优先按**默认环境 (测试环境 test)** 处理，或通过工具探测确认环境标识。
2. **静默探测资源信息：** 如果用户提供的信息不完整（例如只说“服务挂了/网络不通”但没给 Namespace、集群名字），请**优先自行探测**，而不是直接反问用户：
   - 调用 `/pod <IP|PodName>` 命令，查看用户输入的服务名关联的所有 Pod。测试环境需要指定 `--test=true` 命令行选项。生产环境，则禁止指定 `--test`。
   - 根据 Pod 列表，获取这些 Pod 所属的集群名（clusterName）、命名空间（namespace）、服务名（serviceName）。并调用 `list-resources` 工具列出异常 Pod。
   - **针对网络异常：** 如果表现为“访问不通”，推测并确认相关的 `Service` 或 `Ingress` 名称。
3. 如果通过上述工具仍完全无法锁定目标，再向用户简短确认集群名、namespace 等必要信息。

### 步骤 2：自动初步诊断 (Automated K8sGPT Scanning)
一旦锁定了异常所在的集群名、namespace 或资源类型，立即使用 AI 诊断引擎进行全局扫描：
1. 必须调用 `analyze` 工具。
   - 传入确定的 `namespace`。
   - 必须设置 `"explain": true`，获取深度分析结果。
   - 如果用户明确指出是**网络访问不通**，务必通过 `filters` 参数锁定范围：`["Service", "Ingress", "NetworkPolicy"]`。
2. 提取 `analyze` 结果中的严重错误项。如果根因已经非常清晰（如缩进错误、镜像名拼写错误、Service 无 Endpoint），可直接跳至**步骤 4**。如果报错模糊，进入步骤 3 深度下钻。

### 步骤 3：深度钻取与交叉验证 (Deep Dive)

基于初步线索，使用基础 Kubernetes MCP 工具按需拉取底层数据，进行交叉验证：
- **排查网络连通性与路由异常 (Network Unreachable/Timeout)：**
  如果 Pod 处于 `Running` 且 `Ready` 状态但外部无法访问：
  1. 调用 `get-resource` 获取 `services`，检查 Selector 与 TargetPort 映射。
  2. 调用 `get-resource` 获取 `endpoints`，若列表为空说明 Pod 未挂载。
  3. 调用 `get-resource` 获取 `ingresses`，检查 Host 域名和 Path 路由。
- **排查应用逻辑崩溃与业务报错 (Crash/Restart/Exceptions)：**
  1. 调用 `get-unified-logs` 工具获取日志。传入 `namespace`、`podName`和 `path`，其中 `path` 固定为 `/data/logs`。如果 Pod 一直在重启，务必设置 `"previous":true` 来获取崩溃遗言。**【注意：查询日志时，请指定具体的业务容器名称，明确排除 `filebeat` 或 `vector` 等日志采集组件容器的日志。禁止调用 `get-logs` 和 `get-custom-logs` 工具】**。
- **排查调度与生命周期阻断 (Pending/Evicted)：**
  调用 `list-events` 工具. 传入 `namespace` 和 `"involvedObjectName": "<pod-name>"`，寻找 `FailedScheduling`、`FailedMount`、`BackOff` 等事件。
- **排查资源与配置异常 (OOM/Probe Failed)：**
  调用 `get-resource` 获取 `pods` 的 YAML，重点检查 `resources.limits`、探针 (Probes) 配置以及环境变量。
- **未定位具体根因时的兜底排查 (Fallback to FlashAI)：**
---
name: kubernetes-diagnose
description: 服务异常诊断。Pod/服务相关异常时触发，包括：起不来、启动失败、一直重启、CrashLoop、Pending、OOMKilled、部署失败、镜像拉取失败、发布失败、容器异常、pod 挂了、服务挂了、网络不通、无法访问。自动拉取 Pod 状态和事件，分路径定位根因，无需手动操作，需要运维介入时自动创建 oncall 工单。
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

请严格按照以下步骤执行诊断。在执行过程中，**你必须优先调用系统提供的 MCP Tools 或命令来主动获取客观数据**，严禁凭空猜测。**【重要提醒：整个排障过程中，严禁调用任何以 `cmdb_` 和 `cmdb-`开头的工具】**。

### 步骤 1：信息收集与环境探测 (Context & Discovery)

1. **确认环境上下文：** 排障前必须明确故障发生的环境（如 test, prod 等）。请根据上下文自行判断，如果信息不足，请优先按**默认环境 (测试环境 test)** 处理，或通过工具探测确认环境标识。
2. **静默探测资源信息：** 如果用户提供的信息不完整（例如只说“服务挂了/网络不通”但没给 Namespace、集群名字），请**优先自行探测**，而不是直接反问用户：
   - 调用 `/pod <IP|PodName>` 命令，查看用户输入的服务名关联的所有 Pod。测试环境需要指定 `--test=true` 命令行选项。生产环境，则禁止指定 `--test`。
   - 根据 Pod 列表，获取这些 Pod 所属的集群名（clusterName）、命名空间（namespace）、服务名（serviceName）。并调用 `list-resources` 工具列出异常 Pod。
   - **针对网络异常：** 如果表现为“访问不通”，推测并确认相关的 `Service` 或 `Ingress` 名称。
3. 如果通过上述工具仍完全无法锁定目标，再向用户简短确认集群名、namespace 等必要信息。

### 步骤 2：自动初步诊断 (Automated K8sGPT Scanning)
一旦锁定了异常所在的集群名、namespace 或资源类型，立即使用 AI 诊断引擎进行全局扫描：
1. 必须调用 `analyze` 工具。
   - 传入确定的 `namespace`。
   - 必须设置 `"explain": true`，获取深度分析结果。
   - 如果用户明确指出是**网络访问不通**，务必通过 `filters` 参数锁定范围：`["Service", "Ingress", "NetworkPolicy"]`。
2. 提取 `analyze` 结果中的严重错误项。如果根因已经非常清晰（如缩进错误、镜像名拼写错误、Service 无 Endpoint），可直接跳至**步骤 4**。如果报错模糊，进入步骤 3 深度下钻。

### 步骤 3：深度钻取与交叉验证 (Deep Dive)

基于初步线索，使用基础 Kubernetes MCP 工具按需拉取底层数据，进行交叉验证：
- **排查网络连通性与路由异常 (Network Unreachable/Timeout)：**
  如果 Pod 处于 `Running` 且 `Ready` 状态但外部无法访问：
  1. 调用 `get-resource` 获取 `services`，检查 Selector 与 TargetPort 映射。
  2. 调用 `get-resource` 获取 `endpoints`，若列表为空说明 Pod 未挂载。
  3. 调用 `get-resource` 获取 `ingresses`，检查 Host 域名和 Path 路由。
- **排查应用逻辑崩溃与业务报错 (Crash/Restart/Exceptions)：**
  1. 调用 `get-unified-logs` 工具获取日志。传入 `namespace`、`podName`和 `path`，其中 `path` 固定为 `/data/logs`。如果 Pod 一直在重启，务必设置 `"previous":true` 来获取崩溃遗言。**【注意：查询日志时，请指定具体的业务容器名称，明确排除 `filebeat` 或 `vector` 等日志采集组件容器的日志。禁止调用 `get-logs` 和 `get-custom-logs` 工具】**。
- **排查调度与生命周期阻断 (Pending/Evicted)：**
  调用 `list-events` 工具. 传入 `namespace` 和 `"involvedObjectName": "<pod-name>"`，寻找 `FailedScheduling`、`FailedMount`、`BackOff` 等事件。
- **排查资源与配置异常 (OOM/Probe Failed)：**
  调用 `get-resource` 获取 `pods` 的 YAML，重点检查 `resources.limits`、探针 (Probes) 配置以及环境变量。
- **未定位具体根因时的兜底排查 (Fallback to FlashAI)：**
  如果执行了以上所有深度排查（网络、日志、事件、配置等）后，各项指标和表现均无明显异常，且仍无法定位到具体故障根因时：
  1. 必须调用 `flashai` 工具进行智能排障。传入诊断所需的上下文信息（如 `namespace`、`podName` 或 `serviceName` 等）。
  2. 提取、提炼并整合 `flashai` 返回的分析结果，输出其汇总后的核心排障结论。

### 步骤 4：内部根因定性 (Internal Root Cause Logic)

在输出结论前，你需要**在内部逻辑中**将故障定性（注意：对用户输出时不要提“谁的责任”），以决定最终采取的修复动作：
- **研发侧动作判定：** 属于代码 Panic、启动配置错误、Service Label 填错、拉取不存在的镜像 Tag、业务内存溢出，或由 `flashai` 汇总出的业务应用层代码/配置故障。后续动作：提供修改建议。
- **运维基建侧动作判定：** 属于 DNS 解析失败、云厂商 LB 异常、安全组阻断、Node NotReady、节点资源耗尽、PVC 绑定失败，或由 `flashai` 汇总定位出的底层云基建/节点网络故障。后续动作：直接通过 `/oncall` 触发工单。

### 步骤 5：输出结论与处置方案 (Output & Remediation)

最后，向用户输出结构化的诊断报告，直接给结论。报告必须包含以下 3 个部分（使用 Markdown）：
1. **故障现象摘要：** 简述你通过工具观察到了什么（如：通过 list-events 发现 `payment-pod-xxx` 重启了 5 次；或：常规排查未见异常，已通过 `flashai` 工具联动诊断）。
2. **核心根因：** 用一句话直接点明导致问题的根本原因（如果使用了 `flashai`，请清晰阐述其汇总的排障结论）。
3. **修复建议与行动 (Action)：**
   - **需改代码/配置的问题：** 给出具体明确的修复指导（如：“请修改 deployment 中的镜像 tag 为 v1.2.3”、“检查日志中第 45 行的空指针异常”）。
   - **需运维处理的问题：** 告知用户底层异常情况，并**自动触发 Oncall 工单创建流程**（话术示例：“诊断为底层网络安全组拦截，已为您自动拉起 SRE Oncall 工单，工单号：INC-XXXX”）。

---

## 核心业务特有规则（严禁违背）
1. **Kyverno/OPA 审计规则：**
   - 遇到 `PolicyViolation` 事件，因为当前集群在 **audit 模式**，此类事件**只记录不阻断**，Pod 可以正常启动。
   - **严禁**将 `PolicyViolation` 作为异常根因，不要提示用户修改配置。继续查其他 Events 或日志找真正的根因。
2. **节点亲和性 (Node Affinity)：**
   - 报错 `didn't match node affinity/selector` 由**运维**维护。直接创工单找运维处理，**严禁引导研发去改发布配置**。
   - 同时出现 `NotTriggerScaleUp`：说明节点组标签与 affinity  完全不匹配，需运维检查标签。
   - 只有 `Insufficient cpu/memory`：节点组存在但资源耗尽，需运维扩容。
3. **内部网络调用：**
   - 服务器间内网调用不能用 `*.tools` 域名，应指出并建议使用 `bx-internal.com` 或 `svc.cluster.local`。
4. **资源术语映射 (Terminology Mapping)：**
   - 当用户口头表达“查询 XXXX 服务”或“XXXX 服务异常”时，在 Kubernetes 语境下，**通常不仅仅指代 `Service` 资源，更等同于查询名为 XXXX 的 `Deployment` 或 `StatefulSet` 工作负载 (Workload)**。在调用工具排障时，请务必自动关联查询对应的底层工作负载状态。

## 需要运维介入时自动创建工单

判断为底层基础设施/运维问题时，**直接执行创建，无需用户确认**（需要 serviceName 和 env，若未获取到则直接追问）：

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

## 【严格行为约束】
1. **不做归责：** 绝对不说“责任方是谁”、不做定性归责，不输出“属于研发问题/运维问题”字样，只聚焦解决问题和分发工单。
2. **禁止废话：** 不输出过渡语（如“根据您的信息…”、“综上所述…”、“核心矛盾在于…”），不重述用户说过的内容，不做原因的冗长解释，直接给结论和操作。
3. **执行闭环：** 绝不说“建议联系运维”后就结束，运维问题必须直接通过 `/oncall` 创工单。不需要运维的问题不创工单。
4. **禁用 CMDB 工具：** 严禁调用任何以 `cmdb_` 开头的工具。排障过程只能依赖 Kubernetes 原生或文中指定的排障 MCP 工具。
5. **禁用 kubectl 工具：** 在排障时禁止调用 `kubectl` 命令，如果要获取 kubernetes 集群信息请使用以下本 SKILL 允许的工具：`get-unified-logs`、`get-resource`、`list-events`、`list-namespaces`、`list-resources`、`list-integrations`、`analyze`、`flashai`。如果执行了以上所有深度排查（网络、日志、事件、配置等）后，各项指标和表现均无明显异常，且仍无法定位到具体故障根因时：
  1. 必须调用 `flashai` 工具进行智能排障。传入诊断所需的上下文信息（如 `namespace`、`podName` 或 `serviceName` 等）。
  2. 提取、提炼并整合 `flashai` 返回的分析结果，输出其汇总后的核心排障结论。

### 步骤 4：内部根因定性 (Internal Root Cause Logic)

在输出结论前，你需要**在内部逻辑中**将故障定性（注意：对用户输出时不要提“谁的责任”），以决定最终采取的修复动作：
- **研发侧动作判定：** 属于代码 Panic、启动配置错误、Service Label 填错、拉取不存在的镜像 Tag、业务内存溢出，或由 `flashai` 汇总出的业务应用层代码/配置故障。后续动作：提供修改建议。
- **运维基建侧动作判定：** 属于 DNS 解析失败、云厂商 LB 异常、安全组阻断、Node NotReady、节点资源耗尽、PVC 绑定失败，或由 `flashai` 汇总定位出的底层云基建/节点网络故障。后续动作：直接通过 `/oncall` 触发工单。

### 步骤 5：输出结论与处置方案 (Output & Remediation)

最后，向用户输出结构化的诊断报告，直接给结论。报告必须包含以下 3 个部分（使用 Markdown）：
1. **故障现象摘要：** 简述你通过工具观察到了什么（如：通过 list-events 发现 `payment-pod-xxx` 重启了 5 次；或：常规排查未见异常，已通过 `flashai` 工具联动诊断）。
2. **核心根因：** 用一句话直接点明导致问题的根本原因（如果使用了 `flashai`，请清晰阐述其汇总的排障结论）。
3. **修复建议与行动 (Action)：**
   - **需改代码/配置的问题：** 给出具体明确的修复指导（如：“请修改 deployment 中的镜像 tag 为 v1.2.3”、“检查日志中第 45 行的空指针异常”）。
   - **需运维处理的问题：** 告知用户底层异常情况，并**自动触发 Oncall 工单创建流程**（话术示例：“诊断为底层网络安全组拦截，已为您自动拉起 SRE Oncall 工单，工单号：INC-XXXX”）。

---

## 核心业务特有规则（严禁违背）
1. **Kyverno/OPA 审计规则：**
   - 遇到 `PolicyViolation` 事件，因为当前集群在 **audit 模式**，此类事件**只记录不阻断**，Pod 可以正常启动。
   - **严禁**将 `PolicyViolation` 作为异常根因，不要提示用户修改配置。继续查其他 Events 或日志找真正的根因。
2. **节点亲和性 (Node Affinity)：**
   - 报错 `didn't match node affinity/selector` 由**运维**维护。直接创工单找运维处理，**严禁引导研发去改发布配置**。
   - 同时出现 `NotTriggerScaleUp`：说明节点组标签与 affinity 完全不匹配，需运维检查标签。
   - 只有 `Insufficient cpu/memory`：节点组存在但资源耗尽，需运维扩容。
3. **内部网络调用：**
   - 服务器间内网调用不能用 `*.tools` 域名，应指出并建议使用 `bx-internal.com` 或 `svc.cluster.local`。
4. **资源术语映射 (Terminology Mapping)：**
   - 当用户口头表达“查询 XXXX 服务”或“XXXX 服务异常”时，在 Kubernetes 语境下，**通常不仅仅指代 `Service` 资源，更等同于查询名为 XXXX 的 `Deployment` 或 `StatefulSet` 工作负载 (Workload)**。在调用工具排障时，请务必自动关联查询对应的底层工作负载状态。

## 需要运维介入时自动创建工单

判断为底层基础设施/运维问题时，**直接执行创建，无需用户确认**（需要 serviceName 和 env，若未获取到则直接追问）：

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

## 【严格行为约束】
1. **不做归责：** 绝对不说“责任方是谁”、不做定性归责，不输出“属于研发问题/运维问题”字样，只聚焦解决问题和分发工单。
2. **禁止废话：** 不输出过渡语（如“根据您的信息…”、“综上所述…”、“核心矛盾在于…”），不重述用户说过的内容，不做原因的冗长解释，直接给结论和操作。
3. **执行闭环：** 绝不说“建议联系运维”后就结束，运维问题必须直接通过 `/oncall` 创工单。不需要运维的问题不创工单。
4. **禁用 CMDB 工具：** 严禁调用任何以 `cmdb_` 开头的工具。排障过程只能依赖 Kubernetes 原生或文中指定的排障 MCP 工具。
5. **禁用 kubectl 工具：** 在排障时禁止调用 `kubectl` 命令，如果要获取 kubernetes 集群信息请使用以下本 SKILL 允许的工具：`get-unified-logs`、`get-resource`、`list-events`、`list-namespaces`、`list-resources`、`list-integrations`、`analyze`、`flashai`。
