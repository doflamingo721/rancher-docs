---
title: Harvester Integration
weight: 10
---

Introduced in Rancher v2.6.1, [Harvester](https://docs.harvesterhci.io/) is an open-source hyper-converged infrastructure (HCI) software built on Kubernetes. Harvester installs on bare metal servers and provides integrated virtualization and distributed storage capabilities. Although Harvester operates using Kubernetes, it does not require users to know Kubernetes concepts, making it a more user-friendly application.

---
**_New in v2.6.3_**

Harvester is GA. Please refer to the [Harvester release notes](https://github.com/harvester/harvester/releases) for all updates.

---
### Feature Flag

The Harvester feature flag is used to manage access to the Virtualization Management (VM) page in Rancher where users can navigate directly to Harvester clusters and access the Harvester UI. The Harvester feature flag is enabled by default. Click [here](../../pages-for-subheaders/enable-experimental-features.md) for more information on feature flags in Rancher.

To navigate to the Harvester cluster, click **☰ > Virtualization Management**. From Harvester Clusters page, click one of the clusters listed to go to the single Harvester cluster view. 

* If the Harvester feature flag is enabled, Harvester clusters will be filtered out from any pages or apps (such as Fleet and the multi-cluster app) that list Kubernetes clusters.

* If the Harvester feature flag is disabled, and a Harvester cluster is imported, the Harvester cluster will be shown in the Rancher cluster list in the Cluster Management page. Harvester clusters will only be shown on the cluster list when the feature flag is off.

* With the Harvester integration, Harvester clusters can now be imported into Rancher as a cluster type `Harvester`. 

* Users may import a Harvester cluster only on the Virtualization Management page. Importing a cluster on the Cluster Management page is not supported, and a warning will advise you to return to the VM page to do so. 

### Harvester Node Driver

The [Harvester node driver](https://docs.harvesterhci.io/v0.3/rancher/node-driver/) is marked as `tech preview` on RKE and RKE2 options in Rancher. This will be the case both on the Create page and once the driver is already enabled. The node driver is available whether or not the Harvester feature flag is enabled. Note that the node driver is off by default. Users may create RKE or RKE2 clusters on Harvester only from the Cluster Management page.

Harvester allows `.ISO` images to be uploaded and displayed through the Harvester UI, but this is not supported in the Rancher UI. This is because `.ISO` images usually require additional setup that interferes with a clean deployment (without requiring user intervention), and they are not typically used in cloud environments.

Click [here](../../pages-for-subheaders/about-provisioning-drivers.md#node-drivers) for more information on node drivers in Rancher.

### Limitations

---
**Applicable to Rancher v2.6.1 and v2.6.2 only:**

- Harvester v0.3.0 doesn’t support air-gapped environment installation.
- Harvester v0.3.0 doesn’t support upgrade from v0.2.0 nor upgrade to v1.0.0.

---