---
title: "2.3 Build a Unit-Test Job"
chapter: true
weight: 14
---

## Build a Unit-Test Job

In this section you will build specify a job that will unit test the application on every code commit. Let's begin with by adding some new elements the config.yml file.

Replace the all the content inside of the config.yml by copying and pasting the code snippet below and save the file:

{{<highlight yaml>}}
version: 2.1
orbs:
  node: circleci/node@4.2.0
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  terraform: circleci/terraform@2.0.0

{{</highlight>}}

### versions:

In the code above, the **version:** key specifies which backend version of CircleCI services will process the pipeline. It is also intended to issue warnings for deprecation or breaking changes.

### orbs:

**orbs:** key represents [Orbs][2] which are shareable packages of CircleCI configuration you can use to simplify your builds. Choose from the many partner, community, or CircleCI authored orbs in our [public registry][3]. In this case, the example application is a node.js project and the **node: circleci/node@4.2.0** line specifies that this pipeline will import or include that orb's functionality. The node.js orb is very useful in accomplishing many things such as *npm installs* and [cache management][4]. Think of orbs as a form of encapsulation similar to concepts found Object Oriented Programming (OOP) principles. The other orbs listed in this example, provide the same ease of use for their specific purposes and will be discussed in future sections and modules of this workshop.

### jobs:

Now you'll add the first job to your pipeline. Append the following **jobs:** code snippet to your config.yml under the previous snippet you pasted:

{{<highlight yaml>}}
jobs:
  run_tests:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - node/install-packages:
          override-ci-command: npm install
          cache-path: ~/project/node_modules

{{</highlight>}}

In above example, there is a job defined with the **run_test:** key which is essentially a label or name for this job and used as an identifier. When naming jobs try to use meaningful naming conventions that purposely  describe what they do for readability, maintainability and execution with in pipelines.

**docker:** key represents the [executor][7] which will serve as the runtime in which related command will be executed for this particular job. In this case, the code will be executed on a [Docker executor][8] which is technically a Docker container, specified by the **- image: cimg/node:14.16.0** key.

**steps:** key represents a list of commands to execute related to this job. Every item in this list executes an action or command that accomplishes. This list has a few items, the **- checkout** command is an alias for git clone which gets the code for a specific commit.

The **- node/install-packages:** key represents the node orb implementation within the job, which installs application dependencies and performs behind the scenes caching of those dependencies which will speed up job executions.

### - run:

Now your ready to add a **run:** block that will specify commands that will execute unit-test scripts on the application. Append the following code snippet to your config.yml under the previous snippet you pasted. Copy and paste this snippet:

{{<highlight yaml>}}
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

The above example demonstrates a run block and its properties **name:** and **commands:** which specifies a name identifier and list of commands to execute respectively. The text in the **command:** key executes the unit-test script and saves the results to files, which are then saved or pinned to the pipeline, using the [- store_test_results: ][6] and [- store_artifacts:][5] keys. Pinning test results to pipelines are great for debugging and auditing purposes.  The above code snippet represents a run: block that executes a unit test on the code when it changes. So when a developer commits code and pushes those changes to the upstream repo, CircleCI will detect those changes in the push and trigger execution of the directives defined in the config.yml file. In this case, executing a test, generating test results in the form of files and saving or pinning those results to the pipeline build for auditing purposes. They are very helpful when having to research build and deployment issues as well as serving and audit artifacts if needed.

### Module summary

In this section you learned the importance of testing code and gained some familiarity with the config.yml file along with its elements. You also started building a new CI/CD for the example project you'll be using in this workshop, so congratulations on building your first CI/CD pipeline on CircleCI.

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

{{</highlight>}}

Now that you've learned about continuous integration, CircleCI's config.yml and implemented an app testing pipeline job, you are ready for the next section which will cover DevSecOps and security concepts developers can easily implement into their pipelines.


<!-- URL Links index -->
[1]: https://github.com/CircleCI-Public/aws-circleci-modernization-workshop-code
[2]: https://circleci.com/docs/2.0/orb-intro/
[3]: https://circleci.com/developer/orbs
[4]: https://circleci.com/docs/2.0/persist-data/#caching-strategies
[5]: https://circleci.com/docs/2.0/configuration-reference/#storeartifacts
[6]: https://circleci.com/docs/2.0/configuration-reference/#storetestresults
[7]: https://circleci.com/docs/2.0/configuration-reference/#executors-requires-version-21
[8]: https://circleci.com/docs/2.0/configuration-reference/#docker-executor