# AWS CLI ECS Cheatsheet

## List ECS Clusters

```bash
aws ecs list-clusters | jq -r '.clusterArns[]'
```

## Configure Variables

```bash
export CLUSTER_NAME="my-cluster"
export SERVICE_NAME="my-service"
export TASK_ID="arn:aws:ecs:region:account-id:task/cluster/task-id"
```

## List Resources

### List Container Instances

```bash
aws ecs list-container-instances --cluster $CLUSTER_NAME | jq -r '.containerInstanceArns[]'
```

### List Services

```bash
aws ecs list-services --cluster $CLUSTER_NAME | jq -r '.serviceArns[]'
```

### List Tasks in Service

```bash
aws ecs list-tasks --cluster $CLUSTER_NAME --service-name $SERVICE_NAME | jq -r '.taskArns[]'
```

## Describe Resources

### Get Service Details

```bash
# Get running count
aws ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_NAME | jq -r '.services[].runningCount'

# Get task definition
aws ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_NAME | jq -r '.services[].taskDefinition'

# Get deployments
aws ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_NAME | jq -r '.services[].deployments'

# Get latest events
aws ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_NAME | jq -r '.services[].events[0]'
```

### Get Task Details

```bash
# Get last status
aws ecs describe-tasks --tasks $TASK_ID --cluster $CLUSTER_NAME | jq -r '.tasks[].lastStatus'

# Get stop code
aws ecs describe-tasks --tasks $TASK_ID --cluster $CLUSTER_NAME | jq -r '.tasks[].stopCode'

# Get stop reason
aws ecs describe-tasks --tasks $TASK_ID --cluster $CLUSTER_NAME | jq -r '.tasks[].stoppedReason'

# Full task info
aws ecs describe-tasks --tasks $TASK_ID --cluster $CLUSTER_NAME | jq '.tasks[0]'
```

## Update ECS Service

```bash
# Scale service to desired count
aws ecs update-service --cluster $CLUSTER_NAME --service $SERVICE_NAME --desired-count 3

# Update task definition
aws ecs update-service --cluster $CLUSTER_NAME --service $SERVICE_NAME \
  --task-definition my-task:2 --force-new-deployment

# Enable/disable service
aws ecs update-service --cluster $CLUSTER_NAME --service $SERVICE_NAME --desired-count 0
```

## Troubleshooting

### Task Failed to Start

```bash
# Check task stopping reason
aws ecs describe-tasks --tasks $TASK_ID --cluster $CLUSTER_NAME | \
  jq '.tasks[] | {lastStatus, stoppedReason, stoppingAt}'

# Check CloudWatch logs (if container agent logs available)
aws logs tail /ecs/my-service --follow
```

### Service Not Progressing

```bash
# Check if there are any service events
aws ecs describe-services --cluster $CLUSTER_NAME --services $SERVICE_NAME | \
  jq '.services[0].events[:5]'

# Verify task definition exists and is accessible
aws ecs describe-task-definition --task-definition my-task:1 | jq '.taskDefinition.status'
```
