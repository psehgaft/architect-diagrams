# ICA AWS architecture diagrams

Editable diagrams.net (draw.io) sources for the AWS architecture sections 6.1–6.5. The diagrams use AWS 4 icons included in diagrams.net.

## Architecture overview

The baseline deployment is a **single AWS Region spanning three Availability Zones**. Amazon EKS provides a managed regional control plane; customer worker nodes and ICA workloads run in private VPC subnets distributed across the selected AZs. The architecture has a controlled edge/ingress path (Route 53, optional CloudFront/WAF/Shield, and an Application Load Balancer or optional API Gateway integration), an application tier for ICA services, optional AI inference services, and managed data/messaging services.

The default network posture is private-by-default: only approved entry points are exposed for inbound user traffic; worker nodes, pods, and data services stay in private subnets. Use security groups, IAM roles for service accounts or EKS Pod Identity, encryption, private VPC endpoints, and controlled egress as appropriate. Optional VPC endpoints keep supported AWS service calls on private network paths. This does not remove the need to configure service authorization and data protection.

Multi-Region is an optional extension with an independent EKS cluster and VPC in each Region. Active/passive uses one serving Region and a warm standby; active/active sends traffic to multiple healthy Regions. Route 53 can implement health-checked DNS routing, but it does not by itself replicate data, activate standby capacity, preserve sessions, or perform application recovery.

Availability Zone redundancy reduces the impact of some zonal failures. It does not guarantee that the complete application has no single point of failure: workload replicas, topology spread, disruption budgets, cluster/node scaling, ALB targets, databases, cache, file storage, queues, and recovery procedures all need compatible configuration and testing.

## Diagram index

| File | Coverage | Description |
|---|---|---|
| [01-ica-aws-high-level.drawio](./01-ica-aws-high-level.drawio) | 6.1, 6.5 | Edge, ingress, VPC, EKS, ICA components, inference and data services. |
| [02-eks-multi-az-baseline.drawio](./02-eks-multi-az-baseline.drawio) | 6.2 | Three-AZ VPC layout, managed control plane, public/private subnets, CPU/GPU workers, endpoints, and data subnets. |
| [03-ica-ai-integration-flow.drawio](./03-ica-ai-integration-flow.drawio) | 6.4 | ICA Core, Assistants, Agentic Applications, Context services, Scribeflow, and managed/custom inference paths. |
| [04-aws-multi-region-active-passive.drawio](./04-aws-multi-region-active-passive.drawio) | 6.3 | Route 53 health-checked failover from primary Region to warm standby. |
| [05-aws-multi-region-active-active.drawio](./05-aws-multi-region-active-active.drawio) | 6.3 | Healthy Regions concurrently serve requests with a selected Route 53 routing policy. |

## Open and edit

1. Open [diagrams.net](https://app.diagrams.net/).
2. Choose **Open Existing Diagram** and select a `.drawio` file, or drag the file onto the canvas.
3. Update component names, Regions, AZs, endpoints, and routing decisions to match the approved design.
4. Save as XML. Export PNG or SVG from **File → Export as** when a document preview is needed.

The diagrams use AWS 4 icon stencils built into diagrams.net; no separate icon files are required.

## Design assumptions to validate

- The baseline uses three AZs, but confirm that the selected Region and required GPU instance families have capacity in each chosen AZ. GPU quotas and available instance capacity are independent constraints.
- EKS manages the regional control plane; worker nodes run in the customer's VPC. The managed control plane is drawn outside the VPC boundary to make this separation explicit.
- Select the appropriate ALB exposure and API Gateway integration. A public user-facing service may require an internet-facing ALB or CloudFront distribution, while API Gateway can also use private integrations. Keep only approved entry points reachable.
- Choose a networking strategy for private workloads: required VPC endpoints, DNS, security groups, route tables, and any NAT Gateway egress. A NAT Gateway provides outbound connectivity; it does not make an Internet destination private.
- Use topology spread constraints, replicas, readiness/liveness checks, and PodDisruptionBudgets where they match application semantics. EKS does not automatically spread every workload.
- Choose service-specific high availability and recovery for Aurora/RDS, S3, EFS, ElastiCache, SQS/SNS/EventBridge, and any external dependencies. Their regional and cross-Region behavior differs.
- For Bedrock, configure IAM permissions and model access separately from network connectivity. A Bedrock interface VPC endpoint (AWS PrivateLink) can provide private VPC-to-service API connectivity.
- For multi-Region, specify data replication/consistency, recovery ordering, health-check signals, DNS TTL/client cache effects, standby capacity, failback, and measurable RTO/RPO. Route 53 only provides the configured DNS routing behavior.
- “No single point of failure” and automatic application failover are design goals, not guarantees made by the diagram or by EKS alone.

## References

- AWS, [Amazon EKS control plane](https://docs.aws.amazon.com/eks/latest/best-practices/control-plane.html) and [EKS VPC and subnet considerations](https://docs.aws.amazon.com/eks/latest/best-practices/subnets.html).
- AWS Prescriptive Guidance, [Spread workloads across nodes and Availability Zones](https://docs.aws.amazon.com/prescriptive-guidance/latest/ha-resiliency-amazon-eks-apps/spread-workloads.html).
- Amazon Bedrock, [Use interface VPC endpoints (AWS PrivateLink)](https://docs.aws.amazon.com/bedrock/latest/userguide/vpc-interface-endpoints.html).
- Amazon Route 53, [Active-active and active-passive failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-types.html) and [Routing policies](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html).
- AWS Well-Architected, [Disaster recovery of workloads on AWS](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html).
- Amazon API Gateway, [Best practices for private APIs and private integrations](https://docs.aws.amazon.com/whitepapers/latest/best-practices-api-gateway-private-apis-integration/rest-api.html).
- diagrams.net, [AWS 4 icon palette source](https://github.com/jgraph/drawio/blob/dev/src/main/webapp/js/diagramly/sidebar/Sidebar-AWS4.js).

Use these as conceptual architecture views. Confirm AWS service limits, Region/AZ availability, endpoint behavior, security policies, and cost with the target deployment's requirements.
