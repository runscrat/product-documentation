# IP address denylist

**Denylist** is a list of IP addresses that are not allowed to access your applications even if originating legitimate requests. The filtering node in any [mode](../../admin-en/configure-wallarm-mode.md) blocks all requests originated from denylisted IP addresses (unless IPs are duplicated in the [allowlist](allowlist.md)).

In the Wallarm Console → **IP lists** → **Denylist**, you can manage blocked IP addresses as follows:

--8<-- "../include/waf/features/ip-lists/common-actions-with-lists-overview.md"

![!IP denylist](../../images/user-guides/ip-lists/denylist-apps.png)

!!! info "Old name of the list"
    The old name of the IP address denylist is "IP address blacklist".

## Examples of IP denylist usage

* Block IP addresses from which several consecutive attacks were originated.

    An attack may include several requests originated from one IP address and containing malicious payloads of different types. One of the methods to block such attacks is to block requests origin. You can configure automatic source IP blocking by configuring the threshold for source IP blocking and appropriate reaction in the [trigger](../triggers/trigger-examples.md#denylist-ip-if-4-or-more-attack-vectors-are-detected-in-1-hour).
* Block behavioral-based attacks.

    The Wallarm filtering node can block most harmful traffic request-by-request if a malicious payload is detected. However, for behavioral‑based attacks when every single request is legitimate (e.g. login attempts with username/password pairs) blocking by origin might be necessary.

    By default, automatic blocking of behavioral attacks source is disabled. [Instructions on configuring brute force protection →](../../admin-en/configuration-guides/protecting-against-bruteforce.md#configuration-steps)

--8<-- "../include/waf/features/ip-lists/common-actions-with-lists-allow-apps.md"

!!! warning "Re-adding deleted IP address"
    After manually deleting the IP address added to the list by the [trigger](../triggers/triggers.md), the trigger will run again only after half of the previous time the IP address was in the list.
    
    For example:

    1. IP address was automatically added to the graylist for 1 hour because 4 different attack vectors were received from this IP address in 3 hours (as it is configured in the [trigger](../triggers/trigger-examples.md#graylist-ip-if-4-or-more-attack-vectors-are-detected-in-1-hour)).
    2. User deleted this IP address from the graylist via Wallarm Console.
    3. If 4 different attack vectors are sent from this IP address within 30 minutes, then this IP address will not be added to the graylist.