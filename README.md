# aws-ecs-github-actions-deployment

## Step 1: Create an ECR Repository
```bash
aws ecr create-repository --repository-name <MY_REPO_NAME>
```

## Step 2: Login in to ECR
```bash
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AccountID>.dkr.ecr.us-east-1.amazonaws.com
```