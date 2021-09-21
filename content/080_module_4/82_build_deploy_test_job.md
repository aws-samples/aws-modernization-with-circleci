---
title: "5.3 Build a Deployment Test Job"
chapter: true
weight: 12
---

## Deployment Test Job

In the previous section you created a pipeline job that provisions an [Amazon ECS][2] cluster and deploys the project application to it in the form of a Docker container.

In this section you will build a job that will test the application deployment to ensure the deployment and application are functioning as expected and surface any unexpected issues or bugs. 

Merely successfully deploying releases to target environments does not guarantee that the deployment is functioning as expected. Far too often, deployed applications are not tested post deployment and presents serious issues that can produce negative effects such unavailable services or service level outages.

Testing deployments is a recommended practice that should be adopted in all deployment processes, *"Continuous"* or not. Testing deployments is especially easy to accomplish as a job within a CI/CD pipeline and should be implemented whenever possible.

## Build smoke-test Deployment job

Now that you built an awesome deployment job, that deploys code changes to ECS, its recommended, the deployments should be tested. Testing deployments can automated and conducted from a pipeline job. Next you'll build a pipeline job that will test deployments.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  smoketest_deployment:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout
      - attach_workspace:
          at: /tmp/ecs/
      - run:
          name: Smoke Test ECS Deployment
          command: |
            source /tmp/ecs/endpoint
            ./test/smoke_test $ENDPOINT
{{</highlight>}}

The snippet above executes a set of tests that will verify the deployment and application are functioning as expected. The tests can be found in the **test/** directory in the project code repo. The test checks for an *OK 200* service response and also validates the site is serving up specific text. This example of deployment testing serves as a simple yet effective validation. Let's briefly breakdown the snippet above.

You should already be familiar with the *machine:*, *steps:* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

**- attach_workspace:** block attaches the CircleCI Workspace that you created in the previous job. This command will provide the job access to the data saved earlier.

**- run: name: Smoke Test ECS Deployment** block has multiple commands listed in the **command:** block and we'll address them individually:

- **source /tmp/ecs/endpoint**
    - sources the */tmp/ecs/endpoint* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **./test/smoke_test $ENDPOINT**
    - Executes the *smoke_test* script that tests for an *OK 200* service response and specific text rendered in the deployed application.

Now that you learned about testing deployments, jump over to the next section where you will learn about destroying all the awesome infrastructures and resource your jobs created.

<!-- URL Links index -->
[1]: https://www.terraform.io
[2]: https://aws.amazon.com/ecr/
[3]: https://aws.amazon.com/apprunner/
[4]: https://www.terraform.io/docs/cloud/
[5]: https://www.terraform.io/docs/cli/index.html
[6]: /040_circleci_setup/43_terraform_cloud_token.html