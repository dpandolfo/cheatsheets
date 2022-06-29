# AWS CLI EKS Cheatsheet

## List ECS Clusters
```
aws eks list-clusters | jq .clusters
```

## Configure Variable for Cluster Name
```
export CLUSTER_NAME = CLUSTER_NAME
```
## Describe EKS Cluster
```
aws eks describe-cluster --name $CLUSTER_NAME | jq .cluster
```

## Configure Variable for Region
```
export AWS_REGION = AWS_REGION
```

## Update Kubeconfig for Cluster
```
aws eks update-kubeconfig --region $AWS_REGION --name $CLUSTER_NAME
```

## Get K8s Current Context 
```
kubectl config current-context
kubectl cluster-info
```

## Util K8s Commands
```
kubectl describe pods <>
kubectl logs -f <>
kubectl get namespace
kubectl get all --all-namespaces 
kubectl get pods -o wide --all-namespaces
kubectl get pods
kubectl get service
kubectl get deploy
kubectl get deployments
kubectl get pods
kubectl get hpa 
kubectl get cronjobs
kubectl get ingress
kubectl get serviceaccounts
kubectl get dns
kubectl get secrets
kubectl get configmaps
kubectl get endpoints
kubectl get events
```