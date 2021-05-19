---
title: "2.4 Setup and automate static website hosting"
chapter: true
weight: 16
---

# Automate deploy with CodeDeploy on test coverage pass

Now let's use CircleCI's AWS Codedeploy integration to continuously deploy updates to our app when our tests pass. 

## Step 1. Instell AWS Code Deploy Executer

We're going to leverage a script called the AWS Code Deploy Exectuter to deploys applications with the AWS Code Deploy service. 

The script uses the AWS CLI for underlying commands and extends functionality to provide common requirements for applications being deployed including compression, encryption, bucket revision limiting, monitoring, and more.

We can install this script with `npm` like this:

```bash
npm install aws-code-deploy --save-dev
```

The file can then be executed directly: ./node_modules/aws-code-deploy/bin/aws-code-deploy.sh

## Step 2. Setup environment variables

Environment variables are used to control the deployment actions. Which we will need to set up in our CircleCI CircleCI Enviroment Variables control panel. 

![](..\static\images\envci.png)

A brief summary of the variables is listed in the table below. Full descriptions with recommendations can be found by searching the readme for the variable name.

| Variable                                          | Required | Description                                                |
| :-------------------------------------------------| :------- | :----------------------------------------------------------|
| `AWS_CODE_DEPLOY_KEY`                             | No       | If specified, sets the AWS key id |
| `AWS_CODE_DEPLOY_SECRET`                          | No       | If specified, sets the AWS secret key |
| `AWS_CODE_DEPLOY_REGION`                          | No       | If specified, sets the AWS region |
| `AWS_CODE_DEPLOY_APPLICATION_NAME`                | **Yes**  | Application name. If it does not exist, will create. |
| `AWS_CODE_DEPLOY_DEPLOYMENT_GROUP_NAME`           | **Yes**  | Deployment group name. If it does not exist, will create. |
| `AWS_CODE_DEPLOY_DEPLOYMENT_CONFIG_NAME`          | No       | Deployment config name. By default: _CodeDeployDefault.OneAtATime_ |
| `AWS_CODE_DEPLOY_MINIMUM_HEALTHY_HOSTS`           | No       | The minimum number of healthy instances during deployment. By default: _type=FLEET_PERCENT,value=75_ |
| `AWS_CODE_DEPLOY_SERVICE_ROLE_ARN`                | No       | Service role arn giving permissions to use Code Deploy when creating a deployment group |
| `AWS_CODE_DEPLOY_EC2_TAG_FILTERS`                 | No       | EC2 tags to filter on when creating a deployment group |
| `AWS_CODE_DEPLOY_AUTO_SCALING_GROUPS`             | No       | Auto Scaling groups when creating a deployment group |
| `AWS_CODE_DEPLOY_APP_SOURCE`                      | **Yes**  | The source directory used to create the deploy archive or a pre-bundled tar, tgz, or zip |
| `AWS_CODE_DEPLOY_APP_BUNDLE_TYPE`                 | No       | Deploy bundle type when created from source directory (`tgz` or `zip`). Default: `zip`)
| `AWS_CODE_DEPLOY_S3_BUCKET`                       | **Yes**  | The name of the S3 bucket to deploy the revision |
| `AWS_CODE_DEPLOY_S3_KEY_PREFIX`                   | No       | A prefix to use for the revision bucket key |
| `AWS_CODE_DEPLOY_S3_FILENAME`                     | **Yes**  | The destination name within S3. |
| `AWS_CODE_DEPLOY_S3_LIMIT_BUCKET_FILES`           | No       | Number of revisions to limit. If 0, unlimited. Default = `0` |
| `AWS_CODE_DEPLOY_S3_SSE`                          | No       | If specified and `true` will ensure the CodeDeploy archive is stored in S3 with Server Side Encryption (SSE) |
| `AWS_CODE_DEPLOY_REVISION_DESCRIPTION`            | No       | A description that is stored within AWS Code Deploy that stores information about the specific revision |
| `AWS_CODE_DEPLOY_DEPLOYMENT_DESCRIPTION`          | No       | A description that is stored within AWS Code Deploy that stores information about the specific deployment           |
| `AWS_CODE_DEPLOY_OUTPUT_STATUS_LIVE`              | No       | Boolean `true\|false` that specifies whether the deployment status should use a single line showing live status. 

## Add deployment to your `config.yml`

Let's add the required content to our workflows now:

```YAML
deployment:
  production:
    branch: master
    commands:
      - bash vendor/bin/aws-code-deploy.sh
```

## Setup manual approval

For times when you’d prefer to keep manual approvals in place, you can easily configure the manual approval process by adding to your workflow a special job containing a `type: approval` entry:

```YAML
version: 2
release-branch-workflow:
  jobs:
    - build
    - request-testing:
        type: approval
        requires:
          - build
    - deploy-aws:
        requires:
          - request-testing
```