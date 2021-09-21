---
title: "5.1 Amazon Elastic Container Service - ECS"
chapter: true
weight: 10  
---

## Amazon Elastic Container Service - ECS

[Amazon Elastic Container Service (ECS)][2] will be the service that your pipeline will provision and automatically deploy the project Docker images to. 

Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerized applications. It deeply integrates with the rest of the AWS platform to provide a secure and easy-to-use solution for running container workloads in the cloud.

Amazon ECS is a great service to demonstrate Continuous Deployment concepts so let's jump into the next section where you will learn how to build a new job that provisions a new Amazon ECS service using Terraform and deploy the project docker image to it.


<!-- URL Links index -->
[1]: https://www.terraform.io
[2]: https://aws.amazon.com/ecs
[3]: https://aws.amazon.com/pm/ec2-graviton/
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