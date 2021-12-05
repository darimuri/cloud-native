gcloud cheat sheet
---

login
```
gcloud auth login
```

set project
```
gcloud config set project n3r-app-323802
```

[check workload identity enabled (and update if not)](https://cloud.google.com/anthos/multicluster-management/connect/prerequisites#enable_wi)
```
$ gcloud container clusters describe https://container.googleapis.com/v1/projects/n3r-app-323802/locations/us-central1-c/clusters/fleet-cluster-1 --format="value(workloadIdentityConfig.workloadPool)"

$ gcloud container clusters update https://container.googleapis.com/v1/projects/n3r-app-323802/locations/us-central1-c/clusters/fleet-cluster-1 --workload-pool=n3r-app-323802.svc.id.goog
Updating fleet-cluster-1...done.                                                                                                                                       
Updated [https://container.googleapis.com/v1/projects/n3r-app-323802/zones/us-central1-c/clusters/fleet-cluster-1].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/us-central1-c/fleet-cluster-1?project=n3r-app-323802

$ gcloud container clusters describe https://container.googleapis.com/v1/projects/n3r-app-323802/locations/us-east1-b/clusters/fleet-cluster-2 --format="value(workloadIdentityConfig.workloadPool)"

$ gcloud container clusters update https://container.googleapis.com/v1/projects/n3r-app-323802/locations/us-east1-b/clusters/fleet-cluster-2 --workload-pool=n3r-app-323802.svc.id.goog
Updating fleet-cluster-2...done.                                                                                                                                                     
Updated [https://container.googleapis.com/v1/projects/n3r-app-323802/zones/us-east1-b/clusters/fleet-cluster-2].
To inspect the contents of your cluster, go to: https://console.cloud.google.com/kubernetes/workload_/gcloud/us-east1-b/fleet-cluster-2?project=n3r-app-323802

```

[create cluster node-pool](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#enable_on_cluster)
```
$ gcloud container node-pools create pool-1 --zone=us-central1-c --cluster=fleet-cluster-1 --workload-metadata=GKE_METADATA
Creating node pool pool-1...done.                                                                                                                                                                              
Created [https://container.googleapis.com/v1/projects/n3r-app-323802/zones/us-central1-c/clusters/fleet-cluster-1/nodePools/pool-1].
NAME    MACHINE_TYPE  DISK_SIZE_GB  NODE_VERSION
pool-1  e2-medium     100           1.21.5-gke.1302

$ gcloud container node-pools create pool-1 --zone=us-east1-b --cluster=fleet-cluster-2 --workload-metadata=GKE_METADATA
Creating node pool pool-1...done.                                                                                                                                                                              
Created [https://container.googleapis.com/v1/projects/n3r-app-323802/zones/us-east1-b/clusters/fleet-cluster-2/nodePools/pool-1].
NAME    MACHINE_TYPE  DISK_SIZE_GB  NODE_VERSION
pool-1  e2-medium     100           1.21.5-gke.1302

```

[register cluster to fleet](https://cloud.google.com/anthos/multicluster-management/connect/registering-a-cluster#register_cluster)
```
$ gcloud projects get-iam-policy n3r-app-323802 | grep 'gkehub.iam.gserviceaccount.com' -A 1 -B 1
- members:
  - serviceAccount:service-154953831880@gcp-sa-gkehub.iam.gserviceaccount.com
  role: roles/gkehub.serviceAgent


$ gcloud container hub memberships register fleet-test1 --enable-workload-identity \
--gke-uri=https://container.googleapis.com/v1/projects/n3r-app-323802/locations/us-central1-c/clusters/fleet-cluster-1
Waiting for membership to be created...done.                                                                                                                                                                   
Created a new membership [projects/n3r-app-323802/locations/global/memberships/fleet-test1] for the cluster [fleet-test1]
Generating the Connect Agent manifest...
Deploying the Connect Agent on cluster [fleet-test1] in namespace [gke-connect]...
Deployed the Connect Agent on cluster [fleet-test1] in namespace [gke-connect].
Finished registering the cluster [fleet-test1] with the Hub.


$ gcloud container hub memberships register fleet-test2 --enable-workload-identity \
--gke-uri=https://container.googleapis.com/v1/projects/n3r-app-323802/locations/us-east1-b/clusters/fleet-cluster-2
Waiting for membership to be created...done.                                                                                                                                                                   
Created a new membership [projects/n3r-app-323802/locations/global/memberships/fleet-test2] for the cluster [fleet-test2]
Generating the Connect Agent manifest...
Deploying the Connect Agent on cluster [fleet-test2] in namespace [gke-connect]...
Deployed the Connect Agent on cluster [fleet-test2] in namespace [gke-connect].
Finished registering the cluster [fleet-test2] with the Hub.

```

