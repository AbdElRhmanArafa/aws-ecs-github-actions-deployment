# aws-ecs-github-actions-deployment

## Step 1: Create an ECR Repository
```bash
aws ecr create-repository --repository-name <MY_REPO_NAME>
```

## Step 2: Login in to ECR
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AccountID>.dkr.ecr.us-east-1.amazonaws.com
```

## Step 3: Build Docker Image
```bash
docker build -t <MY_REPO_NAME> .
```

## Step 4: Tag Docker Image
```bash
docker tag <MY_REPO_NAME>:latest <AccountID>.dkr.ecr.us-east-1.amazonaws.com/<MY_REPO_NAME>:latest
```

## Step 5: Push Docker Image to ECR
```bash
docker push <AccountID>.dkr.ecr.us-east-1.amazonaws.com/<MY_REPO_NAME>:latest
```

## Step 6: Create ECS Cluster
```bash
aws ecs create-cluster --cluster-name <MY_CLUSTER_NAME>
```

## Step 7: Create CloudWatch log group
```bash
aws logs create-log-group --log-group-name <MY_LOG_GROUP_NAME>
```
## Step 8: Create IAM Role
 - use case : Elstic Container Service 
 - Choose a use case for the specified service : Elastic Container Service Task
 - Attach the following policies:
    - AmazonECSTaskExecutionRolePolicy
    - AmazonEC2ContainerRegistryReadOnly
    - CloudWatchLogsFullAccess
 - Create the task role following policy:
    - AmazonS3FullAccess

## Step 9: Create ECS Task Definition
```bash
aws ecs register-task-definition --cli-input-json "$(cat <PATH_TO_TASK_DEFINiTION>)"
```
## Step 10: Create Securtiy Group
```bash
aws ec2 create-security-group --group-name <NAME-sg>\
    --description "Allow traffic on port 80"\
    --vpc-id <VPC_ID>
aws ec2 authorize-security-group-ingress --group-name <NAME-sg> --protocol tcp --port 80 --cidr 0.0.0.0/0
```
## Step 11: Create ECS Service
- The service definition should have the task definition ARN and cluster ARN.
- The service definition should have the security group and subnets.
```bash
aws ecs create-service --cli-input-json "$(cat <PATH_TO_SERVICE_DEFINITION>)"
```
## Step 12: Create workflows and add secrets
- Create a new workflow file in the `.github/workflows` directory.