---
title: "5.2 Build an Amazon ECS Job"
chapter: true
weight: 11
---

## Build Amazon ECS job

In this section, you will learn how to provision a new [Amazon ECS cluster][2] service, using Terraform, and deploy the project application image to it from within a CI/CD job.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  deploy_aws_ecs:
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
      - run:
          name: Terraform apply
          command: |
            terraform -chdir=./terraform/ecs apply \
              -var docker_img_name=${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME} \
              -var docker_img_tag=0.1.<< pipeline.number >> \
              -auto-approve
            export ENDPOINT="$(terraform -chdir=./terraform/ecs output load_balancer_hostname)"
            mkdir -p /tmp/ecs/
            echo 'export ENDPOINT='${ENDPOINT} > /tmp/ecs/endpoint
      - persist_to_workspace:
          root: /tmp/ecs/
          paths:
            - "*"      
      - run: sleep 80
{{</highlight>}}

You should already be familiar with the *machine:*, *resource_class:*, *steps:* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:*  and orbs elements in this job.

**- run: name: Create .terraformrc file locally** block executes the following command:

- **echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc** creates a required file, that is used by the Terraform CLI to authenticate your Terraform Cloud credentials and grant access to interact with the service. Notice that the *$TERRAFORM_TOKEN* environment variable, that you created earlier, specified and represents a protected way of referencing sensitive data in the config.yml file. Using environment variables in this manner protects against exposing sensitive data in pipeline configurations.

**- terraform/install:** block leverages the [Terraform Orb][9] to install the appropriate Terraform CLI binary into the executor. The cli will be required to execute Terraform code in the pipeline.

**- terraform/init:** executes a [terraform init][10] orb command which initializes the project in the *terraform/ecs/* directory defined in the **path:** parameter.

**- terraform/plan:** executes the terraform *plan* orb command which creates a graph of the terraform execution plan.

**- run: name: Terraform apply** block has multiple commands listed in the **command:** block and we'll address them individually:

- **terraform -chdir=./terraform/ecs apply \
  -var docker_img_name=${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME} \
  -var docker_img_tag=0.1.<< pipeline.number >> \
  -auto-approve**
  - This command executes the Terraform code in the *terraform/ecs/* directory. This particular Terraform leverages [Terraform Variables][14], which enables you to specify the Docker image and tag to deploy.  

- **export ENDPOINT="$(terraform -chdir=./terraform/ecs output load_balancer_hostname)"**
  - Saves the ECS cluster's public URL from the Terraform *load_balancer_hostname* output into the environment variable *$ENDPOINT* which is finally saved into to the /tmp/ec/endpoint file for future use in subsequent jobs.

**- persist_to_workspace:** is a special key that represents a CircleCI Workspace. When a workspace is declared in a job, files and directories can be added to it. Each addition creates a new layer in the workspace filesystem. Downstream jobs can then use this workspace for their own needs or add more layers on top. Workspaces are not shared between pipeline runs. The only time a workspace can be accessed after the pipeline has run is when a workflow is rerun within the 15 day limit.

**- run: sleep 80**
  - Delays the job for 80 seconds. The ECS service needs a few seconds to deploy and propagate the deployment to all the ECS resources before executing subsequent job in this pipeline.

This job demonstrates a *Continuous Deployment* because it is integrated into and executed by a job within your CI/CD pipeline and it will automatically deploy updated code to the App Runner service anytime the code is committed and pushed to the upstream project code repository.


Now that you've learned how to build *Continuos Deployment* jobs, jump over to the next section where you will learn how to test the deployment to ensure the deployment and application are functioning as expected.

<!-- URL Links index -->
[[1]: https://www.terraform.io
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
[14]: https://www.terraform.io/docs/language/values/variables.html