[waf-mode-instr]:                   ../admin-en/configure-wallarm-mode.md
[blocking-page-instr]:              ../admin-en/configuration-guides/configure-block-page-and-code.md
[logging-instr]:                    ../admin-en/configure-logging.md
[proxy-balancer-instr]:             ../admin-en/using-proxy-or-balancer-en.md
[process-time-limit-instr]:         ../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[allocating-memory-guide]:          ../admin-en/configuration-guides/allocate-resources-for-node.md
[enable-libdetection-docs]:         ../admin-en/configure-parameters-en.md#wallarm_enable_libdetection
[ptrav-attack-docs]:                ../attacks-vulns-list.md#path-traversal
[attacks-in-ui-image]:              ../images/admin-guides/test-attacks-quickstart.png
[nginx-process-time-limit-docs]:    ../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[nginx-process-time-limit-block-docs]:  ../admin-en/configure-parameters-en.md#wallarm_process_time_limit_block
[overlimit-res-rule-docs]:           ../user-guides/rules/configure-overlimit-res-detection.md
[graylist-docs]:                     ../user-guides/ip-lists/graylist.md
[waf-mode-instr]:                   ../admin-en/configure-wallarm-mode.md
[envoy-process-time-limit-docs]:    ../admin-en/configuration-guides/envoy/fine-tuning.md#process_time_limit
[envoy-process-time-limit-block-docs]: ../admin-en/configuration-guides/envoy/fine-tuning.md#process_time_limit_block
[ip-lists-docs]:                    ../user-guides/ip-lists/overview.md

# Upgrading the Docker NGINX- or Envoy-based image

These instructions describe the steps to upgrade the running Docker NGINX- or Envoy-based image 4.0 or 3.x to the version 4.2.

!!! warning "Using credentials of already existing Wallarm node"
    We do not recommend using the already existing Wallarm node of the previous version. Please follow these instructions to create a new filtering node of the version 4.2 and deploy it as the Docker container.

To upgrade the node 2.18 or lower, please use the [different instructions](older-versions/docker-container.md).

## Requirements

--8<-- "../include/waf/installation/requirements-docker-nginx-4.0.md"

## Step 1: Update API port

--8<-- "../include/waf/upgrade/api-port-443.md"

## Step 2: Download the updated filtering node image

=== "NGINX-based image"
    ``` bash
    docker pull wallarm/node:4.2.2-1
    ```
=== "Envoy-based image"
    ``` bash
    docker pull wallarm/envoy:4.2.0-1
    ```

## Step 3: Switch to the token-based connection to the Wallarm Cloud

If upgrading the node from the version 3.6 or lower, consider that the approach to connect the container to the Wallarm Cloud has been upgraded:

