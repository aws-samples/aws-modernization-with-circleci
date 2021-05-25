---
title: "3.4 Set up Elastic Beanstalk environment"
chapter: true
weight: 16
---

## Step 1 &mdash; Create the `eb` config file

Add a `bash` script for creating the `eb` config file:

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

## Step 2 &mdash; Create the Elastic Beanstalk ClI config file

`eb init` will create this file for you. However, if you don't want to run it, you can simply create and configure this file manually:

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

That's it for this module! Let's recap what we've learned:

1. In [Section 3.1](/060_module_2/60_create_project.html), we created our initial project structure.
2. Next, in [Section 3.2](/060_module_2/62_config-variables.html), we configured the variables that EB needed to operate
3. In [Section 3.3](/060_module_2/64_automate_deploy.html), we learned to leverage the `awsebcli` tool and implement it as a script in our CircleCI configuration
4. In this section, we made this final configs to Elastic Beanstalk nececcary to automate our deployment.

Let's move on to the next module where we will clean up what we've ust done!