# CMDB Models

Use this file only to choose the likely CMDB model.
After choosing a model, always call `cmdb_schema` to confirm real fields before querying.

available models: 
business_services, service_clusters, aws_subnet, aws_vpc, aws_region, aws_account, aws_security_group,
aws_security_group_rule, aws_eni, aws_ec2_instances, aws_ebs_volumes, aws_rds_cluster, aws_rds_instance,
aws_hosted_zone, aws_hosted_zone_record, services_loadbalancer_v2, eks_service_clusters_meta, aws_eks_clusters,
aws_eks_ingress, aws_eks_nodes, aws_eks_node_groups, service_clusters_aws_subnet_rel, service_clusters_aws_vpc_rel,
service_clusters_aws_security_group_rel, service_clusters_aws_ec2_instances_rel, aws_lb_listener, aws_lb_listener_rule

detailed info(except _rel model):
- business_services: Business service metadata, ownership, repo, deployment, and config.
- service_clusters: Service-to-environment mapping, deployment context, and cluster placement.
- aws_subnet: Subnet inventory, CIDR, AZ, VPC relation, and IP capacity.
- aws_vpc: VPC inventory, CIDR ranges, account/region, and network attributes.
- aws_region: AWS region records and account association.
- aws_account: AWS account metadata, region scope, env role, and state.
- aws_security_group: Security group metadata, VPC relation, account/region, and tags.
- aws_security_group_rule: Security group ingress/egress rules, ports, CIDRs, and source references.
- aws_eni: ENI metadata, IPs, subnet/VPC placement, security groups, and instance attachment.
- aws_ec2_instances: EC2 inventory, instance type/state, IPs, network placement, AMI, and block devices.
- aws_ebs_volumes: EBS volume metadata, size/type, IOPS, encryption, and attachment info.
- aws_rds_cluster: RDS cluster metadata, engine, endpoints, backup/encryption, and VPC placement.
- aws_rds_instance: RDS instance metadata, class, engine, endpoint, storage, and cluster relation.
- aws_hosted_zone: Route53 hosted zone metadata, scope, VPC association, name servers, and account.
- aws_hosted_zone_record: Route53 DNS record data, type, value, TTL, routing policy, and zone relation.
- services_loadbalancer_v2: Load balancer metadata, ARN, DNS name, type, status, and region/account.
- eks_service_clusters_meta: EKS workload metadata, replicas, resources, ports, namespace, and workload type.
- aws_eks_clusters: EKS cluster metadata, version, endpoint, VPC config, IAM role, and access settings.
- aws_eks_ingress: EKS Ingress metadata, class, rules, TLS, annotations, and cluster/namespace relation.
- aws_eks_nodes: EKS node metadata, status, EC2 mapping, capacity, labels, taints, and addresses.
- aws_eks_node_groups: EKS node group metadata, scaling, instance types, subnets, security groups, and AMI settings.
- aws_lb_listener: Load balancer listener metadata, port/protocol, SSL policy, certificates, default actions, and LB relation.
- aws_lb_listener_rule: Listener rule metadata, priority/default flag, conditions, actions, tags, and listener relation.