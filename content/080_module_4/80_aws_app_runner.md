---
title: "5.1 AWS App Runner"
chapter: true
weight: 10
---

## AWS App Runner

[AWS App Runner][3] will be the service that your pipeline will create and automatically deploy the project Docker images to. 

AWS App Runner is a fully managed service that makes it easy for developers to quickly deploy containerized web applications and APIs, at scale and with no prior infrastructure experience required. Start with your source code or a container image. App Runner automatically builds and deploys the web application and load balances traffic with encryption. App Runner also scales up or down automatically to meet your traffic needs. With App Runner, rather than thinking about servers or scaling, you have more time to focus on your applications.

App Runner is a great service to demonstrate Continuous Deployment concepts so let's jump into the next section where you will learn how to build a new job that creates a new App Runner service using Terraform and deploy the project docker image to it.


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