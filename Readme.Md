# RHDH - Bitbucket Authentication 

Backstage provides an authentication provider (https://backstage.io/docs/auth/bitbucket/provider) for bitbucket authentication which can be directly configured with app-config.yaml but it will result in ```User not found ``` error as the user entity is not getting created by this.

The workaround is to use Oauth2Proxy -> KeyCloak to federate this as OIDC provider with identity provider as Bitbucket.

![Alt text](./docs/assets/bitbucket-rhdh.png?raw=true "Logical Architecture")

## KeyCloak

Install the RHSSO operator on Openshift Once the Operator is installed create a key cloak instance, realm , client

```
  oc apply -f ./KeyCloak/KeyCloak.yaml
```


## Bitbucket settings.

Setup OAUTH Consumer for Key Cloak. open URL : https://bitbucket.org/< Org name >/workspace/settings/api

![Alt text](./docs/assets/Bitbucket-oauthconsumer.png "OAuth Consumer")

Create an Oauth Consumer set the following

1) Callback URL : <<KeyCloak_Route_URL>>/auth/realms/<<realmname>>/broker/bitbucket/endpoint
2) Under Permissions enable Account Email,Read checkbox.

![Alt text](./docs/assets/bitbucket-consumer-details.png "OAuth Consumer Detail")


## KeyCloak Realm/client config

Replace the clientid and client secret in the file `KeyCloakRealm.yaml`

```
   oc apply -f ./KeyCloak/KeyCloakRealm.yaml
```

Create KeyCloak Client

```
   oc apply -f ./KeyCloak/KeyCloakClient.yaml
```