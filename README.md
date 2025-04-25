### must add secrets before apply
* argo/argo-crd/ssh-secrets.yaml
* nginx/container-registry-secrets.yaml

#### argo/argo-crd/ssh-secrets.yaml
```sh
apiVersion: v1
kind: Secret
metadata:
  name: 
  namespace: 
  labels:
    argocd.argoproj.io/secret-type: repository
type: Opaque
stringData:
  url: git@github.com
  sshPrivateKey: |
    -----BEGIN OPENSSH PRIVATE KEY-----

    -----END OPENSSH PRIVATE KEY-----
```

#### nginx/container-registry-secrets.yaml
```sh
apiVersion: v1
kind: Secret
metadata:
  name: 
type: kubernetes.io/dockerconfigjson
stringData:
  .dockerconfigjson: '{"auths":{"https://index.docker.io/v1/":{"auth":""}}}'
```

### create container regisstry secret for pulling images
```sh
kubectl apply -f nginx/container-registry-secrets.yaml
```

### Run kustomization
```sh
kubectl kustomize . --enable-helm | kubectl apply -f -
```