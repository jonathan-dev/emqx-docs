# Upgrade EMQX Cluster from 4.4 to 5.x


This page provides instructions on how to upgrade your EMQX cluster to the latest version.

Since EMQX 5.x is not backward compatible with version 4.x in terms of configurations, management APIs, and clustering APIs, a [rolling upgrade](./rolling-upgrades.md) from an early version is not supported. To ensure a successful upgrade, read the [Considerations Before You Start](#considerations-before-you-start) carefully before you start the upgrading procedure.

If you encounter any problems during the upgrade, contact [EMQX Support](https://www.emqx.com/en/support).

::: tip

Note that performing a fresh installation of the current version will not preserve your existing cluster configuration. You can refer to [Installation](./install.md) for a fresh installation.

:::

## Intended Audience

This instruction is intended for anyone who needs to upgrade from earlier versions of EMQX and experienced Linux system administrators familiar with data center operations.

## Considerations Before You Start

Before you start the upgrade, be aware of the requirements, incompatible changes, and potential pitfalls described in this section.

### Upgrade Version Path

To upgrade an existing EMQX Cluster to version 5.1 or later, you must currently be running a 4.4.x release.

If you are running a version earlier than 4.4.x, you must first upgrade through successive major releases until you reach version 4.4.x. For example, if you are currently running version 4.3.x, you must first upgrade to version 4.4 before proceeding to upgrade to 5.x. Refer to the [4.4 upgrade guide](https://docs.emqx.com/en/enterprise/v4.4/changes/upgrade-4.4.html#data-and-config-backup) for instructions.

### Incompatible Changes

You need to resolve all incompatibilities or conflicts with your current deployment before starting the upgrade. To identify the potential compatibility issues and breaking changes that may affect your applications and deployments, you can refer to [Incompatible Changes between EMQX 5.1 and EMQX 4.4](../changes/breaking-changes-5.1.0.md), and the following incompatible changes documents in later versions:

:::: tabs type:card

::: tab EMQX Enterprise

- [Incompatible Changes in EMQX 5.4](../changes/breaking-changes-ee-5.4.md)
- [Incompatible Changes in EMQX 5.5](../changes/breaking-changes-ee-5.5.md)
- [Incompatible Changes in EMQX 5.6](../changes/breaking-changes-ee-5.6.md)
- [Incompatible Changes in EMQX 5.7](../changes/breaking-changes-ee-5.7.md)
- [Incompatible Changes in EMQX 5.8](../changes/breaking-changes-ee-5.8.md)

:::

::: tab EMQX Open Source

- [Incompatible Changes in EMQX 5.4](../changes/breaking-changes-ce-5.4.md)
- [Incompatible Changes in EMQX 5.5](../changes/breaking-changes-ce-5.5.md)
- [Incompatible Changes in EMQX 5.6](../changes/breaking-changes-ce-5.6.md)
- [Incompatible Changes in EMQX 5.7](../changes/breaking-changes-ce-5.7.md)
- [Incompatible Changes in EMQX 5.8](../changes/breaking-changes-ce-5.8.md)

:::

::::

It is also advisable to thoroughly test your application in a staging environment before deploying the upgrade to your production environment. This will help ensure a smooth transition and minimize any potential disruptions.

#### Client Applications and Data Bridge Compatibility

Running upgraded deployments with incompatible clients and data bridge versions may result in unexpected or undefined behavior. Therefore, it is important to make sure that your client applications and data bridge backend services are compatible with the latest EMQX version. You can refer to [Data Integration Incompatibility Between EMQX 5.1 and EMQX 4.4](../changes/data-integration-4.4-to-5.1-incompatibility.md) and [Client SDKs](../connect-emqx/introduction.md) to verify the compatibility with EMQX 5.x.

### Potential Pitfalls

During the upgrade process, it is important to be aware of common potential pitfalls, including:

- Incompatibilities between the upgraded EMQX version and your client applications.
- Configuration changes that may be required to adapt to the new version.
- Dependencies on external systems or services that may need to be updated or reconfigured.

To mitigate these pitfalls, read the latest version of EMQX documentation carefully and perform thorough testing in a staging environment before applying the upgrade to your production environment. Additionally, consider engaging the support services provided by EMQX to ensure a smooth and successful upgrade.

## Upgrade EMQX Cluster

::: tip Prerequisites

- Make sure that you are running EMQX cluster release 4.4.x.
- You have read the [release notes](../changes/all-changes-ee.md).
- Your deployment environment is capable of running VMs with one of the [operating systems supported by EMQX](./install.md), or docker containers.
- You have sufficient disk space and memory available for the upgrade.
- You have reviewed any specific prerequisites mentioned in [Performance Tuning (Linux)](../performance/tune.md).

:::

:::: tabs type:card

::: tab EMQX Enterprise

1. Download EMQX `@EE_VERSION@` packages.
   - **Use a Package Manager:** Check if your operating system's package manager offers the EMQX `@EE_VERSION@` binaries. If available, use the package manager to download and install the binaries.
   - **Download Binaries Manually:** If the package manager does not provide the EMQX `@EE_VERSION@` binaries or the network of the server is restricted, you can manually download them from the [official EMQX website.](https://www.emqx.com/en/downloads-and-install/enterprise)

2. Deploy a new EMQX Cluster using the binaries. For detailed installation steps, refer to [Installation](../deploy/install.md). This ensures a clean installation of the latest version.

3. Migrate EMQX cluster. 

   - Back up the configurations and data of your EMQX 4.4 cluster using the API or Dashboard.
   - Convert the configuration files format from version 4.4 to the new format compatible with EMQX `@EE_VERSION@`.
   - Restore the migrated configuration files to the EMQX `@EE_VERSION@` cluster using the command:  `emqx ctl data import <File>`. 

   ::: tip

   A migration tool is currently in development to migrate your configuration from the existing EMQX 4.4 cluster to the new EMQX `@EE_VERSION@` cluster. This tool aims to automate the migration procedure, ensuring a smooth transition.
   
   :::

:::

::: tab EMQX Open Source

1. Download EMQX `@CE_VERSION@` packages.
   - **Use a Package Manager:** Check if your operating system's package manager offers the EMQX `@CE_VERSION@` binaries. If available, use the package manager to download and install the binaries.
   - **Download Binaries Manually:** If the package manager does not provide the EMQX `@CE_VERSION@` binaries or network of the server is restricted, you can manually download them from the [official EMQX website.](https://www.emqx.com/en/downloads-and-install/broker)

2. Deploy a new EMQX Cluster using the binaries. For detailed installation steps, refer to [Installation](../deploy/install.md). This ensures a clean installation of the latest version.

3. Migrate EMQX cluster. 

   - Back up the configurations and data of your EMQX 4.4 cluster using the API or Dashboard.
   - Convert the configuration files format from version 4.4 to the new format compatible with EMQX `@CE_VERSION@`.
   - Restore the migrated configuration files to the EMQX `@CE_VERSION@` cluster using the command:  `emqx ctl data import <File>`. 

   ::: tip

   A migration tool is currently in development to migrate your configuration from the existing EMQX 4.4 cluster to the new EMQX `@CE_VERSION@` cluster. This tool aims to automate the migration procedure, ensuring a smooth transition.
   
   :::

:::

::::

5. Verify the new EMQX Cluster thoroughly to ensure that it functions as expected. Test its connectivity, messaging capabilities, and any other relevant functionalities to confirm that the upgrade was successful.

   ::: warning Notice

   Downgrading to the previous version is not supported, so make sure that you have fully verified the new cluster before destroying the old cluster.

   :::

6. Switch your production environment to use the upgraded cluster. Update your DNS records, load balancers, or any other relevant configurations to direct traffic to the new cluster. Monitor the system closely after the switch to ensure smooth operation.
