---
title: "Continuous Deployment"
chapter: true
draft: false
weight: 8
---

# Introduction

During this workshop, we've covered many Continuos Delivery related concepts such as Continuous Integration, DevSecOps and Infrastructure as Code, and now we're going to briefly discuss [Continuous Deployments (CD)][1] the **"CD"** in CI/CD.

As Continuous Integration (CI) primarily focuses on software development practices, Continuous Deployment can be considered the practices that deliver or release the results of CI.

[Continuous Deployment (CD)][1] is the practice of automatically releasing code changes to target environments. CD is also a strategy for releasing software where any code commit that passes quality assurance (QA) and testing phases are automatically released into the target environments such as QA, staging and ultimately production.

In some teams and organizations, Continuous Deployment means that every change goes through the CI/CD pipeline and automatically gets put into production, resulting in many software releases every day. Other teams may implement CD a differently, for example they may automatically deploy releases to pre-production environments, such as QA to ensure that the release conforms to regulatory requirements before deploying the release to a production environment. In both scenarios, the software releases are automatically deployed to target environments regardless of when or how they're deployed.

Below are some core concepts of Continuous Deployment:

- Create release artifacts to deploy
- Automatically deploy artifacts to target environments
- Validate apps and services are functioning post deployment
- Monitor deployments to verify state and recover if failing

In this module you will:

- Build a job that creates an [AWS Elastic Container Service][3] Cluster powered by [AWS Graviton (Arm) EC2][2] compute nodes, via Terraform
- Deploy the project app container to ECS
- Build a job that tests the deployment to ensure its valid and functioning as expected
- Build jobs that will destroy all of the infrastructures and resources created by previous pipeline jobs

Now that you learned about Continuous Deployment, it's time to see it in action within your CI/CD pipeline, so jump over to the next section and get started with building jobs that demonstrate Continuous Deployment concepts.

<!-- URL Links index -->
[1]: https://circleci.com/integrations/deployment/
[2]: http://aws.amazon.com/ec2/graviton
[3]: https://aws.amazon.com/ecs/
