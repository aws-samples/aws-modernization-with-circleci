---
title: "4.2 Build an AWS ECR Job using Terraform"
chapter: true
weight: 12
---

## AWS Elastic Container Registry

In this workshop, you will create a [Docker Image][7] based on the project application. This Docker Image must be uploaded/pushed to a [Docker Registry][7], which is a centrally managed catalog of images, that can be deployed to target environments and infrastructures. AWS provides the [AWS Elastic Container Registry (ECR)][2] service which enables developers to create private and public image registries. In this section you will create a new [ECR Public Repository][8], using Terraform, to host your project Docker image.

## Pickup from previous module: DevSecOps

You briefly learned about Infrastructure as Code and Terraform in previous sections, so now it time to build a new job into our pipeline that creates a new AWS Public ECR.

At the end of the last module, your config.yml file was left in the state shown in the code snippet below. If it's not that's ok, go ahead and copy the snippet below and paste it into the file. Its important to to ensure your config.yml is identical to the code snippet below before progressing further:

{{<highlight yaml>}}
version: 2.1
orbs:
  node: circleci/node@4.2.0
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  terraform: circleci/terraform@2.0.0
jobs:
  run_tests:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - node/install-packages:
          override-ci-command: npm install
          cache-path: ~/project/node_modules
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mocha-junit-reporter --reporter-options mochaFile=./test/test-results.xml
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_test_results:
          path: test/
      - store_artifacts:
          path: test-results          
  scan_app:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - run:
          name: Snyk Scan Application files 
          command: npm install 
      - snyk/scan:
          fail-on-issues: false
          monitor-on-build: false

{{</highlight>}}

## Build a create ECR job

As previously mentioned, the project application will be distributed in a Docker image, but before you can create the image, you must have a container registry available to host the image. In this section, you will learn how to create this registry using terraform from within a CI/CD job.

Now that your config.yml is ready, you can build a create ECR job. The code snippet below demonstrates how to define and provision a job that creates a new AWS Public ECR within your CI/Cd pipeline.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  create_ecr_repo:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - run:
          name: Create .terraformrc file locally
          command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
      - terraform/install:
          terraform_version: "0.14.10"
          arch: "amd64"
          os: "linux"
      - run:
          name: Create ECR Repo
          command: echo 'Create AWS ECR Repo with Terraform'
      - terraform/init:
          path: ./terraform/ecr
      - terraform/apply:
          path: ./terraform/ecr
      - run: 
          name: "Retrieve ECR URIs"
          command: |
            cd ./terraform/ecr
            mkdir -p /tmp/ecr/
            terraform init
            echo 'export ECR_NAME='$(terraform output ECR_NAME) >> /tmp/ecr/ecr_envars
            export ECR_PUBLIC_URI=$(terraform output ECR_URI)
            echo 'export ECR_PUBLIC_URI='$ECR_PUBLIC_URI >> /tmp/ecr/ecr_envars
            echo 'export ECR_URL='$(echo ${ECR_PUBLIC_URI:1:-1} | cut -d"/" -f1,2) >> /tmp/ecr/ecr_envars
      - persist_to_workspace:
          root: /tmp/ecr/
          paths:
            - "*"

{{</highlight>}}

You should already be familiar with the *docker:*, *step:s* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

**command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc** creates a required file, that is used by the Terraform CLI to authenticate your Terraform Cloud credentials and grant access to interact with the service. Notice that the *$TERRAFORM_TOKEN* environment variable, that you created earlier, specified and represents a protected way of referencing sensitive data in the config.yml file. Using environment variables in this manner protects against exposing sensitive data in pipeline configurations.

**- terraform/install:** block leverages the [Terraform Orb][9] to install the appropriate Terraform CLI binary into the executor. The cli will be required to execute Terraform code inthe pipeline.

**- terraform/init:** block leverages the [Terraform Orb][9] to perform a [terraform init][10] command, which initializes the project in the *terraform/ecr/* directory.

**- terraform/apply:** block leverages the [Terraform Orb][9] to perform a [terraform apply][11] command, which executes the code in the *terraform/ecr/* directory.

**- run: name: "Retrieve ECR URIs"** block has multiple commands listed in the **command:** block and we'll address them individually:

- **cd ./terraform/ecr**
    - Change directory into the *./terraform/ecr* directory
- **mkdir -p /tmp/ecr/**
    - Create a new directory that will hold valuable files that will be shared between jobs.
- **terraform init**
    - initialize the Terraform project
- **echo 'export ECR_NAME='$(terraform output ECR_NAME) >> /tmp/ecr/ecr_envars**
    - Use the *$(terraform output ECR_NAME)* command to get the ECR name assigned using Terraform and save it a file as an environment variable to be used between jobs
- **export ECR_PUBLIC_URI=$(terraform output ECR_URI)**
    - Use the *$(terraform output ECR_URI)* command to create a local variable and assign the  ECR URI value.
- **echo 'export ECR_PUBLIC_URI='$ECR_PUBLIC_URI >> /tmp/ecr/ecr_envars**
    - Saves the assigned *$ECR_PUBLIC_URI* value to the */tmp/ecr/ecr_envars* file 
- **echo 'export ECR_URL='$(echo ${ECR_PUBLIC_URI:1:-1} | cut -d"/" -f1,2) >> /tmp/ecr/ecr_envars**
    - Saves a parsed version of the *$ECR_PUBLIC_URI* to the */tmp/ecr/ecr_envars* file

These commands essentially saves data to a file, and that data will be used in subsequent pipeline jobs. 

**- persist_to_workspace:** is a special key that represents a [CircleCI Workspace][13]. When a workspace is declared in a job, files and directories can be added to it. Each addition creates a new layer in the workspace filesystem. Downstream jobs can then use this workspace for their own needs or add more layers on top. Workspaces are not shared between pipeline runs. The only time a workspace can be accessed after the pipeline has run is when a workflow is rerun within the 15 day limit.

Congratulations! You've just learned how to create a CI/CD pipeline job that leverages Terraform to create a new AWS Public ECR from within your pipeline.

In the next section, you'll learn how to create a Docker image and upload it to your newly created ECR.

<!-- URL Links index -->
[1]: https://www.terraform.io
[2]: https://aws.amazon.com/ecr/
[3]: https://aws.amazon.com/apprunner/
[4]: https://www.terraform.io/docs/cloud/
[5]: https://www.terraform.io/docs/cli/index.html
[6]: /040_circleci_setup/43_terraform_cloud_token.html
[7]: https://docs.docker.com/get-started/overview/
[8]: https://docs.aws.amazon.com/AmazonECR/latest/public/public-repositories.html
[9]: https://circleci.com/developer/orbs/orb/circleci/terraform
[10]: https://www.terraform.io/docs/cli/commands/init.html
[11]: https://www.terraform.io/docs/cli/commands/apply.html
[12]: https://circleci.com/docs/2.0/persist-data/
[13]: https://circleci.com/docs/2.0/persist-data/#using-workspaces