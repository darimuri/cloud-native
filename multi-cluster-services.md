- [Google Anthos](#google-anthos)
- [Rancher](#rancher)

[Google Anthos](https://cloud.google.com/anthos)
---
1. [Google Console](https://console.cloud.google.com/anthos)
2. [Config Management](https://cloud.google.com/anthos/config-management)
3. [Service Mesh](https://cloud.google.com/anthos/service-mesh)
4. [Anthos 보안](https://cloud.google.com/anthos/security)
5. [applications](https://cloud.google.com/kubernetes-applications)
   1. [marketplace](https://console.cloud.google.com/marketplace/browse?filter=solution-type:k8s&_ga=2.73417814.510897855.1637412101-1281303463.1637412101)
6. Introduction
   1. [Google Anthos: The First True Multi Cloud Platform?](https://cloud.netapp.com/blog/gcp-cvo-blg-google-anthos-the-first-true-multi-cloud-platform) (March 18, 2021)
   2. [Hybrid Deployment on Google Cloud: Meet Google Anthos](https://cloud.netapp.com/blog/hybrid-deployment-with-google-anthos-an-intro-gc-cvo-blg) (November 7, 2021)

[Azure Arc](https://docs.microsoft.com/ko-kr/azure/azure-arc/overview)


[Rancher](https://rancher.com/docs/rancher/v2.6/en/overview/)
---
1. [Suse Rancher](https://www.suse.com/products/suse-rancher/)
Start at local dockerd
```
$ sudo docker run --privileged -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
```
Connect to https://localhost