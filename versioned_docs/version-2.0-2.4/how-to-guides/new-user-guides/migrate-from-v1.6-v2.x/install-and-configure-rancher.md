---
title: "1. Get Started"
weight: 25
aliases:
  - /rancher/v2.x/en/v1.6-migration/get-started/
---
Get started with your migration to Rancher v2.x by installing Rancher and configuring your new Rancher environment.

## A. Install Rancher v2.x

The first step in migrating from v1.6 to v2.x is to install the Rancher v2.x Server side-by-side with your v1.6 Server, as you'll need your old install during the migration process. Due to the architecture changes between v1.6 and v2.x, there is no direct path for upgrade. You'll have to install v2.x independently and then migrate your v1.6 services to v2.x.

New for v2.x, all communication to Rancher Server is encrypted. The procedures below instruct you not only on installation of Rancher, but also creation and installation of these certificates.

Before installing v2.x, provision one host or more to function as your Rancher Server(s). You can find the requirements for these hosts in [Server Requirements](../../../pages-for-subheaders/installation-requirements.md).

After provisioning your node(s), install Rancher:

- [Docker Install](../../../pages-for-subheaders/rancher-on-a-single-node-with-docker.md)

    For development environments, Rancher can be installed on a single node using Docker. This installation procedure deploys a single Rancher container to your host.

- [Kubernetes Install](../../../pages-for-subheaders/install-upgrade-on-a-kubernetes-cluster.md)

    For production environments where your user base requires constant access to your cluster, we recommend installing Rancher in a high availability Kubernetes installation. This installation procedure provisions a three-node cluster and installs Rancher on each node using a Helm chart.

    >**Important Difference:** Although you could install Rancher v1.6 in a high-availability Kubernetes configuration using an external database and a Docker command on each node, Rancher v2.x in a Kubernetes install requires an existing Kubernetes cluster. Review [Kubernetes Install](../../../pages-for-subheaders/install-upgrade-on-a-kubernetes-cluster.md) for full requirements.

## B. Configure Authentication

After your Rancher v2.x Server is installed, we recommend configuring external authentication (like Active Directory or GitHub) so that users can log into Rancher using their single sign-on. For a full list of supported authentication providers and instructions on how to configure them, see [Authentication](../../../pages-for-subheaders/about-authentication.md).

<figcaption>Rancher v2.x Authentication</figcaption>

![Rancher v2.x Authentication](/img/auth-providers.svg)

### Local Users

Although we recommend using an external authentication provider, Rancher v1.6 and v2.x both offer support for users local to Rancher. However, these users cannot be migrated from Rancher v1.6 to v2.x. If you used local users in Rancher v1.6 and want to continue this practice in v2.x, you'll need to [manually recreate these user accounts](../../../pages-for-subheaders/about-authentication.md) and assign them access rights.

As a best practice, you should use a hybrid of external _and_ local authentication. This practice provides access to Rancher should your external authentication experience an interruption, as you can still log in using a local user account. Set up a few local accounts as administrative users of Rancher.


### SAML Authentication Providers

In Rancher v1.6, we encouraged our SAML users to use Shibboleth, as it was the only SAML authentication option we offered. However, to better support their minor differences, we've added more fully tested SAML providers for v2.x: Ping Identity, Microsoft ADFS, and FreeIPA.

## C. Provision a Cluster and Project

Begin work in Rancher v2.x by using it to provision a new Kubernetes cluster, which is similar to an environment in v1.6. This cluster will host your application deployments.

A cluster and project in combined together in Rancher v2.x is equivalent to a v1.6 environment. A _cluster_ is the compute boundary (i.e., your hosts) and a _project_ is an administrative boundary (i.e., a grouping of namespaces used to assign access rights to users).

There's more basic info on provisioning clusters in the headings below, but for full information, see [Provisioning Kubernetes Clusters](../../../pages-for-subheaders/kubernetes-clusters-in-rancher-setup.md).

### Clusters

In Rancher v1.6, compute nodes were added to an _environment_. Rancher v2.x eschews the term _environment_ for _cluster_, as Kubernetes uses this term for a team of computers instead of _environment_.

Rancher v2.x lets you launch a Kubernetes cluster anywhere. Host your cluster using:

- A [hosted Kubernetes provider](../../../pages-for-subheaders/set-up-clusters-from-hosted-kubernetes-providers.md).
- A [pool of nodes from an infrastructure provider](../../../pages-for-subheaders/use-new-nodes-in-an-infra-provider.md). Rancher launches Kubernetes on the nodes.
- Any [custom node(s)](../../../pages-for-subheaders/use-existing-nodes.md). Rancher can launch Kubernetes on the nodes, be they bare metal servers, virtual machines, or cloud hosts on a less popular infrastructure provider.

### Projects

Additionally, Rancher v2.x introduces [projects](../../advanced-user-guides/manage-clusters/projects-and-namespaces.md), which are objects that divide clusters into different application groups that are useful for applying user permissions. This model of clusters and projects allow for multi-tenancy because hosts are owned by the cluster, and the cluster can be further divided into multiple projects where users can manage their apps, but not those of others.

When you create a cluster, two projects are automatically created:

- The `System` project, which includes system namespaces where important Kubernetes resources are running (like ingress controllers and cluster dns services)
- The `Default` project.

However, for production environments, we recommend [creating your own project](../../advanced-user-guides/manage-clusters/projects-and-namespaces.md#creating-projects) and giving it a descriptive name.

After provisioning a new cluster and project, you can authorize your users to access and use project resources. Similarly to Rancher v1.6 environments, Rancher v2.x allows you to [assign users to projects](../../../pages-for-subheaders/manage-projects.md). By assigning users to projects, you can limit what applications and resources a user can access.

## D. Create Stacks

In Rancher v1.6, _stacks_ were used to group together the services that belong to your application. In v2.x, you need to [create namespaces](../../advanced-user-guides/manage-clusters/projects-and-namespaces.md), which are the v2.x equivalent of stacks, for the same purpose.

In Rancher v2.x, namespaces are child objects to projects. When you create a project, a `default` namespace is added to the project, but you can create your own to parallel your stacks from v1.6.

During migration, if you don't explicitly define which namespace a service should be deployed to, it's deployed to the `default` namespace.

Just like v1.6, Rancher v2.x supports service discovery within and across namespaces (we'll get to [service discovery](discover-services.md) soon).


### [Next: Migrate Your Services](migrate-services.md)
