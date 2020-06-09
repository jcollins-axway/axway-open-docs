---
title: API Gateway and API Manager 7.7 July 2020 Release Notes
linkTitle: API Gateway and API Manager July 2020
weight: 70
date: 2020-06-09T00:00:00.000Z
---
## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

No new features and enhancements are available in this update.

<!-- Add the new features here -->

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this update.

<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

### Filebeat v6.2.2

This update uses Filebeat v6.2.2. Before installing this update, you must delete the Filebeat folder  `/apigateway/tools/filebeat-5.2.0`. When using Filebeat, follow the [official Filebeat documentation](https://www.elastic.co/guide/en/beats/filebeat/6.6/index.html).

### OpenJDK JRE

API Gateway and API Manager 7.7 and later support OpenJDK JRE, and this update includes Zulu OpenJDK 1.8 JRE instead of Oracle JRE 1.8.

### OpenSSL and FIPS support

This update uses OpenSSL 1.1.1, as OpenSSL 1.0.2 is EOL.

OpenSSL 1.1.1 does not support FIPS, and running API Gateway in FIPS mode is not supported in this update. OpenSSL 3.0 (when available) will support FIPS, and FIPS support will be available again in a future update.

References to FIPS in the documentation will not be removed, but this does not mean that FIPS is still supported and the references should be ignored.

## Deprecated features

<!-- Add features that are deprecated here -->

As part of our software development life cycle we constantly review our API Management offering.

The following capabilities have been deprecated.

### Antivirus scanners

API Gateway already supports the industry standard Internet Content Adaption Protocol (ICAP). From the November 2020 update the following embedded antivirus scanners will be removed:

* McAfee
* Sophos
* Clam AV

Content scanning is still supported using the ICAP filter, which provides out-of-the-box integration with ICAP-capable servers provided by Symantec, McAfee, OPSWAT and others, promoting ease of deployment and operational control.

## Removed features

<!-- Add features that are removed here -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities. As part of this review, no capabilities have been removed.

## Fixed issues

This version of API Gateway and API Manager includes:

* Fixes from all 7.5.3, 7.6.2, and 7.7 service packs released prior to this version. For details of all the service pack fixes included, see the corresponding *SP Readme* attached to each service pack on [Axway Support](https://support.axway.com).
* Fixes from all 7.7 updates released prior to this version. For details of all the update fixes included, see the corresponding [release note](/docs/apim_relnotes/) for each 7.7 update.

### Fixed security vulnerabilities

| Internal ID | Case ID | CVE Identifier | Description                            |
| ----------- | ------- | -------------- | -------------------------------------- |
| RDAPI-XXXXX | XXXXXXX |                | **Issue**: TODO. **Resolution**: TODO. |

### Other fixed issues

| Internal ID | Case ID | Description                            |
| ----------- | ------- | -------------------------------------- |
| RDAPI-XXXXX | XXXXXXX | **Issue**: TODO. **Resolution**: TODO. |

## Known issues

The following are known issues for this update.

| Internal ID | Description    |
| ----------- | -------------- |
| RDAPI-XXXXX | TODO.          |

### Policy Studio help documentation

This update includes changes to the Policy Studio documentation, but the version of the documentation (Eclipse Help) shipped with Policy Studio is out of sync with the online documentation and does not include these changes.

We are working to rectify this issue in a future update. In the meantime, you can find the up to date documentation online at [Develop in Policy Studio](/docs/apim_policydev/).

## Update a classic (non-container) deployment

These instructions apply to API Gateway and API Manager classic deployments only. For container deployments, see [Update a container deployment](#update-a-container-deployment).

### Prerequisites

This update has the following prerequisites in addition to the [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

1. Shut down any Node Manager or API Gateway instances on your existing installation.
2. Back up your existing installation. You can use the `update_apigw.sh` script to take a backup of your entire API Gateway installation directory as detailed in the [API Gateway server install steps](#install-the-api-gateway-server-update), or you can manage your own backups as detailed in [API Gateway backup and disaster recovery](/docs/apim_administration/apigtw_admin/manage_operations/#api-gateway-backup-and-disaster-recovery). Ensure that you back up any customized files. You should merge updated files instead of copying them back directly to avoid any regex matching issues, whether you manage your own backups or not. For example, the following directories might contain customized files:

   ```
   webapps/apiportal/vordel/apiportal
   webapps/emc/vordel/manager/app
   webapps/emc
   system/conf/apiportal/email
   system/conf
   samples/scripts/
   tools/filebeat-VERSION-PLATFORM
   ```
3. If you have an existing Apache Cassandra installation, ensure that you back up your data (Cassandra and `kpsadmin`), and that the `JAVA_HOME` variable is set correctly in `cassandra.in.sh` and `cassandra.in.bat`.
4. Remove the old Filebeat folder `/apigateway/tools/filebeat-5.2.0`. Check any customized files to see if they are compatible with the new version. See [Filebeat](#filebeat-v6-2-2) for more information.
5. On Linux, remove existing capabilities on product binaries (which might prevent overwriting files):

   ```
   setcap -r INSTALL_DIR/apigateway/platform/bin/vshell
   ```

### FIPS mode only

If FIPS mode is enabled, you must also perform the following steps to install the update:

1. Run `togglefips --disable` to turn FIPS mode off.
2. Start the Node Manager to move the JARs.
3. Stop the Node Manager.
4. Install the API Gateway update as described in the [Installation](#installation) section.
5. Start the Node Manager.

In Policy Studio, If FIPS mode is enabled, you must also perform the following steps to install the update:

1. Open Policy Studio and select **Window > Preferences**.
2. Select the **FIPS Mode** setting.
3. Deselect the **Enable FIPS Mode in Axway Policy Studio** option and click **OK**.
4. Restart Policy Studio using the `policystudio -clean` command.

In Configuration Studio, If FIPS mode is enabled, you must also perform the following steps to install the update:

1. Open Configuration Studio and select **Window > Preferences**.
2. Select the **FIPS Mode** setting.
3. Deselect the **Enable FIPS Mode in Axway Configuration Studio** option and click **OK**.
4. Restart Configuration Studio using the `configurationstudio -clean` command.

### Installation

This section describes how to install the update on existing 7.7 installations of API Gateway or API Manager.

* If you have installed an existing version of API Manager, installing the API Gateway server update automatically also installs the updates and fixes for API Manager.
* If you have installed a licensed version of API Gateway or API Manager 7.7, you do not require a new license to install updates.

#### Install the API Gateway server update

To install the update on your existing API Gateway 7.7 server installation, perform the following steps:

1. Ensure that your existing API Gateway instance and Node Manager have been stopped.
2. Remove any previous patches from your `INSTALL_DIR/ext/lib` and `INSTALL_DIR/META-INF` directories (or the `ext/lib` directory in an API Gateway instance). These patches have already been included in this update. You do not need to copy patches from a previous version.
3. Download and unpack the API Gateway 7.7 server Update file into a new directory. For example:

   ```
   mkdir 77update
   tar -xzvf APIGateway_7.7.YYYYMMDD_Core_linux-x86-64_BNnn.tar.gz -C 77update
   ```

    {{< alert title="Note" color="primary" >}}You must extract the file into a new directory and not into the existing API Gateway installation directory.{{< /alert >}}
4. Run the [`update_apigw.sh` script](#update-apigw-sh-script) from the directory into which you extracted the Update file (for example, `77update`) and specify  your API Gateway installation directory using the `--install_dir` option. For example:

   ```
   ./update_apigw.sh --install_dir /opt/Axway-7.7/
   ```
5. Restart your Node Manager and API Gateway instances on the local machine.

##### `update_apigw.sh` script

Run the `update_apigw.sh` script with the `--help` option to see the available options:

```
./update_apigw.sh --help
```

The script generates a trace file in the `update-output/trace` directory. Use the `--tracelevel` option to change the level of tracing.

The script takes a backup of your entire API Gateway installation directory and places it in a `tar` file in the `update-output/backups` directory. Specify a different directory using the `--backup_dir` option. To manage your own backups, use the `--no_backup` option.

To run the script without user interaction, specify `--mode unattended` option.

Running the `update_apigw.sh` script performs the following steps:

1. Check that the installation directory is valid.
2. Check that the user who owns the API Gateway binaries is the same user running the `update_apigw.sh` script.
3. Check that the Node Manager and API Gateway instances that run on the local machine are not running.
4. Take a backup unless `--no_backup` has been specified.
5. Install the update content into the API Gateway installation directory.
6. Perform all of the steps that were performed by a post installation script in earlier updates. This includes JRE cleanup, `system/lib/modules` cleanup, third-party and Axway JAR cleanup, apply `acl.json` fix, apply passphrase obfuscation fix, and apply modifications to Node Manager entity store configuration. Fixes are only applied if they have not previously been applied.

#### Install the Policy Studio update

To install the update on your existing Policy Studio installation, an update script is provided. The update script is located inside the API Gateway 7.7 Policy Studio Update pack (for example, `APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz`).

Download and unpack the API Gateway 7.7 Policy Studio Update file into a new directory. For example:

```
mkdir 77update
tar -xzvf APIGateway_7.7.YYYYMMDD_PolicyStudio_linux-x86-64_BNnn.tar.gz -C 77update
```

{{< alert title="Note" color="primary" >}}You must extract the file into a new directory and not into the existing API Gateway installation directory.

You must also remove the files `libeay32.dll` and `ssleay32.dll` if they exist in the directory `INSTALL_DIR/policystudio`. {{< /alert >}}

Run the `update_policy_studio.sh` script from the directory into which you extracted the Update file (for example, `77update`) and specify your API Gateway installation directory as an argument:

```
./update_policy_studio.sh INSTALL_DIR
```

`INSTALL_DIR` is the base API Gateway 7.7 installation directory that contains the `policystudio` directory.

For example:

```
./update_policy_studio.sh /opt/Axway-7.7/
```

If you had applied any modifications to the `policystudio.ini` file, you must reapply them after upgrade.

{{< alert title="Note" color="primary" >}}You must execute the update script using the same user who installed Policy Studio.

An update script is also available for Windows. It is called `update_policy_studio.bat` and it is located in the API Gateway 7.7 Policy Studio Update pack for Windows (`.zip`).{{< /alert >}}

Running this script performs the following steps:

1. Back up your existing `INSTALL_DIR/policystudio` directory.
2. Remove old JRE versions by deleting the `INSTALL_DIR/policystudio/jre` directory.
3. Unzip and extract API Gateway 7.7 Policy Studio Update over the `policystudio` directory in your existing API Gateway 7.7 installation directory.
4. Start Policy Studio with `policystudio -clean`.

A backup of the installation is created at `INSTALL_DIR/backups/policystudio/<date_time>`.

#### Install the Configuration Studio update

To install the update on your existing Configuration Studio installation, an update script is provided. The update script is located inside the API Gateway 7.7 Configuration Studio Update pack (for example, `APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz`).

Download and unpack the API Gateway 7.7 Configuration Studio Update file into a new directory. For example:

```
mkdir 77update
tar -xzvf APIGateway_7.7.YYYYMMDD_ConfigurationStudio_linux-x86-64_BNnn.tar.gz -C 77update
```

{{< alert title="Note" color="primary" >}}You must extract the file into a new directory and not into the existing API Gateway installation directory.

You must also remove the files `libeay32.dll` and `ssleay32.dll` if they exist in the directory `INSTALL_DIR/configurationstudio`.{{< /alert >}}

Run the `update_configuration_studio.sh` script from the directory into which you extracted the Update file (for example, `77update`) and specify your API Gateway installation directory as an argument:

```
./update_configuration_studio.sh INSTALL_DIR
```

`INSTALL_DIR` is the base API Gateway 7.7 installation directory that contains the `configurationstudio` directory.

For example:

```
./update_configuration_studio.sh /opt/Axway-7.7/
```

If you had applied any modifications to the `configurationstudio.ini` file, you must reapply them after upgrade.

{{< alert title="Note" color="primary" >}}You must execute the update script using the same user who installed Configuration Studio.

An update script is also available for Windows. It is called `update_configuration_studio.bat` and it is located in the API Gateway 7.7 Configuration Studio Update pack for Windows (`.zip`).{{< /alert >}}

Running this script performs the following steps:

1. Back up your existing `INSTALL_DIR/configurationstudio` directory.
2. Remove old JRE versions by deleting the `INSTALL_DIR/configurationstudio/jre` directory.
3. Unzip and extract API Gateway 7.7 Configuration Studio Update over the `configurationstudio` directory in your existing API Gateway 7.7 installation directory.
4. Start Configuration Studio with `configurationstudio -clean`

A backup of the installation is created at `INSTALL_DIR/backups/configurationstudio/<date_time>`.

#### Install the API Gateway Analytics update

To install the update on your existing API Gateway Analytics 7.7 installation, perform the following steps:

1. Ensure that your existing API Gateway Analytics instance and Node Manager have been stopped.
2. Remove old third-party libraries by deleting the following directories:

   ```
   INSTALL_DIR/analytics/system/lib/modules
   ```
3. Remove old JRE versions by deleting the following directories:

   ```
   INSTALL_DIR/apigateway/platform/jre
   ```
4. Verify the owners of API Gateway binaries before extracting the update.

   ```
   ls -l INSTALL_DIR/analytics/posix/bin
   ```
5. Using the same user who owns the API Gateway Analytics binaries, unzip and extract API Gateway 7.7 Analytics Update over the `analytics` directory in your existing API Gateway 7.7 installation directory. For example:

   ```
   tar -xzvf APIGateway_7.7.YYYYMMDD_Analytics_linux-x86-64_BNnn.tar.gz -C /opt/Axway-7.7/analytics/
   ```
6. Change to the `analytics` directory in your installation:

   ```
   cd INSTALL_DIR/analytics
   ```
7. Run the post-install script for API Gateway Analytics.

   ```
   apigw_analytics_sp_post_install.sh
   ```

You must also install an update for your existing API Gateway 7.7 server.

### After installation

The following steps apply after installing the update.

#### API Gateway

To allow an unprivileged user to run the API Gateway on a Linux system, perform the following steps:

1. Add the following line to the `INSTALL_DIR/system/conf/jvm.xml` file:

   ```
   <VMArg name="-Djava.library.path=$VDISTDIR/$DISTRIBUTION/jre/lib/amd64/server:$VDISTDIR/$DISTRIBUTION/jre/lib/amd64:$VDISTDIR/$DISTRIBUTION/lib/engines:$VDISTDIR/ext/$DISTRIBUTION/lib:$VDISTDIR/ext/lib:$VDISTDIR/$DISTRIBUTION/jre/lib:system/lib:$VDISTDIR/$DISTRIBUTION/lib"/>
   ```
2. Run the command `setcap 'cap_net_bind_service=+ep cap_sys_rawio=+ep' INSTALL_DIR/platform/bin/vshell` to allow the API Gateway to listen on privileged ports.

#### API Manager

When API Manager is installed, you must run the `update-apimanager` script after the API Gateway post-install script to ensure that all paths are up-to-date. For details, see [Run update-apimanager](/docs/apim_installation/apigw_upgrade/upgrade_steps_extcass/#run-update-apimanager).

{{< alert title="Caution" color="warning" >}} Before executing the `update-apimanager` script:

* Apply the update to all API Gateways.
* Ensure that all Node Managers and API Gateway instances are running.

{{< /alert >}}

## Update a container deployment

If a `fed` file is provided as part of building the API Manager container, you must follow these steps to update the `fed` with the configuration changes:

1. Install the update on a installation of the API Gateway.
2. Run the following command:

   ```
   /opt/Axway-7.7/apigateway/posix/bin/update-apimanager --fed <path to old file>.fed --oa <path to update file>.fed
   ```

You do not need to run any API Manager instances.

The `fed` now contains the updates for the API Manager configuration and can be used to build containers.

## Documentation

This section describes documentation enhancements and related documentation.

### Documentation enhancements

The latest version of API Gateway, API Manager, and API Portal documentation has been migrated to Markdown format and is available in a [public GitHub repository](https://github.com/Axway/axway-open-docs) to prepare for future collaboration using an open source model. As part of this migration, the documentation has been restructured to help users navigate the content and find the information they are looking for more easily.

Documentation change history is now stored in GitHub. To see details of changes on any page, click the link in the **Last modified** section at the bottom of the page.

### Related documentation

To find all available documentation for this product version:

1. Go to [Manuals on the Axway Documentation portal](https://docs.axway.com/bundle).
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en) - Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.
* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en) - Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email [support@axway.com](mailto:support@axway.com) or visit [Axway Support](https://support.axway.com/).

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.