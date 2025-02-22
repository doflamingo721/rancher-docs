---
title: Backing up Rancher Installed with Docker
shortTitle: Docker Installs
weight: 3
aliases:
  - /rancher/v2.0-v2.4/en/installation/after-installation/single-node-backup-and-restoration/
  - /rancher/v2.0-v2.4/en/installation/after-installation/single-node-backup-and-restoration/
  - /rancher/v2.0-v2.4/en/backups/backups/single-node-backups/
  - /rancher/v2.0-v2.4/en/backups/legacy/backup/single-node-backups/
  - /rancher/v2.0-v2.4/en/backups/v2.0.x-v2.4.x/backup/docker-backups
  - /rancher/v2.0-v2.4/en/installation/backups-and-restoration/single-node-backup-and-restoration/
  - /rancher/v2.x/en/backups/v2.0.x-v2.4.x/backup/docker-backups/
---


After completing your Docker installation of Rancher, we recommend creating backups of it on a regular basis. Having a recent backup will let you recover quickly from an unexpected disaster.

### How to Read Placeholders

During the creation of your backup, you'll enter a series of commands, replacing placeholders with data from your environment. These placeholders are denoted with angled brackets and all capital letters (`<EXAMPLE>`). Here's an example of a command with a placeholder:

```
docker run  \
  --volumes-from rancher-data-<DATE> \
  -v $PWD:/backup busybox tar pzcvf /backup/rancher-data-backup-<RANCHER_VERSION>-<DATE>.tar.gz /var/lib/rancher
```

In this command, `<DATE>` is a placeholder for the date that the data container and backup were created. `9-27-18` for example.

### Obtaining Placeholder Data

Get the placeholder data by running:

```
docker ps
```

Write down or copy this information before starting the [procedure below](#creating-a-backup).

<sup>Terminal <code>docker ps</code> Command, Displaying Where to Find <code>&lt;RANCHER_CONTAINER_TAG&gt;</code> and <code>&lt;RANCHER_CONTAINER_NAME&gt;</code></sup>

![Placeholder Reference](/img/placeholder-ref.png)

| Placeholder                | Example                    | Description                                               |
| -------------------------- | -------------------------- | --------------------------------------------------------- |
| `<RANCHER_CONTAINER_TAG>`  | `v2.0.5`                   | The rancher/rancher image you pulled for initial install. |
| `<RANCHER_CONTAINER_NAME>` | `festive_mestorf`          | The name of your Rancher container.                       |
| `<RANCHER_VERSION>`        | `v2.0.5`                   | The version of Rancher that you're creating a backup for. |
| `<DATE>`                   | `9-27-18`                  | The date that the data container or backup was created.   |
<br/>

You can obtain `<RANCHER_CONTAINER_TAG>` and `<RANCHER_CONTAINER_NAME>` by logging into your Rancher Server by remote connection and entering the command to view the containers that are running: `docker ps`. You can also view containers that are stopped with `docker ps -a`. Use these commands for help anytime while creating backups.

### Creating a Backup

This procedure creates a backup that you can restore if Rancher encounters a disaster scenario.


1. Using a remote Terminal connection, log into the node running your Rancher Server.

1. Stop the container currently running Rancher Server. Replace `<RANCHER_CONTAINER_NAME>` with the [name of your Rancher container](#how-to-read-placeholders).

    ```
    docker stop <RANCHER_CONTAINER_NAME>
    ```
1. <a id="backup"></a>Use the command below, replacing each placeholder, to create a data container from the Rancher container that you just stopped.

    ```
    docker create --volumes-from <RANCHER_CONTAINER_NAME> --name rancher-data-<DATE> rancher/rancher:<RANCHER_CONTAINER_TAG>
    ```

1. <a id="tarball"></a>From the data container that you just created (<code>rancher-data-&lt;DATE&gt;</code>), create a backup tarball (<code>rancher-data-backup-&lt;RANCHER_VERSION&gt;-&lt;DATE&gt;.tar.gz</code>). Use the following command, replacing each placeholder.

    ```
    docker run  --volumes-from rancher-data-<DATE> -v $PWD:/backup:z busybox tar pzcvf /backup/rancher-data-backup-<RANCHER_VERSION>-<DATE>.tar.gz /var/lib/rancher
    ```

    **Step Result:** A stream of commands runs on the screen.

1. Enter the `ls` command to confirm that the backup tarball was created. It will have a name similar to `rancher-data-backup-<RANCHER_VERSION>-<DATE>.tar.gz`.

1. Move your backup tarball to a safe location external to your Rancher Server. Then delete the `rancher-data-<DATE>` container from your Rancher Server.

1. Restart Rancher Server. Replace `<RANCHER_CONTAINER_NAME>` with the name of your Rancher container.

    ```
    docker start <RANCHER_CONTAINER_NAME>
    ```

**Result:** A backup tarball of your Rancher Server data is created. See [Restoring Backups: Docker Installs](./restore-docker-installed-rancher.md) if you need to restore backup data.
