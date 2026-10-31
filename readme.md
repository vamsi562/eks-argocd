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

##
- In argocd, everything is a application
- for managing namespaces, storage classes, ebs, efs
we will create separate folders for each i.e namespace - we will keep all our namespace yamls here & similar
- another applications folder where we define main yamls related to argocd and reference respective git url in source
eg: to create two namespaces i.e roboshop & expense, two yamls were created in namespace folder. In applications folder we create a namespace yaml that include api version of argocd , refer above namespace folder git url.
- argo cd by default sync every minutes. we can change it, by modifying argocd config amp by adding
        ` time.reconciliation: <time in seconds> ` 

## app of apps:
- here we create another yaml and under source path we provide applications folder
- once we this yaml applied it will create all under applications folder - namespace, storage class


### argo cd uses
- manage cluster - mostly suitable for this 
- deployment - we cant control as if any change push to github, argo cd automatically refreshs. Deploying controlled way recommended.
  - in this cas we can write an application yaml, which contains argocd and helm 

refer - catalogue-deploy repo for application.yaml