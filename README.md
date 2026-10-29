# architect-diagrams

A collection of architecture diagrams and their editable sources, covering
cloud and OpenShift platform designs. All diagrams are editable
[diagrams.net](https://app.diagrams.net/) (draw.io) sources, so every view can
be opened, updated, and re-exported from the same file.

> Spanish version: [README.es.md](./README.es.md) · English guide:
> [README.en.md](./README.en.md)

## Diagram collections

### AWS — ICA architecture (`AWS/`)

Editable diagrams.net sources for the ICA AWS architecture (sections 6.1–6.5),
using the AWS 4 icon set built into diagrams.net. The baseline is a single AWS
Region spanning three Availability Zones, with Amazon EKS, private VPC subnets,
and a controlled edge/ingress path. See [`AWS/README.md`](./AWS/README.md)
for the full overview, design assumptions, and references.

| File | Coverage |
| ---- | -------- |
| [`01-ica-aws-high-level.drawio`](./AWS/01-ica-aws-high-level.drawio) | 6.1, 6.5 — Edge, ingress, VPC, EKS, ICA components, inference and data services |
| [`02-eks-multi-az-baseline.drawio`](./AWS/02-eks-multi-az-baseline.drawio) | 6.2 — Three-AZ VPC layout, managed control plane, public/private subnets, CPU/GPU workers |
| [`03-ica-ai-integration-flow.drawio`](./AWS/03-ica-ai-integration-flow.drawio) | 6.4 — ICA Core, Assistants, Agentic Applications, Context services, inference paths |
| [`04-aws-multi-region-active-passive.drawio`](./AWS/04-aws-multi-region-active-passive.drawio) | 6.3 — Route 53 health-checked failover to a warm standby Region |
| [`05-aws-multi-region-active-active.drawio`](./AWS/05-aws-multi-region-active-active.drawio) | 6.3 — Healthy Regions serving traffic concurrently |

### Azure Red Hat OpenShift (`Azure-Red-Hat-OpenShift/`)

Editable diagrams.net sources for the ARO deployment architecture, extending
the ARO networking reference with views for multicloud recovery, private
networking, node scaling, DNS, GitOps, CI/CD, security, and operations. See
[`Azure-Red-Hat-OpenShift/README.md`](./Azure-Red-Hat-OpenShift/README.md)
for the diagram index and the architecture review notes.

| File | Main use |
| ---- | -------- |
| [`aro-networking.drawio`](./Azure-Red-Hat-OpenShift/aro-networking.drawio) | Generic ARO service networking reference |
| [`aro-multicloud-active-passive.drawio`](./Azure-Red-Hat-OpenShift/aro-multicloud-active-passive.drawio) | ARO active with EKS warm standby, GitOps delivery, RTO/RPO targets |
| [`aro-private-network-security.drawio`](./Azure-Red-Hat-OpenShift/aro-private-network-security.drawio) | Private API/ingress, UDR to Azure Firewall, private endpoints, NAT Gateway egress |
| [`aro-cluster-sizing-autoscaling-ingress.drawio`](./Azure-Red-Hat-OpenShift/aro-cluster-sizing-autoscaling-ingress.drawio) | Control plane, three-zone worker MachineSets, autoscalers, internal ingress |
| [`aro-private-api-dns.drawio`](./Azure-Red-Hat-OpenShift/aro-private-api-dns.drawio) | Private API name resolution from a connected hub network |
| [`argocd-gitops-architecture.drawio`](./Azure-Red-Hat-OpenShift/argocd-gitops-architecture.drawio) | Argo CD AppProjects, ApplicationSets, Kustomize overlays, reconciliation |
| [`cicd-infrastructure-and-promotion.drawio`](./Azure-Red-Hat-OpenShift/cicd-infrastructure-and-promotion.drawio) | Infrastructure plan/apply vs. application image promotion and Argo CD sync |
| [`security-identity-and-rbac.drawio`](./Azure-Red-Hat-OpenShift/security-identity-and-rbac.drawio) | Entra ID group mapping, namespace-scoped RoleBindings, OIDC, workload identity |
| [`observability-readiness-and-recovery.drawio`](./Azure-Red-Hat-OpenShift/observability-readiness-and-recovery.drawio) | Workload metrics, ClusterLogForwarder, readiness gates, recovery-test evidence |

### OpenStack (`OpenStack/`)

| File | Main use |
| ---- | -------- |
| [`cloud-factory.drawio`](./OpenStack/cloud-factory.drawio) | OpenStack cloud factory architecture |

### OpenShift Virtualization (`ocp-virtualization/`)

| File | Main use |
| ---- | -------- |
| [`OCP-Virtualization.drawio`](./ocp-virtualization/OCP-Virtualization.drawio) | OpenShift Virtualization architecture (editable source) |
| [`OCP-RHOSO.drawio`](./ocp-virtualization/OCP-RHOSO.drawio) | Red Hat OpenStack Services on OpenShift architecture (editable source) |
| [`demo-diagram.svg`](./ocp-virtualization/demo-diagram.svg) | Exported SVG preview |
| [`standalone-manager-red-hat-virtualization-architecture.svg`](./ocp-virtualization/standalone-manager-red-hat-virtualization-architecture.svg) | Exported SVG preview |

An untitled draft diagram also sits at the repository root
(`Untitled Diagram.drawio`).

## Open and edit

1. Open [diagrams.net](https://app.diagrams.net/).
2. Choose **Open Existing Diagram** and select a `.drawio` file, or drag the
   file onto the canvas.
3. Edit labels, placements, and connectors as needed.
4. Save in XML format. Use **File → Export as** to export PNG or SVG when a
   document preview is needed — prefer SVG exports for vector diffs.

Keep the editable source together with any exported file, and name folders by
system or bounded context when adding new diagrams.

## Contributing

See [`CONTRIBUTING.en.md`](./CONTRIBUTING.en.md) (English) and
[`CONTRIBUTING.es.md`](./CONTRIBUTING.es.md) (Spanish), and the pull request
template in [`PULL_REQUEST_TEMPLATE.md`](./PULL_REQUEST_TEMPLATE.md).

## Notes

These diagrams are conceptual architecture views: confirm service limits,
Region/AZ availability, quotas, endpoint behavior, security policies, and cost
against the target deployment's requirements before implementing them. There
is no license file in this repository yet.
