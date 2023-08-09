[wallarm-status-instr]:             ../admin-en/configure-statistics-service.md
[ptrav-attack-docs]:                ../attacks-vulns-list.md#path-traversal
[attacks-in-ui-image]:              ../images/admin-guides/test-attacks-quickstart.png
[waf-mode-instr]:                   ../admin-en/configure-wallarm-mode.md
[blocking-page-instr]:              ../admin-en/configuration-guides/configure-block-page-and-code.md
[logging-instr]:                    ../admin-en/configure-logging.md
[proxy-balancer-instr]:             ../admin-en/using-proxy-or-balancer-en.md
[process-time-limit-instr]:         ../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[configure-selinux-instr]:          ../admin-en/configure-selinux.md
[configure-proxy-balancer-instr]:   ../admin-en/configuration-guides/access-to-wallarm-api-via-proxy.md
[install-postanalytics-instr]:      ../admin-en/installation-postanalytics-en.md
[dynamic-dns-resolution-nginx]:     ../admin-en/configure-dynamic-dns-resolution-nginx.md
[enable-libdetection-docs]:         ../admin-en/configure-parameters-en.md#wallarm_enable_libdetection
[img-wl-console-users]:             ../images/check-users.png 
[img-create-wallarm-node]:      ../images/user-guides/nodes/create-cloud-node.png
[nginx-process-time-limit-docs]:    ../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[nginx-process-time-limit-block-docs]:  ../admin-en/configure-parameters-en.md#wallarm_process_time_limit_block
[overlimit-res-rule-docs]:           ../user-guides/rules/configure-overlimit-res-detection.md
[graylist-docs]:                     ../user-guides/ip-lists/graylist.md
[ip-lists-docs]:                    ../user-guides/ip-lists/overview.md

# Upgrading Wallarm NGINX modules

These instructions describe the steps to upgrade the Wallarm NGINX modules 4.0 or 3.x to version 4.2. Wallarm NGINX modules are the modules installed in accordance with one of the following instructions:

* [NGINX `stable` module](../installation/nginx/dynamic-module.md)
* [Module for NGINX from CentOS/Debian repositories](../installation/nginx/dynamic-module-from-distr.md)
* [NGINX Plus module](../installation/nginx-plus.md)

To upgrade the node 2.18 or lower, please use the [different instructions](older-versions/nginx-modules.md).

## Requirements

--8<-- "../include/waf/installation/requirements-docker-nginx-4.0.md"

## Upgrade procedure

* If filtering node and postanalytics modules are installed on the same server, then follow the instructions below to upgrade all packages.
* If filtering node and postanalytics modules are installed on different servers, **first** upgrade the postanalytics module following these [instructions](separate-postanalytics.md) and then perform the steps below for filtering node modules.

## Step 1: Update API port

--8<-- "../include/waf/upgrade/api-port-443.md"

## Step 2: Upgrade NGINX to the latest version

Upgrade NGINX to the latest version using the relevant instructions:

=== "NGINX stable"

    DEB-based distributions:

    ```bash
    sudo apt update
    sudo apt install nginx
    ```

    RPM-based distributions:

    ```bash
    sudo yum update
    sudo yum install nginx
    ```
