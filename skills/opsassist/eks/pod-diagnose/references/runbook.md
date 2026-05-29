# EKS Pod 异常根因速查表

数据来源：生产环境 ONCALL 工单（1011 条，2026-05-20 统计）

## 根因分类

| # | 根因 | 关键判断依据 | 责任方 | 自动创建 oncall |
|---|---|---|---|---|
| 1 | 镜像拉取失败（ECR 403） | ImagePullBackOff + 403 / unauthorized | 运维 | ✅ |
| 2 | 镜像 tag 不存在 | manifest unknown / not found | 研发/CI | ❌ |
| 3 | 探针端口不匹配 | Liveness probe failed + 端口差异 | 研发 | ❌ |
| 4 | 节点资源不足 | Insufficient cpu/memory，无 NotTriggerScaleUp | 运维 | ✅ |
| 5 | IP 池耗尽 | Failed to allocate address | 运维 | ✅ |
| 6 | 节点亲和性配置错误 | node affinity 不匹配 + NotTriggerScaleUp | 研发（发布参数） | ❌ |
| 7 | OOMKilled | Exit Code 137 | 研发/运维协作 | ❌（先研发自查） |
| 8 | 应用 panic/crash | Exit Code 0 + panic/fatal/SIGSEGV | 研发 | ❌ |
| 9 | 中间件连接失败 | Redis/ZK/DB/Kafka timeout / Connection refused | 研发确认配置 + 运维查安全组 | ❌ |
| 10 | SGP 安全组不一致 | 5000034 错误码 / SecurityGroupPolicy | 运维 | ✅ |
| 11 | Java 模块化问题 | InaccessibleObjectException | 研发（加 --add-opens） | ❌ |
| 12 | 宿主机故障 | NodeNotReady / Evicted | 运维 | ✅ |
| 13 | Terminating 卡死 | 宿主机故障 / Finalizer 未清理 | 运维 | ✅ |
| 14 | Deployment 残留（转 STS 后） | 两种控制器并存 | 运维 | ✅ |

## NotTriggerScaleUp 判断说明

`didn't match node affinity` 出现时，必须通过 `NotTriggerScaleUp` 信号区分：

- **有 NotTriggerScaleUp**：节点组标签与 affinity 根本不匹配，CA 扩容无效 → 研发问题，去 Spug/旗鱼 检查「项目」和「出网方式」参数
- **无 NotTriggerScaleUp，只有 Insufficient cpu/memory**：节点组存在但资源耗尽 → 运维问题，创建 oncall 扩容

## 典型案例参考

### CrashLoop — Exit Code 0 + panic
- 工单 #3686：`bk-ms-swap-task-go` 重启，日志 `panic: decimal division by 0`
- 判断：业务代码除零，研发修复

### Pending — node affinity 不匹配
- 工单 #3420：出网方式参数选错导致 Pod 调度失败，Events 有 NotTriggerScaleUp
- 判断：发布配置问题，研发去旗鱼检查出网方式

### SGP 不一致
- 工单 #3993：YAML 声明 SG 与集群运行时不一致，报错码 5000034
- 判断：运维手动更新 SecurityGroupPolicy，再触发重新发布

### ECR 403
- 工单 #3960：新账号节点组 IAM Role 缺少 ECR 权限
- 判断：运维给节点组 Role 加 AmazonEC2ContainerRegistryReadOnly 策略
