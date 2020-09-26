# This project creates a 3 tier highly available, scalable architecture backuped by immutable AMI, it includes following resources:
1. route53 type A record alias to application load balancer
2. application load balancer with valid SSL termination and http direct to https
3. web server tier instances in autoscaling group(multi-AZ) within public subnets
4. database tier instance(has option to enable backup instance) within private subnets
5. immutable AMI created by packer and archived as EBS snapshot


## Prerequiste
1. a configured aws IAM account (acess key/secret key) with admin previlleges 
2. a workable hosted zone and domain name in Route53
3. a issued SSL certificate in ACM linked to workable domain name

## Replace resources with your ones 
1. in route53.tf, search rfexpert.net, replace all with your registered domain name
2. in alb.tf, search certificate_arn, update with your issued SSL certificate that is linked to your registered domain name

## Build image using packer
```bash
cd packer
packer build -machine-readable packer.json
```
## Provision infrastructure 
```bash
cd ..
terraform init
terraform apply 
```
## Verify
in browser, go to www.abc.io (your domain):
1. verify that it's returning ip of connected instance, hit refresh to observe ip change between two different ip addresses.
2. verify that http connection will be redirect to https. 
3. verify that issued SSL certificate is in valid status

## Clean up 
```bash
terraform destroy # destroy all resources 
```
go to aws console:
1. manually deregister AMI created by packer 
2. manually delete EBS snapshot created by packer 





