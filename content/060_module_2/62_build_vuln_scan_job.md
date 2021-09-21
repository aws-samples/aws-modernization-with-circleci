---
title: "3.2 Build an Application Vulnerability Scan Job"
chapter: true
weight: 12
---

## Pickup from previous module: continuous integration

Now that you've learned about DevSecOps from previous sections, it's time to build a new job into our pipeline that provides a solid security scan of the code.

At the end of the last module, your config.yml file was left in the state shown in the code snippet below. If it's not that's ok, go ahead and copy the snippet below and paste it into the file. It's important to to make sure your config.yml is identical to the following code snippet before continuing the workshop:

{{<highlight yaml>}}
version: 2.1
orbs:
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  node: circleci/node@4.2.0
  docker: circleci/docker@1.5.0
  terraform: circleci/terraform@3.0.0
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

Your config.yml is ready for you to build a vulnerability scanning job. The following code snippet demonstrates how to define and implement a security scanning job within your CI/CD pipeline.

Copy this snippet and append it to the bottom of your config.yml file:

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

You are already familiar with some of the job related elements, such as executors, steps, run: blocks and commands, so we'll just cover the new parts of the example.

## - run:

The run: block in this example executes an *npm install* command that installs the app dependencies; this is required for accurate and effective vulnerability scanning. 

## - synk/scan:

The **- snyk/scan** orb command has two parameters set to false. Setting the `fail-on-issues:` parameter to false keeps the pipeline moving, even if vulnerabilities are detected. In most cases this parameter should be set to *true* so that the pipeline <i>does</i> fail and the developer is alerted to the reported vulnerabilities. The `monitor-on-build:` parameter specifies whether this project will be continuously monitored for new vulnerabilities. After running this command, you can review what it finds by logging in to the Snyk dashboard and viewing your projects. Since this parameter is set to <i>false</i> for this module, the job will not be updated in the Snyk dashboard.

### Module summary

In this section you learned about DevSecOps and how it relates to CI/CD, along with how to implement automated security scans within your pipeline. 

At the end of this section your config.yml should be identical to this code snippet:

{{<highlight yaml>}}
version: 2.1
orbs:
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  node: circleci/node@4.2.0
  docker: circleci/docker@1.5.0
  terraform: circleci/terraform@3.0.0
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

Congratulations! You've learned about DevSecOps and how to implement security and vulnerability scans into your CI/CD pipelines and automate their execution to ensure your applications are as secure as possible.  In the next section, you will learn about Infrastructure as Code and how to build jobs that provision critical infrastructures and resources.

<!-- URL Links index -->
[1]: https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/
[2]: https://circleci.com/developer/orbs/orb/snyk/snyk
[3]: https://support.snyk.io/hc/en-us/articles/360003812458-Getting-started-with-the-CLI