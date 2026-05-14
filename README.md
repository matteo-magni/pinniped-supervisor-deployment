# Pinniped Supervisor deployment

This codes aims at templating a Pinniped Supervisor deployment that can broker authentication against multiple identity providers,
in this case EntraID (via OIDC) and OpenLDAP.

## Structure

The `base` directory contains the generic resources, not customised for a specific installation:

- `base-install`: the pinniped-supervisor's base installation manifests fetched from github (v.0.44.0)
- `configuration`: divided into
  - `identity-providers`: one per upstream IdP, each one with specific settings
  - `federation-domains`: authentication endpoints that broker one or more identity providers
  - `oidc-clients`: to be mapped to actual clients that will utilise Pinniped Supervisor for authenticating users
- `nginx-reverse-proxy`: optional component that I used to proxy connections between Pinniped and the OpenLDAP instance with SSL termination, as cipher suites were incompatible and therefore they could not complete the SSL handshake phase.

The `base` directory is not sufficient to properly deploy the Pinniped Supervisor resources,
because some mandatory fields are missing and are only added via the specialised kustomization defined in the `instance-a` directory.
Such directory contains settings specific to a Pinniped Supervisor instance, in the form of kustomize constructs (i.e. patches).

The primary goal of this structure is to define multiple Pinniped deployments with a DRY approach.

This structure worked for me but may not work for somebody else.
For example, the OpenLDAP identity provider configuration is part of the base directory
because in my use case there were no differences between the instances.
However, one may want to differentiate this setup too, and therefore more kustomize changes may be needed on a per-instance basis.

## Sample deployment

A soon as all the configurations have been adjusted, deploying it is as simple as

```shell
k apply -k instance-a
```