=== "NGINX Plus"
    For NGINX Plus, please follow the [official upgrade instructions](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-plus/#upgrading-nginx-plus).
=== "NGINX from Debian/CentOS repository"
    For NGINX [installed from Debian/CentOS repository](../installation/nginx/dynamic-module-from-distr.md), please skip this step. The installed NGINX version will be upgraded [later](#step-4-upgrade-wallarm-packages) along with the Wallarm modules.

If your infrastructure needs to use a specific version of NGINX, please contact the [Wallarm technical support](mailto:support@wallarm.com) to build the Wallarm module for a custom version of NGINX.

## Step 3: Add new Wallarm repository

Delete the previous Wallarm repository address and add a repository with a new Wallarm node version package. Please use the commands for the appropriate platform.

**CentOS and Amazon Linux 2.0.2021x and lower**

=== "CentOS 7 and Amazon Linux 2.0.2021x and lower"
    ```bash
    sudo yum remove wallarm-node-repo
    sudo yum clean all
    sudo rpm -i https://repo.wallarm.com/centos/wallarm-node/7/4.2/x86_64/wallarm-node-repo-4.2-0.el7.noarch.rpm
    ```
=== "CentOS 8"
    !!! warning "Support for CentOS 8.x has been deprecated"
        Support for CentOS 8.x [has been deprecated](https://www.centos.org/centos-linux-eol/). You can install the Wallarm node on the AlmaLinux, Rocky Linux or Oracle Linux 8.x operating system insted.

        * [Installation instructions for NGINX `stable`](../installation/nginx/dynamic-module.md)
        * [Installation instructions for NGINX from CentOS/Debian repositories](../installation/nginx/dynamic-module-from-distr.md)
        * [Installation instructions for NGINX Plus](../installation/nginx-plus.md)
=== "AlmaLinux, Rocky Linux or Oracle Linux 8.x"
    ```bash
    sudo yum remove wallarm-node-repo
    sudo yum clean all
    sudo rpm -i https://repo.wallarm.com/centos/wallarm-node/8/4.2/x86_64/wallarm-node-repo-4.2-0.el8.noarch.rpm
    ```

**Debian and Ubuntu**

1. Open the file with the Wallarm repository address in the installed text editor. In these instructions, **vim** is used.

    ```bash
    sudo vim /etc/apt/sources.list.d/wallarm.list
    ```
2. Comment out or delete the previous repository address.
3. Add a new repository address:

    === "Debian 10.x (buster)"
        ```bash
        deb https://repo.wallarm.com/debian/wallarm-node buster/4.2/
        ```
    === "Debian 11.x (bullseye)"
        ```bash
        deb https://repo.wallarm.com/debian/wallarm-node bullseye/4.2/
        ```
    === "Ubuntu 18.04 LTS (bionic)"
        ```bash
        deb https://repo.wallarm.com/ubuntu/wallarm-node bionic/4.2/
        ```
    === "Ubuntu 20.04 LTS (focal)"
        ```bash
        deb https://repo.wallarm.com/ubuntu/wallarm-node focal/4.2/
        ```

## Step 4: Upgrade Wallarm packages

### Filtering node and postanalytics on the same server

1. Execute the following command to upgrade the filtering node and postanalytics modules:

    === "Debian"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys-4.2.md"
    === "Ubuntu"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys-4.2.md"
    === "CentOS or Amazon Linux 2.0.2021x and lower"
        ```bash
        sudo yum update
        ```
    === "AlmaLinux, Rocky Linux or Oracle Linux 8.x"
        ```bash
        sudo yum update
        ```
2. If the package manager asks for confirmation to rewrite the content of the configuration file `/etc/cron.d/wallarm-node-nginx`, send the option `Y`.

    The `/etc/cron.d/wallarm-node-nginx` content should be updated for the new script counting RPS to be downloaded.

    By default, the package manager uses the option `N` but the option `Y` is required for correct RPS counting.

### Filtering node and postanalytics on different servers

!!! warning "Sequence of steps to upgrade the filtering node and postanalytics modules"
    If the filtering node and postanalytics modules are installed on different servers, then it is required to upgrade the postanalytics packages before updating the filtering node packages.

1. Upgrade postanalytics packages following these [instructions](separate-postanalytics.md).
2. Upgrade Wallarm node packages:

    === "Debian"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys-4.2.md"
    === "Ubuntu"
        ```bash
        sudo apt update
        sudo apt dist-upgrade
        ```

        --8<-- "../include/waf/upgrade/warning-expired-gpg-keys-4.2.md"
    === "CentOS or Amazon Linux 2.0.2021x and lower"
        ```bash
        sudo yum update
        ```
    === "AlmaLinux, Rocky Linux or Oracle Linux 8.x"
        ```bash
        sudo yum update
        ```
3. If the package manager asks for confirmation to rewrite the content of the configuration file `/etc/cron.d/wallarm-node-nginx`, send the option `Y`.

    The `/etc/cron.d/wallarm-node-nginx` content should be updated for the new script counting RPS to be downloaded.

    By default, the package manager uses the option `N` but the option `Y` is required for correct RPS counting.

## Step 5: Update the node type

If upgrading the node 3.6 or lower, it is recommended to install the new node type instead of the deprecated one during migration to the latest version.

The deployed node 3.6 or lower has the deprecated **regular** type that is [now replaced with the new **Wallarm node** type](/4.0/updating-migrating/what-is-new/#unified-registration-of-nodes-in-the-wallarm-cloud-by-tokens). The regular node type will be removed in future releases, please migrate before.

!!! info "If the postanalytics module is installed on a separate server"
    If the initial traffic processing and postanalytics modules are installed on separate servers, it is recommended to connect these modules to the Wallarm Cloud using the same node token. The Wallarm Console UI will display each module as a separate node instance, e.g.:

    ![!Node with several instances](../images/user-guides/nodes/wallarm-node-with-two-instances.png)

    The Wallarm node has already been created during the [separate postanalytics module upgrade](separate-postanalytics.md). To connect the initial traffic processing module to the Cloud using the same node credentials:

    1. Copy the node token generated during the separate postanalytics module upgrade.
    1. Proceed to the 4th step in the list below.

To replace the regular node with the Wallarm node:

1. Open Wallarm Console → **Nodes** in the [US Cloud](https://us1.my.wallarm.com/nodes) or [EU Cloud](https://my.wallarm.com/nodes) and create the node of the **Wallarm node** type.

    ![!Wallarm node creation][img-create-wallarm-node]
1. Copy the generated token.
1. Pause the NGINX service on the server with the node of the older version:

    === "Debian"
        ```bash
        sudo systemctl stop nginx
        ```
    === "Ubuntu"
        ```bash
        sudo service nginx stop
        ```
    === "CentOS or Amazon Linux 2.0.2021x and lower"
        ```bash
        sudo systemctl stop nginx
        ```
    === "AlmaLinux, Rocky Linux or Oracle Linux 8.x"
        ```bash
        sudo systemctl stop nginx
        ```

    The NGINX service pausing mitigates the risk of incorrect RPS calculation.
1. Execute the `register-node` script to run the **Wallarm node**:

    === "US Cloud"
        ``` bash
        sudo /usr/share/wallarm-common/register-node -t <NODE_TOKEN> -H us1.api.wallarm.com --force
        ```
    === "EU Cloud"
        ``` bash
        sudo /usr/share/wallarm-common/register-node -t <NODE_TOKEN> --force
        ```
    
    * `<NODE_TOKEN>` is the Wallarm node token.
    * The `--force` option forces rewriting of the Wallarm Cloud access credentials specified in the `/etc/wallarm/node.yaml` file.

## Step 6: Update the Wallarm blocking page

In node 3.6, the Wallarm sample blocking page has been changed. The logo and support email on the page are now empty by default.

If you upgrade the node from version 3.4 or lower and the node is configured to return the `&/usr/share/nginx/html/wallarm_blocked.html` page in response to the blocked requests, [copy and customize](../admin-en/configuration-guides/configure-block-page-and-code.md#customizing-sample-blocking-page) the new version of a sample page.

## Step 7: Rename deprecated NGINX directives

If upgrading the node from version 3.6 or lower, rename the following NGINX directives if they are explicitly specified in configuration files:

* `wallarm_ts_request_memory_limit` → [`wallarm_general_ruleset_memory_limit`](../admin-en/configure-parameters-en.md#wallarm_general_ruleset_memory_limit)
* If you upgrade the node from version 3.4 or lower, there are three more renamed directives:

    * `wallarm_instance` → [`wallarm_application`](../admin-en/configure-parameters-en.md#wallarm_application)
    * `wallarm_local_trainingset_path` → [`wallarm_custom_ruleset_path`](../admin-en/configure-parameters-en.md#wallarm_custom_ruleset_path)
    * `wallarm_global_trainingset_path` → [`wallarm_protondb_path`](../admin-en/configure-parameters-en.md#wallarm_protondb_path)

We only changed directive names, their logic remains the same. The directives with former names will be deleted in future releases, so it is recommended to rename them before.

## Step 8: Update the node logging variables

If upgrading the node from version 3.6 or lower, please consider the following changes to the [node logging variables](../admin-en/configure-logging.md#filter-node-variables):

* The `wallarm_request_time` variable has been renamed to `wallarm_request_cpu_time`.

    We only changed the variable name, its logic remains the same. The old name is temporarily supported as well, but still it is recommended to rename the variable.
* The `wallarm_request_mono_time` variable has been added – place it in the configuration of the logging format if you need log information about total time being the sum of:

    * Time in the queue
    * Time in seconds the CPU spent processing the request

## Step 9: Transfer the `overlimit_res` attack detection configuration from directives to the rule

--8<-- "../include/waf/upgrade/migrate-to-overlimit-rule-nginx.md"

## Step 10: Restart NGINX

--8<-- "../include/waf/restart-nginx-3.6.md"

## Step 11: Test Wallarm node operation

--8<-- "../include/waf/installation/test-after-node-type-upgrade.md"

## Step 12: Delete the node of the previous version

Once the operation of the new node is properly tested, open the **Nodes** section of Wallarm Console and delete the regular node of the previous version from the list.

If the postanalytics module is installed on a separate server, please also delete the node instance related to this module.

## Settings customization

The Wallarm modules are updated to version 4.2. Previous filtering node settings will be applied to the new version automatically. To make additional settings, use the [available directives](../admin-en/configure-parameters-en.md).

--8<-- "../include/waf/installation/common-customization-options-nginx.md"
