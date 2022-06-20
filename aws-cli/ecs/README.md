# AWS CLI ECS Cheatsheet

## List ECS Clusters
```
$ aws ecs list-clusters | jq -r '.clusterArns[]
```

## Configure Variable for Cluster Name
```
export cluster_name = cluster_name
```

## List ECS Instances
```
aws ecs list-container-instances --cluster $cluster_name | jq -r '.containerInstanceArns[]'
```

## List ECS Services
```
aws ecs list-services --cluster $cluster_name | jq -r '.serviceArns[]'
```

## Configure Variable for Service Name
```
export service_name = service_name
```

## Describe ECS Service
 Get the running count of containers for a given service:
```
aws ecs describe-services --cluster $cluster_name --services $service_name | jq -r '.services[].runningCount'
```

Get the task definition revision for a given service:
```
aws ecs describe-services --cluster $cluster_name --services $service_name | jq -r '.services[].taskDefinition'
```

## Describe ECS Task States
Check if the deployment info has accepted the new deployment:
```
aws ecs describe-services --cluster $cluster_name --services $service_name | jq -r '.services[].deployments'
```

If not, get the latest event info:
```
aws ecs describe-services --cluster $cluster_name --services $service_name | jq -r '.services[].events[0]'
```

## Configure Variable for Task ID
```
export task_id = task_id

```

Grab the task id and describe the info to get the last know status:
```
aws ecs describe-tasks --tasks $task_id --cluster $cluster_name  | jq -r '.tasks[].lastStatus'
```

Get the stop code:
```
aws ecs describe-tasks --tasks $task_id --cluster $cluster_name   | jq -r '.tasks[].stopCode'
```

Get the stop reason:
```
aws ecs describe-tasks --tasks $task_id --cluster $cluster_name   | jq -r '.tasks[].stoppedReason'
```

## Update ECS Service
Update ECS Service to 3 replicas:
```
aws ecs update-service --cluster $cluster_name --service $service_name --desired-count 3
```
