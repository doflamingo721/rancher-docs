---
title: API Keys
weight: 7005
---

## API Keys and User Authentication

If you want to access your Rancher clusters, projects, or other objects using external applications, you can do so using the Rancher API. However, before your application can access the API, you must provide the app with a key used to authenticate with Rancher. You can obtain a key using the Rancher UI.

An API key is also required for using Rancher CLI.

API Keys are composed of four components:

- **Endpoint:** This is the IP address and path that other applications use to send requests to the Rancher API.
- **Access Key:** The token's username.
- **Secret Key:** The token's password. For applications that prompt you for two different strings for API authentication, you usually enter the two keys together.
- **Bearer Token:** The token username and password concatenated together. Use this string for applications that prompt you for one authentication string.

:::note

Users may opt to enable [token hashing](../about-the-api/api-tokens.md).

:::

## Creating an API Key

1. Select **User Avatar > Account & API Keys** from upper right corner.

2. Click **Create API Key**.

3. **Optional:** Enter a description for the API key and select an expiration period or a scope. We recommend setting an expiration date.

    The API key won't be valid after expiration. Shorter expiration periods are more secure.

    Expiration period will be bound by `v3/settings/auth-token-max-ttl-minutes`. If it exceeds the max-ttl, API key will be created with max-ttl as the expiration period.
    
    A scope will limit the API key so that it will only work against the Kubernetes API of the specified cluster. If the cluster is configured with an Authorized Cluster Endpoint, you will be able to use a scoped token directly against the cluster's API without proxying through the Rancher server. See [Authorized Cluster Endpoints](../../pages-for-subheaders/rancher-manager-architecture.md#4-authorized-cluster-endpoint) for more information.

4. Click **Create**.

    **Step Result:** Your API Key is created. Your API **Endpoint**, **Access Key**, **Secret Key**, and **Bearer Token** are displayed.

    Use the **Bearer Token** to authenticate with Rancher CLI.

5. Copy the information displayed to a secure location. This information is only displayed once, so if you lose your key, you'll have to make a new one.

## What's Next?

- Enter your API key information into the application that will send requests to the Rancher API.
- Learn more about the Rancher endpoints and parameters by selecting **View in API** for an object in the Rancher UI.
- API keys are used for API calls and [Rancher CLI](../../pages-for-subheaders/cli-with-rancher.md).

## Deleting API Keys

If you need to revoke an API key, delete it. You should delete API keys:

- That may have been compromised.
- That have expired.

To delete an API, select the stale key and click **Delete**.
