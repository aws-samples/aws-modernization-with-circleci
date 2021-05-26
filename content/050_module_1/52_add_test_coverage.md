---
title: "2.2 Implement continuous test coverage analysis"
chapter: true
weight: 12
---

In this section we will be using a tool called CodeCov to implement continuous test coverage analysis of our application. This is an important practice because it allows teams to ensure that their tests are testing all of their code and how much of their code of being exercised. 

## Step 1 &mdash; Add step for test coverage

Now that we have added another test, we can add another `store_artifacts` step to track our test coverage, place this code snippet under the 1st `store_artifacts` step. If you are lost, don't worry, we'll have the entire code snippet of the config.yml at the bottom of this page.

{{<highlight yaml>}}
- store_artifacts:
    path: coverage
{{</highlight>}}
  
## Step 2 &mdash; Setting up CodeCov

1. First, link [Github and CodeCov](https://codecov.io/gh)

    ![CodeCov Sign-up](/images/codecov-sign-up.png)

2. Codecov will automatically sync the repositories in your GitHub account. Select the `aws-workshop-demo-app`.

3. You will find your token directly when you select the repository.

    ![CodeCov Token](/images/codecov-token.png)

4. Next, create an environment variable in the CircleCI dashboard called `CODECOV_TOKEN`. 

    ![Add CodeCov Token to CircleCI](/images/codecov-add-env-var.png)

## Step 3 &mdash; Setting a threshold for test coverage in the codecov.yml

The ultimate goal of continuous integration is continuous deployment, and the ability to deploy with confidence. Implementing this in our pipeline will help us meet this goal by adding coverage thresholds. When thresholds are not met, the pipeline stops the deployment.

Now we need to add a YAML file to the root of your sample react project to configure the thresholds for CodeCov. Create a file called `codecov.yml` and add this configuration to it:

{{<highlight yaml>}}
codecov:
    require_ci_to_pass: no

coverage:
    status:
        project:
            default:
                target: 80%    # the required coverage value
                threshold: 1%  # the leniency in hitting the target
{{</highlight>}}

## Step 4 &mdash; Add step to config.yml

Then, we need to customize our `config.yml` to add the `Send to codecov` step, below the store_artifacts step:

{{<highlight yaml>}}
- run:
    name: Send to codecov
    command: |
        bash <(curl -s https://codecov.io/bash) -Z
{{</highlight>}}
  
Your `config.yml` file should look like this:

{{<highlight yaml>}}
# Use the latest 2.1 version of CircleCI pipeline process engine. See: https://circleci.com/docs/2.0/configuration-reference
version: 2.1
jobs:
    test:
        docker:
            - image: circleci/node:14.0
        steps:
            - checkout
            - run: npm install
            - run:
                name: Run tests with JUnit as reporter
                command: npm test --ci --runInBand --reporters=default --reporters=jest-junit
                environment:
                    JEST_JUNIT_OUTPUT_DIR: test-results
            - store_artifacts:
                path: test-results
            - store_artifacts:
                path: coverage
            - run:
                name: Send to codecov
                command: |
                    bash <(curl -s https://codecov.io/bash) -Z
            - run:
                name: Unzip AWS CLI
                command: unzip -q -o awscliv2.zip
            - run:
                name: Install AWS CLI
                command: sudo ./aws/install
            - run:
                name: Remove AWS CLI Installer
                command: rm awscliv2.zip
            - deploy:
                command:
                    export ARTIFACT_BUILD=$CIRCLE_PROJECT_REPONAME-$CIRCLE_BUILD_NUM.zip
                    zip -r $ARTIFACT_BUILD test-results/
                    aws s3 cp $ARTIFACT_BUILD s3://{your-unique-bucket-name} --metadata {\"git_sha1\":\"$CIRCLE_SHA1\"}
workflows:
    unit-test:
        jobs:
            - test
{{</highlight>}}

{{% notice tip %}}
Be sure that your commands are defined in the right order, they matter when orchestrating your pipeline!
{{% /notice %}}

In this section, we've added CodeCov's token + configuration file to our config.yml. This introduces code coverage analysis in our pipeline. Let's move on to the next section where we will add some vulnerability analysis with FOSSA.