* [The "email and password"-based approach has been deprecated](/4.0/updating-migrating/what-is-new/#unified-registration-of-nodes-in-the-wallarm-cloud-by-tokens). In this approach, the node was registered in the Wallarm Cloud automatically once the container started with correct credentials passed in the `DEPLOY_USER` and `DEPLOY_PASSWORD` variables.
* The token-based approach has been included. To connect the container to the Cloud, run the container with the `WALLARM_API_TOKEN` variable containing the Wallarm node token copied from the Wallarm Console UI.

It is recommended to use the new approach to run the image 4.x. The "email and password"-based approach will be deleted in future releases, please migrate before.

To create a new Wallarm node and get its token:

1. Open Wallarm Console → **Nodes** in the [US Cloud](https://us1.my.wallarm.com/nodes) or [EU Cloud](https://my.wallarm.com/nodes) and create the node of the **Wallarm node** type.

    ![!Wallarm node creation](../images/user-guides/nodes/create-cloud-node.png)
1. Copy the generated token.

## Step 4: Switch from deprecated configuration options

If upgrading the node from version 3.6, please consider the following deprecated configuration options:

* The NGINX directive `wallarm_ts_request_memory_limit` has been renamed to [`wallarm_general_ruleset_memory_limit`](../admin-en/configure-parameters-en.md#wallarm_general_ruleset_memory_limit).

    We only changed the directive name, its logic remains the same. Directive with former name will be deleted in future releases, so you are recommended to rename it before.
    
    Please check if the directive with former name is explicitly specified in the mounted configuration files. If so, rename it.
* The `wallarm_request_time` [logging variable](../admin-en/configure-logging.md#filter-node-variables) has been renamed to `wallarm_request_cpu_time`.

    We only changed the variable name, its logic remains the same. The old name is temporarily supported as well, but still it is recommended to rename the variable.
* The following Envoy parameters have been renamed:

    * `tsets` section → `rulesets`, and correspondingly the `tsN` entries in this section → `rsN`
    * `ts` → [`ruleset`](../admin-en/configuration-guides/envoy/fine-tuning.md#ruleset_param)
    * `ts_request_memory_limit` → [`general_ruleset_memory_limit`](../admin-en/configuration-guides/envoy/fine-tuning.md#request-filtering-settings)

    We only changed parameter names, their logic remains the same. Parameters with former names will be deleted in future releases, so you are recommended to rename them before.
    
    Please check if the parameters with former names are explicitly specified in the mounted configuration files. If so, rename them.

If you upgrade the node from version 3.4 or lower, there are more renamed directives and parameters:

* NGINX: `wallarm_instance` → [`wallarm_application`](../admin-en/configure-parameters-en.md#wallarm_application)
* NGINX: `wallarm_local_trainingset_path` → [`wallarm_custom_ruleset_path`](../admin-en/configure-parameters-en.md#wallarm_custom_ruleset_path)
* NGINX: `wallarm_global_trainingset_path` → [`wallarm_protondb_path`](../admin-en/configure-parameters-en.md#wallarm_protondb_path)
* Envoy: `lom` → [`custom_ruleset`](../admin-en/configuration-guides/envoy/fine-tuning.md#request-filtering-settings)
* Envoy: `instance` → [`application`](../admin-en/configuration-guides/envoy/fine-tuning.md#basic-settings)

## Step 5: Update the Wallarm blocking page (if upgrading NGINX-based image from version 3.4 or lower)

In node 3.6, the Wallarm sample blocking page has been changed. The logo and support email on the page are now empty by default.

If you upgrade the node from version 3.4 or lower and the node is configured to return the `&/usr/share/nginx/html/wallarm_blocked.html` page in response to the blocked requests, transfer this configuration as follows:

1. [Copy and customize](../admin-en/configuration-guides/configure-block-page-and-code.md#customizing-sample-blocking-page) the new version of a sample page.
1. [Mount](../admin-en/configuration-guides/configure-block-page-and-code.md#path-to-the-htm-or-html-file-with-the-blocking-page-and-error-code) the new customized or previously used page and the NGINX configuration file to a new Docker container in the next step.

## Step 6: Transfer the `overlimit_res` attack detection configuration from directives to the rule

--8<-- "../include/waf/upgrade/migrate-to-overlimit-rule-docker.md"

## Step 7: Stop the running container

```bash
docker stop <RUNNING_CONTAINER_NAME>
```

## Step 8: Run the container using the new image

Run the container using the updated image. You can pass the same configuration parameters that were passed when running a previous image version except for the ones listed in the previous steps.

There are two options for running the container using the updated image:

* **With the environment variables** specifying basic filtering node configuration
    * [Instructions for the NGINX-based Docker container →](../admin-en/installation-docker-en.md#run-the-container-passing-the-environment-variables)
    * [Instructions for the Envoy-based Docker container →](../admin-en/installation-guides/envoy/envoy-docker.md#run-the-container-passing-the-environment-variables)
* **In the mounted configuration file** specifying advanced filtering node configuration
    * [Instructions for the NGINX-based Docker container →](../admin-en/installation-docker-en.md#run-the-container-mounting-the-configuration-file)
    * [Instructions for the Envoy-based Docker container →](../admin-en/installation-guides/envoy/envoy-docker.md#run-the-container-mounting-envoyyaml)

## Step 9: Test the filtering node operation

--8<-- "../include/waf/installation/test-after-node-type-upgrade.md"

## Step 10: Delete the filtering node of the previous version

If the deployed image of the version 4.2 operates correctly, you can delete the filtering node of the previous version in Wallarm Console → **Nodes**.
