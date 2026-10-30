# AWS CLI EKS Cheatsheet

## Configure Variables

```bash
export CLUSTER_NAME="my-cluster"
export AWS_REGION="us-east-1"
export AWS_ACCOUNT_ID="123456789012"
```

## List and Describe Clusters

### List EKS Clusters

```bash
aws eks list-clusters --region $AWS_REGION | jq -r '.clusters[]'
```

### Describe EKS Cluster

```bash
aws eks describe-cluster --name $CLUSTER_NAME --region $AWS_REGION | jq '.cluster'
```

### Get Cluster Endpoint and CA Certificate

```bash
aws eks describe-cluster --name $CLUSTER_NAME --region $AWS_REGION | \
  jq '.cluster.endpoint, .cluster.certificateAuthority.data'
```

## Kubeconfig Management

### Update Kubeconfig for Cluster

```bash
aws eks update-kubeconfig --region $AWS_REGION --name $CLUSTER_NAME
```

### View Current Context

```bash
kubectl config current-context
kubectl cluster-info
```

## Node Groups

### List Node Groups

```bash
aws eks list-nodegroups --cluster-name $CLUSTER_NAME --region $AWS_REGION
```

### Describe Node Group

```bash
aws eks describe-nodegroup --cluster-name $CLUSTER_NAME --nodegroup-name my-nodegroup \
  --region $AWS_REGION | jq '.nodegroup'
```

### Scale Node Group

```bash
aws eks update-nodegroup-config --cluster-name $CLUSTER_NAME --nodegroup-name my-nodegroup \
  --scaling-config minSize=1,maxSize=5,desiredSize=3 --region $AWS_REGION
```

## Add-ons Management

### List Add-ons

```bash
aws eks list-addons --cluster-name $CLUSTER_NAME --region $AWS_REGION
```

### Create Add-on (e.g., VPC CNI)

```bash
aws eks create-addon --cluster-name $CLUSTER_NAME --addon-name vpc-cni \
  --service-role-arn arn:aws:iam::$AWS_ACCOUNT_ID:role/eks-service-role \
  --region $AWS_REGION
```

## Troubleshooting

### Check Cluster Status

```bash
# Get cluster status
aws eks describe-cluster --name $CLUSTER_NAME --region $AWS_REGION | jq '.cluster.status'

# Get resource tags
aws eks describe-cluster --name $CLUSTER_NAME --region $AWS_REGION | jq '.cluster.tags'
```

### Verify API Connectivity

```bash
kubectl get nodes
kubectl get pods --all-namespaces
```