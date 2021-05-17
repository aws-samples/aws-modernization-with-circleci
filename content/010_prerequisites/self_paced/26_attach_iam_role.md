---
title: "3.2 Attach IAM Role"
chapter: true
weight: 22
---

# Attach the IAM role to your Workspace

1. Follow [this deep link to find your Cloud9 EC2 instance](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:search=aws-cloud9-circleci;sort=desc:launchTime)

2. Select the instance, then choose **Actions / Security / Modify IAM role**

    ![Attach IAM role](/images/20_prerequisites/attachIAMrole.png)

3. Choose the role that contains "BastionStack" in it.

    ![Bastion Instance Profile role](/images/20_prerequisites/bastionStackRole.png)

4. Click **Save**



