---
title: "5.4 Build a ECR Destroy Job"
chapter: true
weight: 13
hidden: true
---

## Build ECR Destroy job

The previous pipeline jobs created infrastructures and resources that the pipeline required for execution. Now that all of the essential pipeline jobs have been defined, you need to define pipeline jobs that will decommission and destroy the resources created in previous jobs.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  destroy_ecr:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - aws-cli/install
      - aws-cli/setup:
          aws-access-key-id: AWS_ACCESS_KEY_ID
          aws-secret-access-key: AWS_SECRET_ACCESS_KEY
      - attach_workspace:
          at: /tmp/ecr/
      - run:
          name: Create .terraformrc file locally
          command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc && cat $HOME/.terraformrc
      - terraform/install:
          terraform_version: "0.14.10"
          arch: "amd64"
          os: "linux"
      - run:
          name: Prep AWS ECR Destroy
          command: |
            source /tmp/ecr/ecr_envars
            sudo apt-get update && sudo apt-get install -y groff groff-base less wget
            aws ecr-public delete-repository --region us-east-1 --repository-name $ECR_NAME --force
      - terraform/init:
          path: ./terraform/ecr
      - terraform/destroy:
          path: ./terraform/ecr
{{</highlight>}}

You should already be familiar with the *docker:*, *step:s* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

**- aws-cli/install** leverages the [AWS CLI Orb][4] which provides the job access to the AWS CLI to execute AWS centric commands. 

**- aws-cli/setup:** leverages the [AWS CLI Orb][4] and its *setup* command to initialize the AWS cli to access AWS services with the AWS credentials defined in the *$AWS_ACCESS_KEY_ID* and *$AWS_SECRET_ACCESS_KEY* project environment variables defined earlier.

**- attach_workspace:** block attaches the CircleCI Workspace that you created in the previous job. This command will provide the job access to the data saved earlier.

**command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc** creates a required file, that is used by the Terraform CLI to authenticate your Terraform Cloud credentials and grant access to interact with the service. Notice that the *$TERRAFORM_TOKEN* environment variable, that you created earlier, specified and represents a protected way of referencing sensitive data in the config.yml file. Using environment variables in this manner protects against exposing sensitive data in pipeline configurations.

**- terraform/install:** block leverages the [Terraform Orb][9] to install the appropriate Terraform CLI binary into the executor. The cli will be required to execute Terraform code inthe pipeline.

**- run: name: Prep AWS ECR Destroy** block has multiple commands listed in the **command:** block and we'll address them individually:

- **source /tmp/ecr/ecr_envars**
    - sources the */tmp/ecr/ecr_envars* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **sudo apt-get update && sudo apt-get install -y groff groff-base less wget**
    - installs some dependencies required by the aws-cli
- **aws ecr-public delete-repository --region us-east-1 --repository-name $ECR_NAME --force**
    - this is an aws-cli command that deletes the entire ECR repository created in previous jobs. This demonstrates how to leverage the aws-cli tool to execute commands on AWS resources.

**- terraform/init:** block leverages the [Terraform Orb][9] to perform a [terraform init][10] command, which initializes the project.

**- terraform/destroy:** block leverages the [Terraform Orb][9] to perform a [terraform destroy][14] command, which destroys the infrastructures and resources created by previous jobs. In this case the destroy command is cleaning the state of this project.

Now that you've created a job to destroy the AWS ECR resources, jump over to teh next section where you will build a destroy job job to terminate the App Runner service created earlier.

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
[14]: https://www.terraform.io/docs/cli/commands/destroy.html