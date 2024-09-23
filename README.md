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

## Step 9: Create ECS Task Definition
```bash
aws ecs register-task-definition --cli-input-json "$(cat <PATH_TO_TASK_DEFINiTION>)"
```