# RHDH - Bitbucket Authentication 

Backstage provides an authentication provider for bitbucket authentication which can be directly configured with app-config.yaml but it will result in `User not found` error as the user entity is not getting created by this.

The workaround is to use Oauth2Proxy -> KeyCloak to federate this as OIDC provider with identity provider as Bitbucket.

## Bitbucket settings.