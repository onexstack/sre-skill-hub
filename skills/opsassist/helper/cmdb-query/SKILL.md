---
name: cmdb-query
description: >
    Use this skill for CMDB-based metadata and static infrastructure relationship
    queries, including AWS/Kubernetes inventory and topology lookups such as
    VPC/subnet/security-group/EC2 relationships, domain-to-load-balancer mapping,
    service-to-network resources, EKS-to-node/network mapping, hosted-zone/DNS record
    discovery, and instance-to-EBS attachment queries.

    This skill is intended for static resource mapping and ownership of infrastructure
    relationships, not for live diagnostics.

    If the request is Kubernetes troubleshooting and can be handled by k8sgpt-related
    tools, prioritize k8sgpt first. This includes Pod startup failures, CrashLoopBackOff,
    Pending, OOMKilled, log analysis, event analysis, connectivity checks, and other
    real-time cluster investigation scenarios.

    Only use this skill for owner/contact lookup, when alert contact queries failed.
    Do not use this skill for requests already covered by dedicated tools
    such as IP, EC2, Pod, domain resolve, or service query tools. Only use when these approaches
    could not satisfy the query.
---

Before the first `cmdb_search` call, the agent must:
1. Read docs/query-patterns.md and check if the user's question matches any preset path.
2. If a preset path matches, follow it exactly — do not improvise or skip steps.
3. If no preset path matches, identify candidate models,
4. call `cmdb_schema` for all models involved in the planned path,
5. build a complete query plan,
6. then execute searches according to the plan.

Call `cmdb_schema` for multiple candidate models in parallel whenever possible.

Read docs/model.md to know available cmdb models
Read docs/model.md to know available cmdb models
Read docs/model.md to know available cmdb models

# CMDB Query Skill

This skill guides the agent to query CMDB resource metadata through schema-first planning.

CMDB is suitable for static or slow-changing resource metadata and configuration relationships. It must not be used for strong real-time status, monitoring analysis, alert diagnosis, pod logs, pod events, or live topology inference.

## Core Rule: Schema-First Query Planning

Before the first `cmdb_search` call, the agent must:

1. Understand the user's question.
2. Identify the given resource identifier and target output.
3. Select candidate CMDB models.
4. Call `cmdb_schema` for all models involved in the planned query path.
5. Build a complete query plan using only real schema fields.
6. Execute `cmdb_search` step by step.
7. Use returned values from one step as exact-match query values for the next step.

Never call `cmdb_search` with guessed field names.

## CMDB Search Contract

`cmdb_search.query` only supports exact-match filters.

Valid format:

```json
{
  "model_id": "aws_subnet",
  "query": {
    "aws_vpc_id": "vpc-xxx"
  }
}
```

## Rules:

Every query key must be an existing field in the target model schema.
Multiple query keys mean AND filtering.
Query values must be exact values.
Do not use fuzzy match, range match, contains match, regex, SQL-like expressions, or operators.
Do not invent fields such as name, id, resource_id, or arn unless confirmed by schema.
Schema Usage

cmdb_schema returns field definitions for a model.

Use schema to identify:
- valid query fields,
- primary keys,
- fields that may connect to another model,
- fields that should be extracted for the final answer.

Primary keys are usually marked by:

{
  "tag": ["primaryKey"]
}

If multiple candidate models may be involved, call cmdb_schema for them before searching.

Query Plan Format

Before searching, internally form a plan like:
- User target:
- Given identifier:
- Expected output:
- Candidate models:
- Required schemas:
- Query path:
1. model A: query by field X, extract field Y
2. model B: query by field Y, extract field Z
3. model C: query by field Z, return final fields

The plan does not need to be fully shown to the user, but the final answer should briefly mention the actual query path used.

When More Documents Are Needed
Read docs/boundaries.md when deciding whether CMDB is appropriate.