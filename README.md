# README
This repository allows for creating a k3s cluster with argocd.

## START K3S:
To start k3s, run the following commands:
```
sudo mkdir -p /mnt/disks/k3s
export INSTALL_K3S_EXEC="--data-dir /mnt/disks/k3s --token=token --write-kubeconfig=$HOME/.kube/config --write-kubeconfig-mode=644 --disable=traefik"
curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC=$INSTALL_K3S_EXEC sh -
```

## INSTALL HELM
To install helm, run the following commands:
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
/bin/bash get_helm.sh
rm get_helm.sh
```

## INSTALL ARGOCD:
To install argocd, run the following commands:
```
helm repo add argo https://argoproj.github.io/argo-helm
helm install argocd argo/argo-cd -n argocd --create-namespace --set server.extraArgs='{--insecure}'
```

## BOOTSTRAP FROM ARGOCD:
To bootstrap your cluster, run the following command:
```
kubectl apply -f https://raw.githubusercontent.com/d4depot/d4distributed-k3s-bootstrap/refs/heads/main/envs/gcp-prd.yaml
```

## CONNECT TO ARGOCD:
To connect to argocd, the following commands may be usefull:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
kubectl port-forward --address 192.168.68.108 service/argocd-server -n argocd 8000:80
```

## UNINSTALL K3S:
To uninstall k3s, run the following commands:
```
/usr/local/bin/k3s-uninstall.sh
sudo rm -rf /mnt/disks/k3s/
```