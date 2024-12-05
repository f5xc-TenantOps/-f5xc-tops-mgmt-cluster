# 🥾 Bootstrap

## Install microk8s
```shell
sudo snap install microk8s --classic --channel=latest/stable
sudo usermod -a -G microk8s $USER
mkdir -p ~/.kube
chmod 0700 ~/.kube
su - $USER
```

## Enable microk8s addons
```shell
microk8s enable hostpath-storage
sudo apt-get install git
git config --global --add safe.directory /snap/microk8s/current/addons/community/.git
microk8s enable community
microk8s enable cert-manager
```

## Configure cert-manager
```shell
kubectl apply -f cert-manager-components.yaml
microk8s enable ingress:default-ssl-certificate=kube-system/wildcard-k11s-io
```

## Configure argocd
```shell
kc replace -f argocd-components.yaml 
```

## Add argocd account(s)
```shell
argocd account update-password --account <new-account-name> --current-password <admin-password> --new-password <new-account-password>
```

## Apply initial ArgoCD App
```shell
kubectl apply -f argocd/argocd-app.yml
```

## Prepare the ``tfc-operator-system`` NS
```shell
export NAMESPACE=tfc-operator-system
kubectl create namespace $NAMESPACE
kubectl -n $NAMESPACE apply -f bootstrap-secrets.yml
```

