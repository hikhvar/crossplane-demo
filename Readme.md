# Crossplane AWS Demo

Install:
```
# Setup eks cluster
eksctl create cluster

aws eks list-clusters
aws eks update-kubeconfig --name foo

# Install crossplane
kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane --namespace crossplane-system crossplane-stable/crossplane

kubectl-crossplane  install configuration registry.upbound.io/xp/getting-started-with-aws:v1.5.1

# Install argocd
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/core-install.yaml
```

Setup credentials:
```
echo -e "[default]\naws_access_key_id = $(aws configure get aws_access_key_id --profile $AWS_PROFILE)\naws_secret_access_key = $(aws configure get aws_secret_access_key --profile $AWS_PROFILE)" > creds.conf

kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=./creds.conf
```
