# CMDB Query Patterns

This document defines schema-driven query planning patterns.

Do not hardcode field names from model names. Always inspect schemas first.

## General Multi-Step Pattern

Use this pattern when the user asks for relationships between resources.

Process:

1. Identify the source resource.
2. Identify the target resource.
3. Select candidate source, relation, and target models.
4. Inspect schemas of all candidate models.
5. Find real fields that can connect the models.
6. Search the source model.
7. Extract confirmed key fields.
8. Search relation or target models using exact-match filters.
9. Return final target resources.

Example abstract path:

```text
source model -> relation model -> target model