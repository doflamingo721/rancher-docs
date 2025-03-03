---
title: Projects and Kubernetes Namespaces with Rancher
description: Rancher Projects ease the administrative burden of your cluster and support multi-tenancy. Learn to create projects and divide projects into Kubernetes namespaces
weight: 2032
aliases:
  - /rancher/v2.5/en/concepts/projects/
  - /rancher/v2.5/en/tasks/projects/
  - /rancher/v2.5/en/tasks/projects/create-project/
  - /rancher/v2.5/en/tasks/projects/create-project/
  - /rancher/v2.x/en/cluster-admin/projects-and-namespaces/
---

A namespace is a Kubernetes concept that allows a virtual cluster within a cluster, which is useful for dividing the cluster into separate "virtual clusters" that each have their own access control and resource quotas.

A project is a group of namespaces, and it is a concept introduced by Rancher. Projects allow you to manage multiple namespaces as a group and perform Kubernetes operations in them. You can use projects to support multi-tenancy, so that a team can access a project within a cluster without having access to other projects in the same cluster.

This section describes how projects and namespaces work with Rancher.

## About Namespaces

A namespace is a concept introduced by Kubernetes. According to the [official Kubernetes documentation on namespaces,](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

> Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces. [...] Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all.

Namespaces provide the following functionality:

- **Providing a scope for names:** Names of resources need to be unique within a namespace, but not across namespaces. Namespaces can not be nested inside one another and each Kubernetes resource can only be in one namespace.
- **Resource quotas:** Namespaces provide a way to divide cluster resources between multiple users.

You can assign resources at the project level so that each namespace in the project can use them. You can also bypass this inheritance by assigning resources explicitly to a namespace.

You can assign the following resources directly to namespaces:

- [Workloads](../../../pages-for-subheaders/workloads-and-pods.md)
- [Load Balancers/Ingress](../../../pages-for-subheaders/load-balancer-and-ingress-controller.md)
- [Service Discovery Records](../../new-user-guides/kubernetes-resources-setup/create-services.md)
- [Persistent Volume Claims](../../../pages-for-subheaders/create-kubernetes-persistent-storage.md)
- [Certificates](../../new-user-guides/kubernetes-resources-setup/encrypt-http-communication.md)
- [ConfigMaps](../../new-user-guides/kubernetes-resources-setup/configmaps.md)
- [Registries](../../new-user-guides/kubernetes-resources-setup/kubernetes-and-docker-registries.md)
- [Secrets](../../new-user-guides/kubernetes-resources-setup/secrets.md)

To manage permissions in a vanilla Kubernetes cluster, cluster admins configure role-based access policies for each namespace. With Rancher, user permissions are assigned on the project level instead, and permissions are automatically inherited by any namespace owned by the particular project.

For more information on creating and moving namespaces, see [Namespaces](../manage-projects/manage-namespaces.md).

### Role-based access control issues with namespaces and kubectl

Because projects are a concept introduced by Rancher, kubectl does not have the capability to restrict the creation of namespaces to a project the creator has access to.

This means that when standard users with project-scoped permissions create a namespaces with `kubectl`, it may be unusable because `kubectl` doesn't require the new namespace to be scoped within a certain project.

If your permissions are restricted to the project level, it is better to [create a namespace through Rancher](../manage-projects/manage-namespaces.md) to ensure that you will have permission to access the namespace.

If a standard user is a project owner, the user will be able to create namespaces within that project. The Rancher UI will prevent that user from creating namespaces outside the scope of the projects they have access to.

## About Projects

In terms of hierarchy:

- Clusters contain projects
- Projects contain namespaces

You can use projects to support multi-tenancy, so that a team can access a project within a cluster without having access to other projects in the same cluster.

In the base version of Kubernetes, features like role-based access rights or cluster resources are assigned to individual namespaces. A project allows you to save time by giving an individual or a team access to multiple namespaces simultaneously.

You can use projects to perform actions such as:

- Assign users to a group of namespaces (i.e., [project membership](../manage-projects/add-users-to-projects.md)).
- Assign users specific roles in a project. A role can be owner, member, read-only, or [custom](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/custom-roles.md).
- Assign resources to the project.
- Assign Pod Security Policies.

When you create a cluster, two projects are automatically created within it:

- [Default Project](#the-cluster-s-default-project)
- [System Project](#the-system-project)

### The Cluster's Default Project

When you provision a cluster with Rancher, it automatically creates a `default` project for the cluster. This is a project you can use to get started with your cluster, but you can always delete it and replace it with projects that have more descriptive names.

If you don't have a need for more than the default namespace, you also do not need more than the **Default** project in Rancher.

If you require another level of organization beyond the **Default** project, you can create more projects in Rancher to isolate namespaces, applications and resources.

### The System Project

When troubleshooting, you can view the `system` project to check if important namespaces in the Kubernetes system are working properly. This easily accessible project saves you from troubleshooting individual system namespace containers.

To open it, open the **Global** menu, and then select the `system` project for your cluster.

The `system` project:

- Is automatically created when you provision a cluster.
- Lists all namespaces that exist in `v3/settings/system-namespaces`, if they exist.
- Allows you to add more namespaces or move its namespaces to other projects.
- Cannot be deleted because it's required for cluster operations.

>**Note:** In RKE clusters where the project network isolation option is enabled, the `system` project overrides the project network isolation option so that it can communicate with other projects, collect logs, and check health.

## Project Authorization

Standard users are only authorized for project access in two situations:

- An administrator, cluster owner or cluster member explicitly adds the standard user to the project's **Members** tab.
- Standard users can access projects that they create themselves.

## Pod Security Policies

Rancher extends Kubernetes to allow the application of [Pod Security Policies](https://kubernetes.io/docs/concepts/policluster-admin/pod-security-policy/) at the [project level](../manage-projects/manage-pod-security-policies.md) in addition to the [cluster level.](./add-a-pod-security-policy.md) However, as a best practice, we recommend applying Pod Security Policies at the cluster level.

## Creating Projects

This section describes how to create a new project with a name and with optional pod security policy, members, and resource quotas.

1. [Name a new project.](#1-name-a-new-project)
2. [Optional: Select a pod security policy.](#2-optional-select-a-pod-security-policy)
3. [Recommended: Add project members.](#3-recommended-add-project-members)
4. [Optional: Add resource quotas.](#4-optional-add-resource-quotas)

### 1. Name a New Project

1. From the **Global** view, choose **Clusters** from the main menu. From the **Clusters** page, open the cluster from which you want to create a project.

1. From the main menu, choose **Projects/Namespaces**. Then click **Add Project**.

1. Enter a **Project Name**.

### 2. Optional: Select a Pod Security Policy

This option is only available if you've already created a Pod Security Policy. For instruction, see [Creating Pod Security Policies](../authentication-permissions-and-global-configuration/create-pod-security-policies.md).

Assigning a PSP to a project will:

- Override the cluster's default PSP.
- Apply the PSP to the project.
- Apply the PSP to any namespaces you add to the project later.

### 3. Recommended: Add Project Members

Use the **Members** section to provide other users with project access and roles.

By default, your user is added as the project `Owner`.

>**Notes on Permissions:**
>
>- Users assigned the `Owner` or `Member` role for a project automatically inherit the `namespace creation` role. However, this role is a [Kubernetes ClusterRole](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole), meaning its scope extends to all projects in the cluster. Therefore, users explicitly assigned the `Owner` or `Member` role for a project can create namespaces in other projects they're assigned to, even with only the `Read Only` role assigned.
>
>- By default, the Rancher role of `project-member` inherits from the `Kubernetes-edit` role, and the `project-owner` role inherits from the `Kubernetes-admin` role. As such, both `project-member` and `project-owner` roles will allow for namespace management, including the ability to create and delete namespaces.
>
>- Choose `Custom` to create a custom role on the fly: [Custom Project Roles](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/cluster-and-project-roles.md#custom-project-roles).

To add members:

1. Click **Add Member**.
1. From the **Name** combo box, search for a user or group that you want to assign project access. Note: You can only search for groups if external authentication is enabled.
1. From the **Role** drop-down, choose a role. For more information, refer to the [documentation on project roles.](../authentication-permissions-and-global-configuration/manage-role-based-access-control-rbac/cluster-and-project-roles.md)

### 4. Optional: Add Resource Quotas

Resource quotas limit the resources that a project (and its namespaces) can consume. For more information, see [Resource Quotas](../../../pages-for-subheaders/manage-project-resource-quotas.md).

To add a resource quota,

1. Click **Add Quota**.
1. Select a Resource Type. For more information, see [Resource Quotas.](../../../pages-for-subheaders/manage-project-resource-quotas.md).
1. Enter values for the **Project Limit** and the **Namespace Default Limit**.
1. **Optional:** Specify **Container Default Resource Limit**, which will be applied to every container started in the project. The parameter is recommended if you have CPU or Memory limits set by the Resource Quota. It can be overridden on per an individual namespace or a container level. For more information, see [Container Default Resource Limit](../../../pages-for-subheaders/manage-project-resource-quotas.md)
1. Click **Create**.

**Result:** Your project is created. You can view it from the cluster's **Projects/Namespaces** view.

| Field                   | Description                                                                                              |
| ----------------------- | -------------------------------------------------------------------------------------------------------- |
| Project Limit           | The overall resource limit for the project.                                                              |
| Namespace Default Limit | The default resource limit available for each namespace. This limit is propagated to each namespace in the project when created. The combined limit of all project namespaces shouldn't exceed the project limit.  |

## Switching between Clusters and Projects

To switch between clusters and projects, use the **Global** drop-down available in the main menu.

![Global Menu](/img/global-menu.png)

Alternatively, you can switch between projects and clusters using the main menu.

- To switch between clusters, open the **Global** view and select **Clusters** from the main menu. Then open a cluster.
- To switch between projects, open a cluster, and then select **Projects/Namespaces** from the main menu. Select the link for the project that you want to open.
