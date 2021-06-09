---
title: "Infrastructure as Code (IaC)"
chapter: true
draft: false
weight: 6
---

# Introduction

IaC is an integral part of modern CI/CD pipelines. It is the process of managing and provisioning cloud and IT resources via machine readable definition files. IaC enables organizations to create, manage, and destroy compute resources using modern DevOps tools by statically defining and declaring these resources in code. 

IaC configuration files contain infrastructure specifications, which makes it easier to manage and distribute configurations. It also ensures that you provision environments consistently Developers can automate infrastructure provisioning with IaC resulting in minimal to no manual provisioning and management of servers, operating systems, storage, and other infrastructure components each time an app is developed or deployed.

IaC can help your organization manage IT infrastructure needs while also improving consistency and reducing errors and manual configuration.

Benefits:
- Cost reduction
- Reduce errors
- Increase in speed of deployments
- Standardizes infrastructure provisioning
- Improve infrastructure consistency
- Eases management of deployed infrastructure and their configurations


IaC eliminates the need to maintain individual environments with unique configurations that can’t be reproduced automatically and ensures that the production environment will be consistent. Codified infrastructure can be executed in a CI/CD pipeline like an application does during software development, applying the same testing, version control and execution to the infrastructure code.

In this section, you will learn how to:

- Integrate Infrastructure as Code into CI/CD pipeline jobs using [Hashicorp Terraform][1]
- Automate provisioning infrastructure and resources
- Create an [AWS Elastic Container Registry (ECR)][2] using Terraform

<!-- URL Links index -->
[1]: https://www.terraform.io/
[2]: https://aws.amazon.com/ecr/







