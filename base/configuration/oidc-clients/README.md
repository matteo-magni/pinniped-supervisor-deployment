# OIDC Clients

As soon as an OIDC client has been created, it is possible to request Pinniped to generate a client secret with the following command

```shell
kubectl apply -n pinniped-supervisor -o yaml -f - <<EOF
apiVersion: clientsecret.supervisor.pinniped.dev/v1alpha1
kind: OIDCClientSecretRequest
metadata:
  name: client.oauth.pinniped.dev-vsphere-supervisor
spec:
  generateNewSecret: true
EOF
```

The status will display the generated secret, which will no longer be available to see.
Optionally, it is also possible to revoke old secrets, setting `.spec.revokeOldSecrets: true`.

**N.B.** The client application must also trust the certificate of the CA signing the federation domain's certificate.
