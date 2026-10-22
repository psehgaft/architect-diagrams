# Azure Red Hat OpenShift networking

Editable diagrams.net (draw.io) recreation of the supplied Azure Red Hat OpenShift networking diagram, using the Azure icon set bundled with diagrams.net.

## Files

- [aro-networking.drawio](./aro-networking.drawio) — editable source diagram.

## Open and edit

1. Open [diagrams.net](https://app.diagrams.net/).
2. Select **Open Existing Diagram** and choose `aro-networking.drawio`, or drag the file onto the canvas.
3. Edit labels, placement, and connectors as needed.
4. Save the file as XML. To export a preview, use **File → Export as → PNG** or **SVG**.

The Azure service icons are referenced from the built-in diagrams.net Azure 2 icon library (`img/lib/azure2/`), so the diagram remains compact and editable without separate image files.

## What the diagram shows

- An Azure Red Hat OpenShift cluster within a virtual network, with separate control plane and worker subnets.
- Three control plane VMs and worker VM examples.
- The ARO-managed `aro-internal` load balancer for internal API and service traffic, and the `aro` public load balancer for public routes, a public API when enabled, and documented outbound behavior.
- `aro-pls` Private Link Service for Microsoft and Red Hat site reliability engineering management access.
- Public and private DNS records for `*.apps` and `api`, according to the selected ingress and API visibility.
- The ARO network security group (`aro-nsg`), shown with TCP 6443 called out.
- A private endpoint for the Microsoft-managed Azure Container Registry used for platform images.
- Optional virtual network peering and Azure ExpressRoute connectivity.
- OVN-Kubernetes overlay and high-level CIDR planning notes.

## Important planning notes

This is a conceptual architecture view, not a deployment template. Microsoft documents that control plane and worker nodes use different subnets, each at least `/27`; the default pod CIDR is `10.128.0.0/14` and the default service CIDR is `172.30.0.0/16`. Confirm that VNet, pod, service, and connected network ranges do not overlap. Pod CIDRs should be at least `/18`; each node receives a fixed `/23` allocation for pods.

Ingress and API visibility can be selected independently. Public routes normally use a public Standard Load Balancer and private routes normally use an internal load balancer. For private API visibility, the API address is reachable from connected networks such as peered VNets. OVN-Kubernetes provides the cluster overlay and built-in NetworkPolicy enforcement.

The diagram does not imply that users can modify ARO-managed network security groups, use the Microsoft-managed registry as their own image registry, or change Azure's documented outbound load-balancer rules.

## References

1. Microsoft Learn, [Network concepts for Azure Red Hat OpenShift](https://learn.microsoft.com/en-us/azure/openshift/concepts-networking) — primary reference for components, visibility, subnets, CIDRs, NSGs, Private Link, registry connectivity, and OVN-Kubernetes.
2. Red Hat, [Azure Red Hat OpenShift networking documentation](https://docs.redhat.com/en/documentation/azure_red_hat_openshift/4/html/networking/) — OpenShift networking background.
3. diagrams.net, [Azure 2 icon library source](https://github.com/jgraph/drawio/tree/dev/src/main/webapp/img/lib/azure2) — Azure icons used by the editable diagram.

Review the Microsoft Learn page and the documentation for the deployed ARO release before using this diagram as an implementation baseline; service details can change.
