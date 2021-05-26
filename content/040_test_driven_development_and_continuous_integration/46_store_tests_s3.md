---
title: "1.4 Storing tests in S3"
chapter: true
weight: 16
---

## Step 1 &mdash; Setup Amazon S3

1. Go to your [S3 bucket dashboard](https://s3.console.aws.amazon.com/s3/home?region=us-east-1#).

2. Select the Create Bucket button.

3. Name your bucket something unique: `{yourName}-circleci-workshop-bucket`.

4. We will **uncheck** the Block all public access checkbox to enable **public access** of this particular bucket. You will also need to check **Acknowledge current settings checkbox**

    ![Bucket Settings](/images/create-s3-bucket.png)

5. Click the properties tab and scroll down to the bottom to the Static website hosting section:

    ![Turn on static web hosting](/images/s3-static-web-hosting.png)

6. Click edit, enable static web hosting and click the save changes button on bottom right hand corner.

## Step 2 &mdash; Store tests in Amazon S3

1. Open the `config.yml` file

2. Add a section for storing the test data in your config.yml file. **Remember to update the bucket name below!**
    * What we're doing here is setting the steps to store test results in the S3 bucket that we've created.
    * We're also adding the steps  and install the AWS CLI2 installer

{{<highlight yaml>}}
steps:
    - store_artifacts:
        path: test-results
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
{{</highlight>}}

{{% notice info %}}
Your `config.yml` file should now look like the one below. If it does not, please copy the yaml code below and paste it into your `config.yml` file, **otherwise CircleCI will not execute these commands properly**.
{{% /notice %}}

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

You've completed the first module! We've learned how to add some simple tests to our React application as well as creating and configuring our `config.yml` file in our `.circleci` directory. We've added the configurations to install npm, run tests, store test results to the newly created S3 bucket and installing AWS CLI. Let's move on to the next module where we will learn how to implement coverage analysis and how to implement security scanning in our pipeline.