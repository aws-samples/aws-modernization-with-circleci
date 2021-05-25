---
title: "1.3 Automating test integration"
chapter: true
weight: 14
---

## Step 1 &mdash; Automate testing with CircleCI

1. Navigate into the CircleCI directory with:

```bash
cd ~/environment/ci-demo-app
```

2. Create a new config by running:

```bash
touch config.yaml
```

3. Create your test automation workflow:

```YAML
# Use the latest 2.1 version of CircleCI pipeline process engine. See: https://circleci.com/docs/2.0/configuration-reference
version: 2.1
# Use a package of configuration called an orb.
orbs:
  # Declare a dependency on the welcome-orb
  welcome: circleci/welcome-orb@0.4.1
# Orchestrate or schedule a set of jobs
workflows:
  # Name the workflow "welcome"
  welcome:
    # Run the welcome/run job in its own container
    jobs:
    test:
    docker:
      - image: circleci/node:14.0
        auth:
          username: $DOCKERHUB_USERNAME
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    steps:
      - checkout
      - run: echo "this is the test job"
      - run: npm install
      - run:
          name: Run tests with JUnit as reporter
          command: npm test --ci --runInBand --reporters=default --reporters=jest-junit
          environment:
            JEST_JUNIT_OUTPUT_DIR: test-results

```

This configuration is creating a job called `test` that leveraged the `node:14.0` Docker image to run tests and report those tests to Junit. 

## Step 2 &mdash; Store tests locally

Add the `jest-junit` config to your `package.json` and configure the output directory for your tests:

```YAML
"jest-junit": {
    "suiteName": "jest tests",
    "outputDirectory": ".",
    "outputName": "test-results/junit.xml",
```

Great, we've got some automation set up to test our code, let's learn how to continuously store our tests both locally and on the cloud.