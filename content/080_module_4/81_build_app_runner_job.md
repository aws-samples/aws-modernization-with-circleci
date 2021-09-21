---
title: "5.2 Build an AWS App Runner Job"
chapter: true
weight: 11
hidden: true
---

## Build App Runner job

In this section, you will learn how to provision a new [AWS App Runner][3] service, using Terraform, and deploy the project application image to it from within a CI/CD job.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  create_deploy_app_runner:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - attach_workspace:
          at: /tmp/ecr/
      - terraform/install:
          terraform_version: "0.14.10"
          arch: "amd64"
          os: "linux"
      - run:
          name: Create and Deploy App Runner
          command: |
            source /tmp/ecr/ecr_envars
            cd ./terraform/app-runner/
            echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
            terraform init
            terraform apply -var image_name=$ECR_PUBLIC_URI \
              -var image_tag=$TAG \
              -auto-approve
            echo 'export ENDPOINT='$(terraform output apprunner_service_url) >> /tmp/ecr/ecr_envars
      - persist_to_workspace:
          root: /tmp/ecr/
          paths:
            - "*"
{{</highlight>}}

You should already be familiar with the *docker:*, *steps:* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

**- attach_workspace:** block attaches the [CircleCI Workspace][3] that you created in the previous job. This command will provide the job access to the data saved earlier.

**- terraform/install:** block leverages the [Terraform Orb][9] to install the appropriate Terraform CLI binary into the executor. The cli will be required to execute Terraform code in the pipeline.

**- run: name: Create and Deploy App Runner** block has multiple commands listed in the **command:** block and we'll address them individually:

- **source /tmp/ecr/ecr_envars**
    - sources the */tmp/ecr/ecr_envars* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc** creates a required file, that is used by the Terraform CLI to authenticate your Terraform Cloud credentials and grant access to interact with the service. Notice that the *$TERRAFORM_TOKEN* environment variable, that you created earlier, specified and represents a protected way of referencing sensitive data in the config.yml file. Using environment variables in this manner protects against exposing sensitive data in pipeline configurations.
- **terraform init** executes a [terraform init][10] command, which initializes the project in the *terraform/ecr/* directory.
- **terraform apply -var image_name=$ECR_PUBLIC_URI \
    -var image_tag=$TAG \
    -auto-approve**
  - This command executes the Terraform code in the *terraform/ecr/* directory. This particular Terraform is a bit different from the previous Terraform apply commands you created earlier. This command leverages [Terraform Variables][14], which enables you to specify the Docker image and tag to deploy.
- **echo 'export ENDPOINT='$(terraform output apprunner_service_url) >> /tmp/ecr/ecr_envars**
  - Saves the App Runner service's public URL from the Terraform *apprunner_service_url* output into the environment variable  **$ECR_PUBLIC_URI* which is finally saved into to the */tmp/ecr/ecr_envars* file for future use in subsequent jobs.

**- persist_to_workspace:** is a special key that represents a CircleCI Workspace. When a workspace is declared in a job, files and directories can be added to it. Each addition creates a new layer in the workspace filesystem. Downstream jobs can then use this workspace for their own needs or add more layers on top. Workspaces are not shared between pipeline runs. The only time a workspace can be accessed after the pipeline has run is when a workflow is rerun within the 15 day limit.

This job demonstrates a *Continuous Deployment* because it is integrated into and executed by a job within your CI/CD pipeline and it will automatically deploy updated code to the App Runner service anytime the code is committed and pushed to the upstream project code repository.


Now that you've learned how to build *Continuos Deployment* jobs, jump over to the next section where you will learn how to test the deployment to ensure the deployment and application are functioning as expected.

<!-- URL Links index -->
[[1]: https://www.terraform.io
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
[14]: https://www.terraform.io/docs/language/values/variables.html