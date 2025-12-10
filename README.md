For cluster access, you need [kubelogin](https://github.com/int128/kubelogin) installed.

A client secret is needed in the complete configuration. Please request the value from your trusty admin team.

Set up a config section for our cluster:

```shell
kubectl config set-cluster ffddorf-k3s \
  --server https://k3s.ffddorf.net
```

Configure client credentials - complete with the provided client secret!

```shell
kubectl config set-credentials ffddorf-oidc \
  --exec-api-version=client.authentication.k8s.io/v1beta1 \
  --exec-command=kubectl \
  --exec-arg=oidc-login \
  --exec-arg=get-token \
  --exec-arg=--oidc-issuer-url=https://accounts.google.com \
  --exec-arg=--oidc-client-id=667020132120-ah9lvfisjud49diutv4vm6l394g3pht6.apps.googleusercontent.com \
  --exec-arg=--oidc-client-secret=
```

Set up a working context that binds the credentials to the cluster 

```shell
kubectl config set-context ffddorf-k3s \
  --cluster=ffddorf-k3s \
  --user=ffddorf-oidc \
  --namespace=default
```

Set the current working context

```shell
kubectl config use-context ffddorf-k3s
```

Verify

```shell
kubectl cluster-info
```
