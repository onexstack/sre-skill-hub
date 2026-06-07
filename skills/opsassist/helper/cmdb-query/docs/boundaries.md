# CMDB Query Boundaries

CMDB is a metadata and configuration database.

Use CMDB for:

- cloud resource metadata,
- static or slow-changing configuration,
- resource inventory,
- known relationship records,
- AWS and Kubernetes infrastructure metadata.

Do not use CMDB for:

- strong real-time status,
- monitoring or alert analysis,
- incident root-cause analysis,
- pod logs,
- pod events,
- live pod status,
- live node health,
- real-time traffic path,
- cross-resource live topology inference.

## Tool Priority Rules

When the user asks for owner or responsible person:

- Prefer `alert-contact` or `domain-lead-identifier`.
- Use CMDB only if those tools cannot answer and the requested owner data is stored in CMDB metadata.

When the user asks about IP, service, or EC2:

- Prefer dedicated `ip`, `svc`, or `ec2` tools.
- Use CMDB only as fallback for metadata or relationship lookup.

When the user asks about Kubernetes troubleshooting:

- Prefer `k8sgpt` tool group or `kubernetes-diagnose` skill.
- Do not use CMDB for pod status, pod events, pod logs, or live cluster diagnosis.

When the user asks about monitoring or alerts:

- Do not use CMDB as the primary tool.
- Use monitoring, alert, or incident tools instead.

## Safe CMDB Use

CMDB can answer questions like:

- Which subnets belong to this VPC?
- Which security group rules are recorded for this security group?
- Which AWS resources are associated with this service cluster?
- Which load balancer is recorded for this domain?
- Which node groups are recorded for this EKS cluster?

CMDB should not claim real-time correctness. If needed, say that the result is based on CMDB metadata.