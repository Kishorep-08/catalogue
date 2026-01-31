### Plugins

* Stage view
This plugin shows the pipeline status in stages with colours. It is useful for quickly identifying where exactly our pipeline failed.

**Status colors:**
- 🟢 Green – Success  
- 🔴 Red – Failure  

* Pipeline Utility steps
  This plugin add very important functionalities like reading the content of the file (Json, csv, xml ...). We used this plugin to read a Json file.
  The function we used was readJSON.

* AWS steps
  This plugin add functionality with which jenkins can talk to AWS API server. We can do any type of task on AWS using this plugin. 
  We used this plugin to configure AWS credentials to connect and push our image artifact to ECR.


aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
docker build -t aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
docker push aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag