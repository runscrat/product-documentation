[statistics-service-all-parameters]:        ../admin-en/configure-statistics-service.md
[img-attacks-in-interface]:                 ../images/admin-guides/test-attacks-quickstart.png
[tarantool-status]:                         ../images/tarantool-status.png
[configure-proxy-balancer-instr]:           ../admin-en/configuration-guides/access-to-wallarm-api-via-proxy.md
[sqli-attack-docs]:                         ../attacks-vulns-list.md#sql-injection
[xss-attack-docs]:                          ../attacks-vulns-list.md#crosssite-scripting-xss

# Upgrading Wallarm node with All-in-One Installer

These instructions describe the steps to upgrade the Wallarm node 4.6.x installed using [all-in-one installer](../installation/nginx/all-in-one.md) to version 4.6.x+.

## Requirements

--8<-- "../include/waf/installation/all-in-one-upgrade-requirements.md"

## Upgrade procedure

The upgrade procedure differs depending on how filtering node and postanalytics modules are installed:

* [On the same server](#filtering-node-and-postanalytics-on-the-same-server): modules are upgraded altogether
* [On different servers](#filtering-node-and-postanalytics-on-different-servers): **first** upgrade the postanalytics module and **then** the filtering module

## Filtering node and postanalytics on the same server

Use the procedure below to upgrade altogether the filtering node and postanalytics modules installed using all-in-one installer on the same server.

### Step 1: Prepare Wallarm token

To upgrade node, you will need a Wallarm token of [one of the types](../user-guides/nodes/nodes.md#api-and-node-tokens-for-node-creation). To prepare a token:

=== "API token"

    1. Open Wallarm Console → **Settings** → **API tokens** in the [US Cloud](https://us1.my.wallarm.com/settings/api-tokens) or [EU Cloud](https://my.wallarm.com/settings/api-tokens).
    1. Find or create API token with the `Deploy` source role.
    1. Copy this token.

=== "Node token"

    For upgrade, use the same node token that was used for installation:

    1. Open Wallarm Console → **Nodes** in the [US Cloud](https://us1.my.wallarm.com/nodes) or [EU Cloud](https://my.wallarm.com/nodes).
    1. In your existing node group, copy token using node's menu → **Copy token**.

### Step 2: Download newest version of all-in-one Wallarm installer

--8<-- "../include/waf/installation/all-in-one-installer-download.md"

### Step 3: Run all-in-one Wallarm installer

--8<-- "../include/waf/installation/all-in-one-installer-run.md"

### Step 4: Restart NGINX

--8<-- "../include/waf/installation/restart-nginx-systemctl.md"

### Step 5: Test Wallarm node operation

To test the new node operation:

1. Send the request with test [SQLI][sqli-attack-docs] and [XSS][xss-attack-docs] attacks to the protected resource address:

    ```
    curl http://localhost/?id='or+1=1--a-<script>prompt(1)</script>'
    ```

1. Open the Wallarm Console → **Events** section in the [US Cloud](https://us1.my.wallarm.com/search) or [EU Cloud](https://my.wallarm.com/search) and ensure attacks are displayed in the list.
1. As soon as your Cloud stored data (rules, IP lists) is synchronized to the new node, perform some test attacks to make sure your rules work as expected.

## Filtering node and postanalytics on different servers

!!! warning "Sequence of steps to upgrade the filtering node and postanalytics modules"
    If the filtering node and postanalytics modules are installed on different servers, then it is required to upgrade the postanalytics packages before updating the filtering node packages.

### Step 1: Prepare Wallarm token

To upgrade node, you will need a Wallarm token of [one of the types](../user-guides/nodes/nodes.md#api-and-node-tokens-for-node-creation). To prepare a token:

=== "API token"

    1. Open Wallarm Console → **Settings** → **API tokens** in the [US Cloud](https://us1.my.wallarm.com/settings/api-tokens) or [EU Cloud](https://my.wallarm.com/settings/api-tokens).
    1. Find or create API token with the `Deploy` source role.
    1. Copy this token.

=== "Node token"

    For upgrade, use the same node token that was used for installation:

    1. Open Wallarm Console → **Nodes** in the [US Cloud](https://us1.my.wallarm.com/nodes) or [EU Cloud](https://my.wallarm.com/nodes).
    1. In your existing node group, copy token using node's menu → **Copy token**.

### Step 2: Download newest version of all-in-one Wallarm installer to postanalytics machine

This step is performed on the postanalytics machine.

--8<-- "../include/waf/installation/all-in-one-installer-download.md"

### Step 3: Run all-in-one Wallarm installer to upgrade postanalytics

This step is performed on the postanalytics machine.

--8<-- "../include/waf/installation/all-in-one-postanalytics.md"

### Step 4: Download newest version of all-in-one Wallarm installer to filtering node machine

This step is performed on the filtering node machine.

--8<-- "../include/waf/installation/all-in-one-installer-download.md"

### Step 5: Run all-in-one Wallarm installer to upgrade filtering node

This step is performed on the filtering node machine.

To upgrade filtering node separately with all-in-one installer, use:

=== "API token"
    ```bash
    # If using the x86_64 version:
    sudo env WALLARM_LABELS='group=<GROUP>' sh wallarm-4.6.12.x86_64-glibc.sh filtering

    # If using the ARM64 version:
    sudo env WALLARM_LABELS='group=<GROUP>' sh wallarm-4.6.12.aarch64-glibc.sh filtering
    ```        

    The `WALLARM_LABELS` variable sets group into which the node will be added (used for logical grouping of nodes in the Wallarm Console UI).

=== "Node token"
    ```bash
    # If using the x86_64 version:
    sudo sh wallarm-4.6.12.x86_64-glibc.sh filtering

    # If using the ARM64 version:
    sudo sh wallarm-4.6.12.aarch64-glibc.sh filtering
    ```

### Step 6: Check the filtering node and separate postanalytics modules interaction

--8<-- "../include/waf/installation/all-in-one-postanalytics-check.md"
