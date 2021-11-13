
- [Multi Clusters](#multi-clusters)
  - [Simplifying multi-clusters in Kubernetes](#simplifying-multi-clusters-in-kubernetes)
    - [A partial Taxonomy](#a-partial-taxonomy)
    - [Multi-cluster Control Plane](#multi-cluster-control-plane)
    - [Network Interconnection Tools](#network-interconnection-tools)
- [Cloud Native Applications](#cloud-native-applications)
  - [Rancher: An Open Source Platform for Cloud Native Applications](#rancher-an-open-source-platform-for-cloud-native-applications)
  - [End to end cloud native app builds and deployments with App Platform](#end-to-end-cloud-native-app-builds-and-deployments-with-app-platform)
- [Service Mesh](#service-mesh)
  - [Multi-Cluster & Multi-Cloud Service Meshes With Kuma and Envoy](#multi-cluster--multi-cloud-service-meshes-with-kuma-and-envoy)
  - [Kubernetes observability with a service mesh](#kubernetes-observability-with-a-service-mesh)
- [Network](#network)
  - [Kubernetes network policies with Cilium and Linkerd](#kubernetes-network-policies-with-cilium-and-linkerd)
  - [Why Linkerd doesn't use Envoy](#why-linkerd-doesnt-use-envoy)
- [Etc](#etc)
  - [Practitioner’s guide: an introduction to Kubernetes multi-tenancy](#practitioners-guide-an-introduction-to-kubernetes-multi-tenancy)


# Multi Clusters

## [Simplifying multi-clusters in Kubernetes](https://www.cncf.io/blog/2021/04/12/simplifying-multi-clusters-in-kubernetes/)

### A partial Taxonomy

Multi-cluster topologies introduce primarily two classes of challenges:

1. They require a form of synchronization between cluster control planes.
2. They require a form of interconnection that makes services accessible in different clusters.

### Multi-cluster Control Plane

1. Dedicated API Server
2. GitOps
3. Virtual-kubelet-based approaches

|Criteria|Liqo|Admiralty|Tensile-Kube|Kubefed|ArgoCD|Fleet|FluxCD|
| - | - | - | - | - | - | - | - |
|Seamless Scheduling|Yes|Yes|Yes|No|No|No|No|
|Support for Decentralized Governance |Yes|Yes|Yes|No|Yes|Yes|Yes|
|No Need for application extra APIs|Yes|Yes|Yes|No|No|No|No|
|Dynamic Cluster Discovery|Yes|No|No|No, using Kubefed CLI|No|No|No|


### Network Interconnection Tools

1. CNI-provided interconnection
   * [CiliumMesh](https://docs.cilium.io/en/v1.9/concepts/clustermesh/)
2. CNI-Agnostic Interconnection
   * [Submariner](https://submariner.io/)
   * [Skupper](https://skupper.io/)
3. Service Mesh
   * [ISTIO](https://istio.io/)
   * [Linkerd](https://linkerd.io/)
4. [Focus: Liqo](https://doc.liqo.io/)

|Criteria|Liqo|Cilium ClusterMesh|Submariner|Skupper|IstioMulti-Cluster|Linkerd Multi-cluster|
| - | - | - | - | - | - | - |
|Architecture|Overlay Network and Gateway|Node to Node traffic|Overlay Network and Gateway|L7 Virtual Network|Gateway-based|Gateway-based|
|Interconnection Set-Up|Peer-To-Peer, Automatic|Manual|Broker-based, Manual|Manual|Manual|Manual|
|Secure Tunnel Technology|Wireguard|No|IPSec (Strongswan, Libreswan), Wireguard|TLS|TLS|TLS|
|CNI Agnostic|Yes|No|Yes|Yes|Yes|Yes|
|Multi-cluster Services (“East-West”)|Yes|Yes|Limited|Yes|Yes, with traffic management|Yes, with traffic splitting|
|Seamless Cluster Extension|Yes|Yes|Yes|No|No|No|
|SeamlessSupport for Overlapped IPs|Yes|No|No, it relies on  global IPs networking|Yes|Yes|Yes|


# Cloud Native Applications

## [Rancher: An Open Source Platform for Cloud Native Applications](https://www.cloudops.com/blog/rancher-an-open-source-platform-for-cloud-native-applications/)

## [End to end cloud native app builds and deployments with App Platform](https://www.cncf.io/blog/2021/04/07/end-to-end-cloud-native-app-builds-and-deployments-with-app-platform/)

# Service Mesh

## [Multi-Cluster & Multi-Cloud Service Meshes With Kuma and Envoy](https://www.cncf.io/blog/2020/12/15/multi-cluster-multi-cloud-service-meshes-with-kuma-and-envoy/)

## [Kubernetes observability with a service mesh](https://buoyant.io/2021/01/11/kubernetes-monitoring-with-a-service-mesh/)

# Network

## [Kubernetes network policies with Cilium and Linkerd](https://buoyant.io/2020/12/23/kubernetes-network-policies-with-cilium-and-linkerd/)

## [Why Linkerd doesn't use Envoy](https://linkerd.io/2020/12/03/why-linkerd-doesnt-use-envoy/)

# Etc

## [Practitioner’s guide: an introduction to Kubernetes multi-tenancy](https://www.cncf.io/blog/2021/04/29/practitioners-guide-an-introduction-to-kubernetes-multi-tenancy/)