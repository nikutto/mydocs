# OAuth2

## Summary

OAuth2.0 is a protocol for authorization.
In this protocol, resource owner can authorize third-party client app to access proteced resource.
Defined in RFC-6749.

## Terminology

### Role

Refer https://datatracker.ietf.org/doc/html/rfc6749#section-1.1.

- resource owner: Owner of the resource. They are often end-user.
- authorization server: Server for authorization.
- resource server: A server which has a protected resources.
- client: An application which want to access protected resource.

Connection between authorization server and resource server is out of scope of OAuth2.

## Authorization flow

In RFC-6749, there are 5 authorization flow.
- Authorization code flow
- Implicit flow
    - Clients skip to get authorization code and directly get access token.
- Resource owner password credentials flow
    - Pass passwords to client and client manage them.
    - Only for special reason
        - For example, to migrate from other authorization method.
- Client credentials flow
    - Only authorize client
    - There are no resource owner
- Refresh token flow

The most import flow is authoirzation code flow.

## Introspection

Token Introspection is defined in RFC-7662.
- https://datatracker.ietf.org/doc/html/rfc7662

This achive token introspection by resource server.
Minimal information is returned, for example, 
to whom it has been issued or when it will expire.

## OIDC

Open ID Connect.

### Claim

- sub: Subject to issue.
- iat: Issued at.
- exp: When it will expire at.

### OIDC Discovery

https://openid.net/specs/openid-connect-discovery-1_0.html

For example,
- https://example.com/.well-known/openid-configuration


## Security tips

### State and PKCE

A state is a random value used to prevent CSRF attack. 
PKCE is a protocol to prevent authorization code interception attack. Defined in RFC-7636.
PKCE dosen't protect CSRF attack.
So, it it good to use both of them.
A state is required parameter and PKCE is recommended parameter. If the client app is native app on smartphones, you should use PKCE.

### Redirect url

Redirect urls should be protected by TLS.
Clients must configure redirect urls previously.
Clients can configure multiple redirect urls. In this case, clients must specify redirect url for authorization request.


## References

- OAuth2: https://datatracker.ietf.org/doc/html/rfc6749
- PKCE: https://datatracker.ietf.org/doc/html/rfc7636
- https://qiita.com/
TakahikoKawasaki/items/200951e5b5929f840a1f