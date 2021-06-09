---
title: "3.2 Build an Application Vulnerability Scan Job"
chapter: true
weight: 12
---

## Pickup from previous module: Continuous Integration

You briefly learned about DevSecOps in previous sections, so now it time to build a new job into our pipeline that provides a solid security scan of the code.

At the end of the last module, your config.yml file was left in the state shown in the code snippet below. If it's not that's ok, go ahead and copy the snippet below and paste it into the file. Its important to to ensure your config.yml is identical to the code snippet below before progressing further:

{{<highlight yaml>}}
version: 2.1
orbs:
  node: circleci/node@4.2.0
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  terraform: circleci/terraform@2.0.0
jobs:
  run_tests:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - node/install-packages:
          override-ci-command: npm install
          cache-path: ~/project/node_modules
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mocha-junit-reporter --reporter-options mochaFile=./test/test-results.xml
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_test_results:
          path: test/
      - store_artifacts:
          path: test-results          

{{</highlight>}}

## Build a vulnerability scan job

Now that your config.yml is ready, you can build a vulnerability scanning job. The code snippet below demonstrates how to define and implement a security scanning job within your CI/Cd pipeline.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  scan_app:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - run:
          name: Snyk Scan Application files 
          command: npm install 
      - snyk/scan:
          fail-on-issues: false
          monitor-on-build: false
{{</highlight>}}

You are already familiar with some of the job related elements, such as executors, steps, run: blocks and commands, so we'll just cover the unfamiliar parts of the example.

## - run:

The run block in this example execute an *npm install* command that installs the app dependencies which is required for accurate and effective vulnerability scanning. 

## - synk/scan:

The **- snyk/scan** orb command shown has two parameters set to false. The **fail-on-issues:** parameter does not fail the pipeline if vulnerabilities are detected for the purposes of this workshop. In most cases this should be set to *true* so that the pipeline does fail and the developer is alerted to and aware of the reported vulnerabilities. The **monitor-on-build:** parameter specifies if this project will be continuously monitored for new vulnerabilities. After running this command you will see it by logging in to the Snyk dashboard and viewing your projects. Since this parameter is set to *false*, this job will not be updated in the Snyk dashboard.

### Section summary

In this section you learned about DevSecOps and how it relates to CI/CD, along with implementing automated security scans within your pipeline. 

At the end of this section your config.yml should be identical to the code snippet below:

{{<highlight yaml>}}
version: 2.1
orbs:
  node: circleci/node@4.2.0
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  terraform: circleci/terraform@2.0.0
jobs:
  run_tests:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - node/install-packages:
          override-ci-command: npm install
          cache-path: ~/project/node_modules
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mocha-junit-reporter --reporter-options mochaFile=./test/test-results.xml
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_test_results:
          path: test/
      - store_artifacts:
          path: test-results          
  scan_app:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - run:
          name: Snyk Scan Application files 
          command: npm install 
      - snyk/scan:
          fail-on-issues: false
          monitor-on-build: false
{{</highlight>}}

Now that you've learned about continuous integration, CircleCI's config.yml and implemented an app testing pipeline job, you are ready for the next section which will cover DevSecOps and security concepts developers can easily implement into their pipelines.

<!-- URL Links index -->
[1]: https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/
[2]: https://circleci.com/developer/orbs/orb/snyk/snyk
[3]: https://support.snyk.io/hc/en-us/articles/360003812458-Getting-started-with-the-CLI