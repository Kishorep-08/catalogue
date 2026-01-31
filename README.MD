## Plugins

### 1. Stage view
This plugin shows the pipeline status in stages with colours. It is useful for quickly identifying where exactly our pipeline failed.

**Status colors:**
- 🟢 Green – Success  
- 🔴 Red – Failure  

### 2. Pipeline Utility steps
  This plugin add very important functionalities like reading the content of the file (Json, csv, xml ...). We used this plugin to read a Json file.
  The function we used is readJSON.

### 3. AWS Steps Plugin
Enables Jenkins to interact with **AWS APIs**.

**Capabilities:**
- Configure AWS credentials
- Perform AWS operations from Jenkins pipelines

**Usage in this project:**
- Configured AWS credentials
- Connected to AWS
- Pushed Docker image artifacts to **Amazon ECR**

---

aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
docker build -t aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
docker push aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag