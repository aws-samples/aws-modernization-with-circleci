---
title: "3.3 Setup Elastic Beanstalk environment"
chapter: true
weight: ADD WEIGHT
---

# Create a bash script to create the eb config file

Let's add a `bash` script for creating the `eb` config file:

**./setup-eb.sh**

```bash
set -x
set -e

mkdir /home/ubuntu/.aws
touch /home/ubuntu/.aws/config
chmod 600 /home/ubuntu/.aws/config
echo "[profile eb-cli]" > /home/ubuntu/.aws/config
echo "aws_access_key_id=$AWS_ACCESS_KEY_ID" >> /home/ubuntu/.aws/config
echo "aws_secret_access_key=$AWS_SECRET_ACCESS_KEY" >> /home/ubuntu/.aws/config
```

# Create the EB Cli config file

`eb init` will create this file for you. However, if you don't want to run it, you can simply create and configure this file manualy:

**./elasticbeanstalk/config.yml**

```YAML
branch-defaults:
  master:
    environment: you-environment-name
global:
  application_name: your-application-name
  default_ec2_keyname: ec2-key-pair-name
  default_platform: 64bit Amazon Linux 2015.03 v1.4.3 running Ruby 2.2 (Puma)
  default_region: sa-east-1
  profile: eb-cli
  sc: git
```