### Plugins

* Stage view
* Pipeline Utility steps
* AWS steps


aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
docker build -t aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag
docker push aws_account_id.dkr.ecr.region.amazonaws.com/my-repository:tag