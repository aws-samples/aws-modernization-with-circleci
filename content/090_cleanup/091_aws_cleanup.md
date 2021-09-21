---
title: "AWS Cleanup"
chapter: true
draft: false
weight: 91
---

## Delete Cloud9 Instance
Now that we've deleted the ECS cluster, we can now safely delete our Cloud9 EC2 instance:

1. Go to your [EC2 dashbord](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:) 
1. Select the EC2 instance with the name "circleci-workshop"
1. Go to the actions drop down menu and select delete instance
