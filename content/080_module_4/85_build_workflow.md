---
title: "5.6 Build a CircleCI Workflow"
chapter: true
weight: 15
---

## CircleCI Workflows
Workflows help you increase the speed of your software development through faster feedback, shorter reruns, and more efficient use of resources. [CircleCI Workflows][15] are a set of rules for defining a collection of jobs and their run order. Workflows support complex job orchestration using a simple set of configuration keys to help you resolve failures sooner.

With workflows, you can:

- Run and troubleshoot jobs independently with real-time status feedback.
- Schedule workflows for jobs that should only run periodically.
- Fan-out to run multiple jobs concurrently for efficient version testing.
- Fan-in to quickly deploy to multiple platforms.

For example, if only one job in a workflow fails, you will know it is failing in real-time. Instead of wasting time waiting for the entire build to fail and rerunning the entire job set, you can rerun just the failed job.

## Build a Workflow

Now that you've built all the jobs this workshop pipeline needs, its time to orchestrate how and when those jobs are executed. This will require you to define a Workflow.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
workflows:
  scan_deploy:
    jobs:
      - run_tests
      - scan_app
      - create_ecr_repo
      - build_push_docker_image:
          requires:
            - create_ecr_repo
      - create_deploy_app_runner:
          requires:
            - build_push_docker_image
      - smoketest_deployment:
          requires:
            - create_deploy_app_runner
      - approve_destroy:
          type: approval
          requires:
            - smoketest_deployment            
      - destroy_ecr:
          requires:
            - approve_destroy
      - destroy_app_runner:
          requires:
            - approve_destroy
{{</highlight>}}

**workflows:** scan_deploy: block defines a CircleCI workflow that will define the jobs to execute and how they're executed. The *scan_deploy* is the an identifier/name for the workflow. A CircleCI pipeline can execute multiple workflows but in this workshop you define only one.

**jobs:** this key defines the list of jobs that will be executed in this pipeline. The jobs listed in this workflow are the jobs you defined throughout this workshop, with the exception of the **- approve_destroy:** job.

Since you defined the jobs listed in this workflow, we'll discuss the *requires:* and *type:* keys.

## requires: key

Workflows define a list of jobs and their run order. It is possible to run jobs concurrently, sequentially, on a schedule, or with a manual gate using an approval job. Jobs are run in parallel by default, so you must explicitly require any dependencies by their job name. The [requires:][16] key is what creates dependencies between jobs. In the workflow snippet above, there are job that must be execute before others. For example the AWS ECR must be created before the Docker Image is pushed to it.

## type: approval

A job may have a [type of approval][17] indicating it must be manually approved before downstream jobs may proceed. Jobs run in the dependency order until the workflow processes a job with the type: approval key followed by a job on which it depends. For example, the *approve_destroy* job listed in the workflow, stops the pipeline from continuing until a manual action occurs, which in CircleCI, requires an authorized user to click the *Approve* button in the CircleCI Dashboard.

Congratulations! You created a workflow for your pipeline. At this point your CI/CD pipeline is complete and can be triggered or executed.

In the next section you will learn how to trigger this pipeline that you've defined in the *config.yml* file. Jump over to the next section and learn how to trigger your pipeline.

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
[15]: https://circleci.com/docs/2.0/workflows/#overview
[16]: https://circleci.com/docs/2.0/configuration-reference/#requires
[17]: https://circleci.com/docs/2.0/configuration-reference/?section=reference#type
