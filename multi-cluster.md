from https://oss.navercorp.com/naver-container-cluster/bd/issues/369#issuecomment-6904564


- [Multi-Cluster](#multi-cluster)
  - [Simplifying multi-clusters in Kubernetes](#simplifying-multi-clusters-in-kubernetes)
    - [A partial Taxonomy](#a-partial-taxonomy)
    - [Multi-Cluster Control Plane](#multi-cluster-control-plane)
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


# Multi-Cluster

## [Simplifying multi-clusters in Kubernetes](https://www.cncf.io/blog/2021/04/12/simplifying-multi-clusters-in-kubernetes/)

### A partial Taxonomy

Multi-cluster topologies introduce primarily two classes of challenges:

1. They require a form of **synchronization between cluster control planes**.
2. They require a form of **interconnection that makes services accessible in different clusters**.

### Multi-Cluster Control Plane

1. Dedicated API Server
    > The official Kubernetes Cluster Federation (a.k.a. KubeFed) [2] represents an example of this approach, which “allows you to coordinate the configuration of multiple Kubernetes clusters from a single set of APIs in a hosting cluster” [2]. To do so, KubeFed extends the traditional Kubernetes APIs with a new semantic for expressing which clusters should be selected for a specific deployment (through “Overrides” and “Cluster Selectors”).
   * [KubeFed](https://github.com/kubernetes-sigs/kubefed#kubernetes-cluster-federation) [1.9k stars](https://github.com/kubernetes-sigs/kubefed)
2. GitOps
   > GitOps is a well-established framework to orchestrate CI/CD workflows. The basic idea is to use a git repository as a single source of truth for application deployment and update the cluster’s corresponding objects. Facing multi-cluster topologies, GitOps can represent an elementary multi-cluster control plane. We can mention GitOps tools such as FluxCD, Fleet, ArgoCD.

    > In such a scenario, the applications are templated with the correct value for the suitable clusters and then deployed on target clusters. This approach, combined with proper network interconnection tools, allows you to obtain a multi-cluster orchestration without dealing with the complexity of extra APIs.

    > However, **GitOps approaches lack dynamic pods placement across the multi-cluster topology**. They do not support any active disaster recovery strategy or cross-cluster bursting. In particular, there is no possibility to automatically migrate workloads across clusters to respond to unexpected failures or deal rapidly with unprevented load peaks.
   * [FluxCD](https://fluxcd.io/docs/)
   * [Fleet](https://rancher.com/docs/rancher/v2.6/en/deploy-across-clusters/fleet/)
   * [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)

3. Virtual-kubelet-based approaches
   > [Virtual Kubelet](https://github.com/virtual-kubelet/virtual-kubelet#virtual-kubelet) [3.3k stars](https://github.com/virtual-kubelet/virtual-kubelet) (VK) is a “Kubernetes kubelet implementation that masquerades as a kubelet to connect Kubernetes to other APIs” [3]. Initial VK implementations model a remote service as a node of the cluster used as a placeholder to introduce serverless computing in Kubernetes clusters. Later, VK has gained popularity in the multi-cluster context: a VK provider can map a remote cluster to a local cluster node. Several projects, including Admiralty, Tensile-kube, and Liqo, adopt this approach. 

    > This approach has several advantages compared to a dedicated API Server. First, it introduces multi-cluster without requiring extra APIs, and it is transparent w.r.t. applications. Second, it flexibly integrates resources of remote clusters in the scheduler’s availability: users can schedule pods in the remote cluster in the same way as they were local. Third, it enables decentralized governance. More precisely, the VK may not require privileged access on the remote cluster to schedule pods and other K8s objects supporting multiple ownerships.
   * [Admiralty](https://admiralty.io/docs/concepts/topologies) [391 stars](https://github.com/admiraltyio/admiralty)
   * [Tensile-kube](https://github.com/virtual-kubelet/tensile-kube#overview) [185 stars](https://github.com/virtual-kubelet/tensile-kube)
   * [Liqo](https://doc.liqo.io/) [464 stars](https://github.com/liqotech/liqo)
   * [More Virtual Kubelet Providers](https://github.com/virtual-kubelet/virtual-kubelet#table-of-contents)
4. more
   1. [Fleet](https://fleet.rancher.io/) from Rancher
      > Fleet can manage deployments from git of raw Kubernetes YAML, Helm charts, or Kustomize or any combination of the three. Regardless of the source, all resources are dynamically turned into Helm charts, and Helm is used as the engine to deploy everything in the cluster. This gives you a high degree of control, consistency, and auditability. Fleet focuses not only on the ability to scale, but to give one a high degree of control and visibility to exactly what is installed on the cluster.
   2. [karmada](https://github.com/karmada-io/karmada#karmada) [1.7k stars](https://github.com/karmada-io/karmada)

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
**Pricing required**

## [End to end cloud native app builds and deployments with App Platform](https://www.cncf.io/blog/2021/04/07/end-to-end-cloud-native-app-builds-and-deployments-with-app-platform/)

# Service Mesh

## [Multi-Cluster & Multi-Cloud Service Meshes With Kuma and Envoy](https://www.cncf.io/blog/2020/12/15/multi-cluster-multi-cloud-service-meshes-with-kuma-and-envoy/)

## [Kubernetes observability with a service mesh](https://buoyant.io/2021/01/11/kubernetes-monitoring-with-a-service-mesh/)

# Network

## [Kubernetes network policies with Cilium and Linkerd](https://buoyant.io/2020/12/23/kubernetes-network-policies-with-cilium-and-linkerd/)

## [Why Linkerd doesn't use Envoy](https://linkerd.io/2020/12/03/why-linkerd-doesnt-use-envoy/)

# Etc

## [Practitioner’s guide: an introduction to Kubernetes multi-tenancy](https://www.cncf.io/blog/2021/04/29/practitioners-guide-an-introduction-to-kubernetes-multi-tenancy/)