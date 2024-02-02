Terraform Metrics:
Terraform Plan Execution Time:

Monitor the time taken for Terraform plan operations to execute.
Example query: terraform_operation_duration_seconds{operation="plan"}
Terraform Apply Execution Time:

Monitor the time taken for Terraform apply operations to execute.
Example query: terraform_operation_duration_seconds{operation="apply"}
Terraform State Locks:

Monitor the number of Terraform state locks.
Example query: terraform_state_locks


