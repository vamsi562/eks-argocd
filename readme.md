# ARGO CD
aws eks update-kubeconfig --region us-east-1 --name roboshop-dev

## Installation
kubectl create namespace argocd
kubectl apply -n argocd --server-side --force-conflicts -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

## ARGOCLI Installation
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64

## Change from cluster ip to load balancer
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
 <!-- once changed service from custer ip to load balancer we can use argocd UI-->

## initial password 
argocd admin initial-password -n argocd <!-- initial password, username admin -->

argocd account update-password

## Steps
- In argocd, everything is a application
- 