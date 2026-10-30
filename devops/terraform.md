# Terraform Cheatsheet

## Project Setup

```bash
# Initialize Terraform working directory
terraform init

# Upgrade to latest provider versions
terraform init -upgrade

# Validate configuration
terraform validate

# Format configuration files
terraform fmt
terraform fmt -recursive  # Format all subdirectories
```

## Planning and State

```bash
# Generate execution plan
terraform plan

# Save plan to file
terraform plan -out=tfplan

# Show specific plan
terraform show tfplan

# Refresh state without applying
terraform refresh

# Show current state
terraform state list
terraform state show aws_instance.example

# Pull latest state
terraform state pull > state.json

# Remove resource from state (without destroying)
terraform state rm aws_instance.example

# Move resource in state
terraform state mv aws_instance.old aws_instance.new
```

## Apply and Destroy

```bash
# Apply changes
terraform apply

# Apply from saved plan
terraform apply tfplan

# Auto-approve without prompt
terraform apply -auto-approve

# Destroy all resources
terraform destroy

# Destroy specific resource
terraform destroy -target aws_instance.example

# Auto-approve destroy
terraform destroy -auto-approve
```

## Variables and Outputs

```bash
# Set variable via CLI
terraform apply -var="instance_type=t2.micro"

# Use variable file
terraform apply -var-file="prod.tfvars"

# Show outputs
terraform output

# Show specific output value
terraform output instance_ip

# Show output as JSON
terraform output -json
```

## Workspaces

```bash
# List workspaces
terraform workspace list

# Create workspace
terraform workspace new staging

# Select workspace
terraform workspace select staging

# Delete workspace
terraform workspace delete staging

# Show current workspace
terraform workspace show
```

## Debugging and Inspection

```bash
# Enable debug logging
TF_LOG=DEBUG terraform apply

# Set log level (TRACE, DEBUG, INFO, WARN, ERROR)
TF_LOG=WARN terraform plan

# Save logs to file
TF_LOG=DEBUG TF_LOG_PATH=terraform.log terraform apply

# Pretty-print JSON output
terraform show -json | jq .

# Get graph of resources (requires graphviz)
terraform graph | dot -Tsvg > graph.svg
```

## Module Management

```bash
# Initialize and download modules
terraform init

# Download modules only
terraform get

# Update modules to latest
terraform get -update

# Show module source
terraform providers
```

## Linting and Testing

```bash
# Format and check syntax
terraform fmt -check

# Validate configuration
terraform validate

# Static analysis (requires tflint)
tflint

# Security scanning (requires checkov)
checkov -d .
```

## State Management

```bash
# List all resources
terraform state list

# Show resource details
terraform state show aws_instance.example

# Move between states
terraform state mv aws_instance.old aws_instance.new

# Remove from state without destroying
terraform state rm aws_instance.example

# Pull state for inspection
terraform state pull > backup.tfstate

# Push state (use with caution)
terraform state push backup.tfstate

# Lock state (for safe operations)
terraform state unlock  # Unlock if stuck
```

## Remote State

```bash
# Configure remote state backend
# In terraform code:
terraform {
  backend "s3" {
    bucket         = "my-bucket"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-lock"
  }
}

# Initialize with remote backend
terraform init

# Show remote state
terraform state pull

# Migrate state from local to remote
terraform init  # Prompts for migration
```

## Useful Patterns

### Targeting Specific Resources

```bash
# Apply only specific resource
terraform apply -target aws_instance.example

# Apply resource and dependencies
terraform apply -target aws_security_group.example

# Plan multiple resources
terraform plan -target aws_instance.example -target aws_instance.another
```

### Conditional Application

```bash
# Apply with variable conditions
terraform apply -var="create_database=true"

# Plan with complex filter
terraform plan -target 'aws_instance.*.web[0]'
```

### Useful Environment Variables

```bash
# Disable state locking
TF_SKIP_LOCK=true terraform apply

# Custom backend config
terraform init -backend-config="bucket=my-bucket"

# Target parallelism
terraform apply -parallelism=5
```

## Common Errors and Solutions

```bash
# State lock issues
terraform force-unlock LOCK_ID

# Provider version conflicts
terraform init -upgrade

# Missing variables
terraform apply -var-file="vars.tfvars"

# Inconsistent state
terraform refresh
terraform state pull > backup.tfstate

# Resource already exists
terraform import aws_instance.example i-1234567
```

## Useful Commands Summary

| Command | Purpose |
|---------|---------|
| `terraform init` | Initialize directory |
| `terraform plan` | Preview changes |
| `terraform apply` | Apply changes |
| `terraform destroy` | Destroy resources |
| `terraform validate` | Check configuration |
| `terraform fmt` | Format code |
| `terraform state` | Manage state |
| `terraform import` | Import existing resources |
| `terraform output` | Display outputs |
| `terraform workspace` | Manage workspaces |
