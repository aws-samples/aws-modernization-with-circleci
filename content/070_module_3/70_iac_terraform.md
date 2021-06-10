---
title: "4.1 Terraform and CI/CD"
chapter: true
weight: 10
---

## Terraform Intro

[Terraform is a tool][1] for building, changing, and versioning infrastructure safely and efficiently. Terraform can manage existing and popular service providers as well as custom in-house solutions.

Configuration files describe to Terraform the components needed to run everything from a single application to your entire datacenter. Terraform generates an execution plan describing what it will do to reach the desired state, and then executes it to build the described infrastructure. As the configuration changes, Terraform is able to determine what changed and create incremental execution plans which can be applied.

The infrastructure Terraform can manage includes low-level components such as compute instances, storage, and networking, as well as high-level components such as DNS entries and SaaS features.

Terraform will be used in this workshop to provision and destroy all of the infrastructures and resources required by the pipeline to deploy. It will also be used to test those deployments in their target environments.

## Terraform project code

The [example project repo][] has a directory named `terraform/` that houses all of the IaC code for the project. Within this directory are two other directories:

- The `ecr/` directory contains the Terraform code for the [AWS Elastic Container Repository (ECR)][2] used in the pipeline.
- The `app-runner` directory contains the Terraform code for the [AWS App Runner][3] infrastructure used in the pipeline. 

**Note: AWS App Runner will be covered in the next module.**

In Terraform, the root module is the working directory where Terraform is invoked. This means the Terraform directory encapsulates the code that represents target infrastructures and resources. Terraform commands must be executed from with the respective directories.

## Terraform CLI + Terraform Cloud

[Terraform CLI][5] is the command line interface to Terraform. The CLI is available by using the terraform command, which accepts a variety of subcommands like `terraform init` or `terraform plan`. A full list of all of the supported subcommands is in the navigation section of the linked page.

The "Terraform CLI" terminology is often used to distinguish it from other components you might use in the Terraform product family, such as Terraform Cloud or the various Terraform providers, which are developed and released separately from Terraform CLI. The Terraform CLI is primarily a local/developer environment tool used to execute Terraform code.

[Terraform Cloud][4] is a platform that performs Terraform runs to provision infrastructure, either on demand or in response to various events. Unlike a general-purpose continuous integration (CI) system, Terraform Cloud is deeply integrated with Terraform's workflows and data which allows it to make Terraform significantly more convenient and powerful for developers.

In this project, you'll be using both the Terraform CLI and Terraform Cloud to provision infrastructures and resources essential to the pipeline execution.

The Terraform Cloud credentials you created in the [CircleCI Setup section][6] will be used access the service and to capture and manage the state of Terraform CLI provisioned infrastructure and resources. Without these credentials, managing the state of the provisioned infrastructure will be extremely difficult, especially among teams.

## Example of ECR Terraform code

There are two Terraform code instances in this project. We will briefly cover the Terraform AWS ECR provisioning code. These code instances already exist and you will not have to write any Terraform code. We will take this opportunity to familiarize you with the code that this project uses to create an new AWS Public ECR. You <i>do not</i> have to take any action with the example code snippet below:

{{<highlight terraform>}}
terraform {
  backend "remote" {
    organization = "datapunks"

    workspaces {
      name = "ecr-aws-circleci"
    }
  }
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_ecrpublic_repository" "aws_circleci_repo" {
  repository_name = var.ecr_name
}

output "ECR_REPO_ARN" {
  value = aws_ecrpublic_repository.aws_circleci_repo.arn
}

output "ECR_REG_ID" {
  value = aws_ecrpublic_repository.aws_circleci_repo.registry_id
}

output "ECR_NAME" {
  value = aws_ecrpublic_repository.aws_circleci_repo.repository_name
}

output "ECR_URI" {
  value = aws_ecrpublic_repository.aws_circleci_repo.repository_uri
}
{{</highlight>}}

Here is a break down of what the elements and blocks in this code snippet do:

- The `terraform{}` element specifies that the state for this infrastructure will be hosted and maintained in the `ecr-aws-circleci` Terraform Cloud workspace you created earlier. 
- The `backend "remote" {}` block demonstrates how to specify the organization and workspaces corresponding to the related resources.
- The `provider "aws" {}` block specifies the [Terraform provider][7] to use in this code. Terraform relies on plugins called "providers" to interact with cloud providers, SaaS providers, and other APIs. Terraform configurations must declare which providers they require so that Terraform can install and use them. Some providers require configuration (like endpoint URLs or cloud regions) before they can be used. Each provider adds a set of resource types and data sources that Terraform can manage. Every resource type is implemented by a provider; without providers, Terraform can't manage any kind of infrastructure. Most providers configure a specific infrastructure platform (either cloud or self-hosted). Providers can also offer local utilities for tasks like generating random numbers for unique resource names.
- The `resource "aws_ecrpublic_repository" {}` block defines the public ECR resource that is going to be created. Terraform [resources][8] are the most important element in the Terraform language. Each resource block describes one or more infrastructure objects, such as virtual networks, compute instances, or higher-level components such as DNS records.
- The `output {}` block defines [output values][9] that make information about your infrastructure available on the command line, and can expose information for other Terraform configurations to use. Output values are similar to return values in programming languages. These output values provide a solid mechanism for retrieving infrastructure data that can be passed between CI/CD jobs in a pipeline.

Now that we've covered Terraform and an example of Terraform code, you can go to the next section, where you'll learn how to build a new job that will provision a new AWS Public ECR.

<!-- URL Links index -->
[1]: https://www.terraform.io
[2]: https://aws.amazon.com/ecr/
[3]: https://aws.amazon.com/apprunner/
[4]: https://www.terraform.io/docs/cloud/
[5]: https://www.terraform.io/docs/cli/index.html
[6]: /040_circleci_setup/43_terraform_cloud_token.html
[7]: https://www.terraform.io/docs/providers/index.html
[8]: https://www.terraform.io/docs/language/resources/index.html
[9]: https://www.terraform.io/docs/language/values/outputs.html