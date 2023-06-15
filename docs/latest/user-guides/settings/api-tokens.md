[user-roles-article]:       ../../user-guides/settings/users.md#user-roles
[img-api-tokens-edit]:      ../../images/api-tokens-edit.png

# API Tokens

In Wallarm Console → **Settings** → **API tokens**, you can manage tokens for [API request authentication](../../api/overview.md).

![!Wallarm API token][img-api-tokens-edit]

This section is available for the users of **[all roles][user-roles-article]** besides **Read Only** and **API developer**.

## Configuring tokens

Users can create own tokens and use them (which means, view token value and include it into API request to authenticate it). For each own token you can set permissions, but not wider than the ones your user has. Optionally, you can set expiration date for the token - if set, the token will be disabled after that date. Also, you can disable/enable your tokens manually.

You can renew the token value at any moment.

**Administrators** / **Global Administrators** can view and manage all tokens in the company account. Besides private tokens, they can create shared ones, that can be viewed/used by other administrators. When specifying permissions for the tokens, they can select to take these permissions from the selected role:

* Administrator
* Analyst
* API Developer
* Read only
* Deploy - API tokens with this role are used to [deploy Wallarm nodes](../../user-guides/nodes/nodes.md#creating-a-node)
* Сustom - switches back to the  manual permission selection

!!! info "Token privacy"
    No other users (even administrators) can use your private tokens (which means, view or copy token value). Besides, non-administrators will not even see your tokens.

Consider that:

* If the token owner has been [disabled](../../user-guides/settings/users.md#disable-access-for-a-user), all one's tokens are automatically disabled as well.
* If the token owner has been reduced in permissions, corresponding permissions will be removed from all one's tokens.
* All disabled tokens are automatically removed in a week after disabling.
* To enable previously disabled token, save it with the new expiration date.

## Backward-compatible tokens

Previously UUID and secret key were used for request authentication which is now replaced with tokens. The UUID and secret key your were using are automatically transformed to the **backward-compatible** token. With this token requests authenticated with UUID and secret key will continue working.

!!! warning "Renew token or enable SSO"
    If you renew the value of the backward-compatible token or enable [SSO/strict SSO](../../admin-en/configuration-guides/sso/employ-user-auth.md) for this token's owner, the backward compatibility ends - all requests authenticated with old UUID and secret key will stop working.

You can also use the generated value of the backward-compatible token passing it in the `X-WallarmApi-Token` header parameter of your requests.

Backward-compatible token has the same permissions as the user role does, these permissions are not displayed in the token window and cannot be changed. If you want to control permissions, you need to remove a backward-compatible token and create a new one.

## API tokens vs. node tokens

You can use API tokens described in this article for Wallarm Cloud API [request authentication](../../api/overview.md) from any client and with any set of permissions.

One of the clients accessing Wallarm Cloud API is Wallarm filtering node itself. To grant a filtering node with the access to API of Wallarm Cloud, besides API tokens, you can use node tokens. [Know the difference and what to prefer →](../../user-guides/nodes/nodes.md#api-and-node-tokens-for-node-creation)

!!! info "API tokens are not supported by some deployment options"
    API tokens currently cannot be used for [NGINX](../../admin-en/installation-kubernetes-en.md) and [Kong](../../installation/kubernetes/kong-ingress-controller/deployment.md) Ingress controllers and [Sidecar proxy](../../installation/kubernetes/sidecar-proxy/deployment.md) deployments, as well as for AWS deployments based on [Terraform module](../../installation/cloud-platforms/aws/terraform-module/overview.md). Use node tokens instead.
