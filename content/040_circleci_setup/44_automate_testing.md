---
title: "1.3 Automating test integration"
chapter: true
weight: 14
---

## Step 1 &mdash; Automate testing with CircleCI

1. Navigate into the CircleCI directory with:

```bash
cd ~/environment/aws-workshop-demo-app
```

2. Create a new config by running:

```bash
# This config file will define your CI/CD pipeline
# This file will automatically be detected and executed by CircleCI
touch .circleci/config.yml
```

3. Create your test automation workflow:

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
workflows:
    unit-test:
        jobs:
            - test
{{</highlight>}}

This configuration is creating a job called **test** that leverages the **node:14.0** Docker image to run tests and report those tests to Junit. 

## Step 2 &mdash; Store tests locally

Add the `jest-junit` config to your `package.json` and configure the output directory for your tests:

{{<highlight json>}}
"jest-junit": {
    "suiteName": "jest tests",
    "outputDirectory": ".",
    "outputName": "test-results/junit.xml",
    "classNameTemplate": "{classname}-{title}",
    "titleTemplate": "{classname}-{title}",
    "ancestorSeparator": " › ",
    "usePathForSuiteName": "true"
}
{{</highlight>}}

Great, we've configured the **test job** in our config.yml to automate testing our code. Now, let's learn how to store our tests both locally and on the cloud.