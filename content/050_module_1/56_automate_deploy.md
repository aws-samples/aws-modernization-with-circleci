---
title: "2.4 Set up and automate static website hosting"
chapter: true
weight: 16
---

Now we are going to use CircleCI's AWS CodeDeploy integration to continuously deploy updates to our app when our tests pass. AWS CodeDeploy is a service that allows teams to easily automate application deployments to Amazon EC2 instances, on-premises instances, serverless Lambda functions and other services. 

## Step 1 &mdash; Install AWS Code Deploy Executor

We are going to use a script called the AWS CodeDeploy Executor to deploy applications with the AWS Code Deploy service. 

The script uses the AWS CLI for underlying commands and extends functionality to provide common requirements for applications being deployed including compression, encryption, bucket revision limiting, monitoring, and more.

We can install this script with `npm` like this:

```bash
npm install aws-code-deploy --save-dev
```

The file can then be executed directly: ./node_modules/aws-code-deploy/bin/aws-code-deploy.sh

## Step 2 &mdash; Setup environment variables

Environment variables are used to control the deployment actions. We can set these up in our CircleCI Environment Variables control panel.

![Variable Config](/images/envci.png)

A brief summary of the variables is listed in the table following. Full descriptions with recommendations can be found by searching the readme for the variable name.

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

## Step 3 &mdash; Add deployment to your `config.yml`

Add the required content to our workflows now:

```YAML
deployment:
  production:
    branch: master
    commands:
      - bash vendor/bin/aws-code-deploy.sh
```

## Step 4 &mdash; Setup manual approval

There are times when you prefer to keep manual approvals in place. You can easily configure a manual approval process by changing your workflow to include a special job containing a `type: approval` entry:

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

In this section, we've learned how to leverage AWS CodeDeploy to automate the process of deploying our application to our environment. In the next section, we're going to learn how to setup static hosting in AWS. 