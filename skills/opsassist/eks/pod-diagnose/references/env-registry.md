# EKS 环境注册表

> 维护方式：直接编辑此文件，PR 合入后生效。
> 用途：SKILL 根据用户输入的 env 别名解析出对应的 EKS 集群、AWS 账号。namespace 需向用户询问。
> 账号为 AWS 账号后 4 位。

## 字段说明

- **env 别名**：用户口语输入，如 `test9`、`prod`、`pre`，多个别名用 `/` 分隔
- **账号**：AWS 账号后 4 位
- **EKS 集群**：集群名
- **备注**：特殊说明

## 注册表

| env 别名 | 账号 | EKS 集群 | 备注 |
|---|---|---|---|
| **bg 业务线 — 生产** | | | |
| prod / bg-prod / production | 9478 | prd-eks-online01 | |
| pre / bg-pre / prerelease | 9478 | prd-eks-online01 | |
| saas2 | 9478 | saas2-eks-online01 | |
| stealth | 9478 | stealth-eks-online01 | |
| pluto | 9478 | pluto-eks-online | |
| **bg 业务线 — 测试** | | | |
| test1 / devtest1 | 6616 | dev-test-eks | |
| test2 / devtest2 | 6616 | dev-test-eks | |
| test6 / devtest6 | 6616 | dev-test-eks | |
| test7 / devtest7 | 6616 | dev-test-eks | |
| test8 / devtest8 | 6616 | dev-test-eks | |
| test9 / devtest9 | 6616 | dev-test-eks | |
| test10 / devtest10 | 6616 | dev-test-eks | |
| test13 / devtest13 | 6616 | dev-test-eks | |
| test14 / devtest14 | 6616 | dev-test-eks | |
| test15 / devtest15 | 6616 | dev-test-eks | |
| test20 / devtest20 | 6616 | dev-test-eks | |
| **bk 业务线 — 生产** | | | |
| bk-prod / bk-production | 2721 | bk-eks-online | |
| bk-pre / bk-prerelease | 2721 | bk-pre | |
| **bk 业务线 — 测试** | | | |
| bk-test1 / bk-devtest1 | 7637 | bk-test-02 | |
| bk-test2 / bk-devtest2 | 7637 | bk-test-02 | |
| bk-test3 / bk-devtest3 | 7637 | bk-test-02 | |
| bk-test4 / bk-devtest4 | 7637 | bk-test-02 | |
| **quant 业务线 — 生产** | | | |
| quant-prod / quant-production | 1485 | quant-eks-online | |
| koala | 1485 | koala-eks-online | |
| **quant 业务线 — 测试** | | | |
| quant-test8 | 7673 | quant-eks-test01 | |
| quant-test9 | 7673 | quant-eks-test01 | |
| **dex 业务线** | | | |
| dex-mainnet | 6966 | dex-eks-online01 | |
| dex-testnet | 6966 | dex-eks-online01 | |
| dex-test1 | 6616 | dex-eks-test01 | |
| dex-test2 | 6616 | dex-eks-test01 | |
| **data 业务线** | | | |
| data-prod | 4911 | data-alg-eks-online | |
| **SRE / DevOps** | | | |
| tfe-prod | 2871 | sre-eks-online01 | |
| tfe-test | 4951 | sre-eks-test01 | |
| sre-monitor | 6980 | monitor-eks-online01 | |

## 缺失数据处理

env 在注册表中找不到时：
1. 追问用户："请提供该环境的 AWS 账号和 EKS 集群名，或联系 SRE 补全注册表。"
2. 补全后继续诊断，不强制阻断。
