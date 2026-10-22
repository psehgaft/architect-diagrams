# Azure Red Hat OpenShift architecture diagrams

Editable diagrams.net (draw.io) sources for the ARO deployment prompt and architecture document. The diagrams extend the original ARO networking reference with views for multicloud recovery, private networking, node scaling, DNS, GitOps, CI/CD, security, and operations.

## Diagram index

| Diagram | Document sections | Main use |
|---|---:|---|
| [ARO networking reference](./aro-networking.drawio) | 8 | Recreates the supplied Azure ARO networking reference. Use as the generic service networking view. |
| [ARO and EKS active/passive platform](./aro-multicloud-active-passive.drawio) | 1, 2, 4, 6, 16, 18 | Shows ARO active and EKS warm standby, GitOps delivery, RTO/RPO targets, and unresolved failover/data-replication decisions. |
| [ARO private networking and controlled egress](./aro-private-network-security.drawio) | 5, 8, 14, 15 | Shows private API/ingress, UDR to centralized Azure Firewall, private endpoints, and the optional NAT Gateway egress path. |
| [ARO sizing, autoscaling, and ingress](./aro-cluster-sizing-autoscaling-ingress.drawio) | 8, 17 | Shows control plane, three-zone worker MachineSets, MachineAutoscalers, ClusterAutoscaler, environment sizing inputs, and internal ingress. |
| [Private API DNS and hub connectivity](./aro-private-api-dns.drawio) | 9, 14 | Shows private API name resolution from a connected hub network and calls out DNS zone ownership/link validation. |
| [Argo CD GitOps architecture](./argocd-gitops-architecture.drawio) | 10, 12 | Shows AppProject boundaries, ApplicationSet generation, Kustomize overlays, and cluster reconciliation. |
| [Infrastructure CI/CD and Dev-to-Prod promotion](./cicd-infrastructure-and-promotion.drawio) | 11 | Separates infrastructure plan/apply from application image promotion and Argo CD sync. |
| [Identity, RBAC, and workload security](./security-identity-and-rbac.drawio) | 3, 5, 15 | Shows Entra ID group mapping, namespace-scoped RoleBinding, OIDC federation, and workload identity. |
| [Observability, readiness, and recovery](./observability-readiness-and-recovery.drawio) | 16–18 | Shows user workload metrics, ClusterLogForwarder, readiness gates, and recovery-test evidence. |

## Open and edit

1. Open [diagrams.net](https://app.diagrams.net/).
2. Select **Open Existing Diagram** and choose a `.drawio` file, or drag it onto the canvas.
3. Edit labels, placements, and connectors as needed.
4. Save in XML format. Use **File → Export as → PNG** or **SVG** to export a preview.

Azure service icons use diagrams.net's bundled Azure 2 library (`img/lib/azure2/`), so the files do not need separate icon downloads.

## Architecture review notes for the source document

These diagrams visualize the proposed design and flag assumptions that need confirmation before treating the source document as an implementation baseline:

1. **ARO UDR and private visibility:** For standard-architecture ARO, Microsoft documents `UserDefinedRouting` for clusters with both API server and ingress visibility set to Private. The private network diagram reflects that combination. The generic `aro-networking.drawio` remains a reference view and is not the UDR deployment target.
2. **NAT Gateway is not private egress:** A NAT Gateway enables outbound Internet connectivity using associated public IPs. A private subnet can use it, but the destination is still the public Internet. For centralized inspection, route workload subnet default routes to Azure Firewall; if NAT Gateway is used for Firewall SNAT, associate it with `AzureFirewallSubnet`. Private Link/private endpoints are the path for private access to supported Azure PaaS services. Revise the source ADR statement that “UDR with a NAT gateway guarantees egress stays completely private.”
3. **Multicloud recovery is not fully specified:** RTO/RPO values are targets. The HLD still needs to name the health-based traffic-steering service, application activation process, data replication and consistency method, backup/restore procedure, failback owner, and tested runbooks. The diagrams mark these as TBD rather than implying that ARO and EKS provide automatic cross-cloud failover.
4. **Autoscaling needs per-pool resources:** A `ClusterAutoscaler` alone does not scale worker nodes. Configure a `MachineAutoscaler` for each worker MachineSet and validate minimum capacity, maximum capacity, and the total-node limit across supported zones. The example `maxNodesTotal: 12` is shown as an input value, not a validated capacity recommendation.
5. **Verify VM sizes and quotas:** `Standard_D8s_v3` and `Standard_D16s_v3` are values copied from the source prompt. Confirm they are supported for the selected ARO version and region, and validate quota and three-zone availability before deployment.
6. **DNS zone ownership needs confirmation:** The document's Azure Private DNS link example assumes a particular zone name and resource group. Confirm whether the target ARO standard-architecture private API DNS zone is service-managed or customer-managed, who owns records, and whether a VNet link is supported for the chosen topology. The diagram intentionally leaves those details open.
7. **Section naming:** The document's “AWS Architecture” section contains an Azure Private DNS resource and ARO API DNS requirement. The DNS diagram flags this; move it under Azure/ARO networking unless a separate AWS DNS integration is intended.
8. **Namespace naming:** OpenShift already uses the `openshift` namespace for platform components. Deploy application workloads into dedicated team/application namespaces and put ResourceQuota, LimitRange, and RBAC there.
9. **Private-cluster runner reachability:** GitHub-hosted runners may not be able to access a private ARO API. Use a controlled runner with private network connectivity or an approved private network integration.

## References

- Microsoft Learn, [Network concepts for Azure Red Hat OpenShift](https://learn.microsoft.com/en-us/azure/openshift/concepts-networking).
- Microsoft Learn, [Create a private Azure Red Hat OpenShift cluster](https://learn.microsoft.com/en-us/azure/openshift/howto-create-private-cluster-4x).
- Microsoft Learn, [Azure Red Hat OpenShift standard-architecture service definitions](https://learn.microsoft.com/en-us/azure/openshift/openshift-service-definitions).
- Microsoft Learn, [Control outbound traffic with Azure Firewall](https://learn.microsoft.com/en-us/azure/networking/design-guide/outbound-egress) and [Azure NAT Gateway overview](https://learn.microsoft.com/en-us/azure/nat-gateway/nat-gateway-design).
- Red Hat OpenShift 4.18, [Applying autoscaling](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html/machine_management/applying-autoscaling), [Configuring user workload monitoring](https://docs.redhat.com/en/documentation/monitoring_stack_for_red_hat_openshift/4.18/html-single/configuring_user_workload_monitoring/configuring_user_workload_monitoring), and [Ingress and load balancing](https://docs.redhat.com/en/documentation/openshift_container_platform/4.18/html-single/ingress_and_load_balancing/configuring-routes).
- Argo CD, [Projects](https://argo-cd.readthedocs.io/en/stable/user-guide/projects/) and [ApplicationSets](https://argo-cd.readthedocs.io/en/stable/user-guide/application-set/).
- GitHub Docs, [OIDC in Azure](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-azure) and [OIDC in AWS](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-aws).
- diagrams.net, [Azure 2 icon library](https://github.com/jgraph/drawio/tree/dev/src/main/webapp/img/lib/azure2).

Verify release-specific behavior against the deployed ARO and OpenShift versions before implementing these conceptual diagrams.
