# Kubectl Essentials

## Context and Cluster Management

```bash
# View current context
kubectl config current-context

# List all contexts
kubectl config get-contexts

# Switch context
kubectl config use-context my-context

# Get cluster info
kubectl cluster-info

# Get API resources
kubectl api-resources
```

## Namespace Management

```bash
# List namespaces
kubectl get namespace

# Create namespace
kubectl create namespace my-namespace

# Set default namespace
kubectl config set-context --current --namespace=my-namespace

# Delete namespace (cascades to all resources)
kubectl delete namespace my-namespace
```

## Pod Operations

### List and View Pods

```bash
# List pods in current namespace
kubectl get pods

# List pods in specific namespace
kubectl get pods -n my-namespace

# List all pods across namespaces
kubectl get pods --all-namespaces

# List with more details
kubectl get pods -o wide

# Watch pods in real-time
kubectl get pods --watch

# Get single pod details
kubectl describe pod my-pod

# Get pod as YAML
kubectl get pod my-pod -o yaml
```

### Pod Logs

```bash
# View logs
kubectl logs my-pod

# Follow logs in real-time
kubectl logs -f my-pod

# View logs from previous container (if crashed)
kubectl logs my-pod --previous

# View logs from specific container in multi-container pod
kubectl logs my-pod -c container-name

# Get last 100 lines
kubectl logs my-pod --tail=100
```

### Pod Execution

```bash
# Execute command in pod
kubectl exec my-pod -- ls -la

# Interactive shell in pod
kubectl exec -it my-pod -- /bin/bash

# Run command in specific container
kubectl exec -it my-pod -c container-name -- /bin/bash

# Copy files from pod
kubectl cp my-pod:/path/to/file ./local-file

# Copy files to pod
kubectl cp ./local-file my-pod:/path/to/file
```

### Port Forwarding

```bash
# Forward local port to pod
kubectl port-forward my-pod 8080:8080

# Forward local port to service
kubectl port-forward svc/my-service 8080:8080

# Forward to specific pod in background
kubectl port-forward my-pod 8080:8080 &
```

## Deployment Management

```bash
# List deployments
kubectl get deploy

# Describe deployment
kubectl describe deployment my-deployment

# Scale deployment
kubectl scale deployment my-deployment --replicas=3

# Update deployment image
kubectl set image deployment/my-deployment \
  my-container=my-image:v2 --record

# Rollout status
kubectl rollout status deployment/my-deployment

# View rollout history
kubectl rollout history deployment/my-deployment

# Rollback to previous version
kubectl rollout undo deployment/my-deployment

# Rollback to specific revision
kubectl rollout undo deployment/my-deployment --to-revision=2

# Restart deployment (rolling restart)
kubectl rollout restart deployment/my-deployment
```

## Services and Networking

```bash
# List services
kubectl get service

# Describe service
kubectl describe service my-service

# Get service endpoints
kubectl get endpoints my-service

# List ingresses
kubectl get ingress

# Describe ingress
kubectl describe ingress my-ingress
```

## Configuration and Secrets

```bash
# List configmaps
kubectl get configmap

# View configmap
kubectl describe configmap my-config

# List secrets
kubectl get secrets

# View secret (base64 encoded)
kubectl get secret my-secret -o yaml

# Decode secret value
kubectl get secret my-secret -o jsonpath='{.data.password}' | base64 -d

# Create generic secret
kubectl create secret generic my-secret --from-literal=key=value

# Create from file
kubectl create secret generic my-secret --from-file=./path/to/file
```

## Resource Monitoring

```bash
# Get all resources
kubectl get all

# Get resource usage (requires metrics server)
kubectl top nodes

# Get pod resource usage
kubectl top pod

# Get pod resource usage in specific namespace
kubectl top pod -n my-namespace

# Get resource requests/limits
kubectl describe pod my-pod | grep -A 4 "Requests\|Limits"
```

## Advanced Queries

### Using Labels and Selectors

```bash
# List pods with specific label
kubectl get pods -l app=my-app

# List all resources with label
kubectl get all -l environment=production

# List pods matching multiple labels
kubectl get pods -l app=my-app,tier=frontend
```

### Output Formatting

```bash
# JSON output
kubectl get pod my-pod -o json

# YAML output
kubectl get pod my-pod -o yaml

# Custom columns
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase

# Wide format (additional columns)
kubectl get pods -o wide

# JSONPath query
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
```

## Debugging and Troubleshooting

```bash
# Get pod events
kubectl describe pod my-pod | grep -A 20 Events

# Check pod status conditions
kubectl get pod my-pod -o jsonpath='{.status.conditions[*]}'

# Get pod resource limits
kubectl describe pod my-pod | grep -E "Limits|Requests"

# Check container restart count
kubectl get pod my-pod -o jsonpath='{.status.containerStatuses[0].restartCount}'

# Get pod IP
kubectl get pod my-pod -o jsonpath='{.status.podIP}'

# List events in namespace
kubectl get events

# Watch events in real-time
kubectl get events --watch
```

## Resource Lifecycle

```bash
# Create resource from YAML
kubectl create -f deployment.yaml

# Apply resource (create or update)
kubectl apply -f deployment.yaml

# Apply directory
kubectl apply -f ./my-manifests/

# Delete resource
kubectl delete pod my-pod

# Delete by label
kubectl delete pods -l app=my-app

# Delete resource by YAML
kubectl delete -f deployment.yaml

# Force delete (for stuck pods)
kubectl delete pod my-pod --grace-period=0 --force
```

## Useful One-Liners

```bash
# Get all pods and their IPs
kubectl get pods -o wide | awk '{print $1, $6}'

# Get all container images in cluster
kubectl get pods -o jsonpath="{..image}" | tr -s '[[:space:]]' '\n' | sort | uniq

# Find pods requesting most CPU
kubectl top pod --all-namespaces | sort -k 2 -n -r | head -10

# Find pods requesting most memory
kubectl top pod --all-namespaces | sort -k 3 -n -r | head -10

# Export all resources
kubectl get all -A -o yaml > cluster-backup.yaml

# Restart all pods in namespace
kubectl rollout restart deployment -n my-namespace --all
```
