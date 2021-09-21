---
title: "5.4 Build an ECS Destroy Job"
chapter: true
weight: 14
---

## Destroy Amazon ECS Cluster

The previous pipeline jobs created infrastructures and resources that the pipeline required for execution. Now that all of the essential pipeline jobs have been defined, you need to define pipeline jobs that will decommission and destroy the resources created in previous jobs.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  destroy_aws_ecs:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout
      - run:
          name: Create .terraformrc file locally
          command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
      - terraform/install:
          terraform_version: "1.0.2"
          arch: "arm64"
          os: "linux"
      - terraform/init:
          path: ./terraform/ecs
      - terraform/plan:
          path: ./terraform/ecs
      - terraform/destroy:
          path: ./terraform/ecs
{{</highlight>}}

You should already be familiar with the *machine:*, *steps:* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

**command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc** creates a required file, that is used by the Terraform CLI to authenticate your Terraform Cloud credentials and grant access to interact with the service. Notice that the *$TERRAFORM_TOKEN* environment variable, that you created earlier, specified and represents a protected way of referencing sensitive data in the config.yml file. Using environment variables in this manner protects against exposing sensitive data in pipeline configurations.

**- terraform/install:** block leverages the [Terraform Orb][9] to install the appropriate Terraform CLI binary into the executor. The cli will be required to execute Terraform code in the pipeline.

**- terraform/init:** block leverages the [Terraform Orb][9] to perform a [terraform init][10] command, which initializes the project.

**- terraform/destroy:** block leverages the [Terraform Orb][9] to perform a [terraform destroy][14] command, which destroys the infrastructures and resources created by previous jobs. In this case the destroy command is cleaning the state of this project.

Congratulations! You've created a job to destroy the App Runner service created earlier. You have also built the last pipeline job for this project. Next you'll learn about [CircleCI Workflows][15] and how to orchestrate all of the awesome jobs you've defining throughout this workshop.

In the next section, you will learn about CircleCI Workflows.


<!-- URL Links index -->
[1]: https://www.terraform.io
[2]: https://aws.amazon.com/ecs/
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
[15]: https://circleci.com/docs/2.0/workflows/#overview