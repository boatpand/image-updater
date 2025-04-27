## Argo Imange Updater and MSTeams notification

Can read app-example.json for customize template of notification.

### must add secrets before apply
* argo/argo-cd/container-registry-secrets.yaml.yaml
* argo/argo-cd/argocd-notifications-secrets.yaml
* argo/argo-crd/ssh-secrets.yaml
* nginx/container-registry-secrets.yaml

#### argo/argo-cd/container-registry-secrets.yaml.yaml
```sh
apiVersion: v1
kind: Secret
metadata:
  name: container-registry-secret
  namespace: argo-cd
type: kubernetes.io/dockerconfigjson
stringData:
  .dockerconfigjson: '{"auths":{"https://registry-1.docker.io":{"auth":""}}}'
```

#### argo/argo-cd/argocd-notifications-secrets.yaml
```sh
apiVersion: v1
kind: Secret
metadata:
  name: argocd-notifications-secret
  namespace: argo-cd
stringData:
  channel-teams-url: 
```

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
  .dockerconfigjson: '{"auths":{"https://registry-1.docker.io":{"auth":""}}}'
```

### Apply Configmap and Secrets
```sh
kubectl apply -f argo/argo-cd/argocd-notifications-cm.yaml
kubectl apply -f argo/argo-cd/container-registry-secrets.yaml.yaml
kubectl apply -f argo/argo-cd/argocd-notifications-secrets.yaml
kubectl apply -f argo/argo-crd/ssh-secrets.yaml
kubectl apply -f nginx/container-registry-secrets.yaml
```

### Run kustomization
```sh
cd argo/argo-cd
kubectl kustomize . --enable-helm | kubectl apply -f -

cd nginx
kubectl kustomize . --enable-helm | kubectl apply -f -

cd argo/argo-crd
kubectl kustomize . --enable-helm | kubectl apply -f -
```