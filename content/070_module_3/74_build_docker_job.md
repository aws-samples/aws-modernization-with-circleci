---
title: "4.2 Build a Docker Image Job"
chapter: true
weight: 12
---

## Docker Images and Containers

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure, so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

A Docker image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, and includes the configuration details needed to make your application run.

You might create your own images, or you might only use images created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers that have changed are rebuilt. This is part of what makes images so lightweight, small, and fast when compared to other virtualization technologies.

A Docker container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.

In this section, you will create a Docker image for the project application, so you will create a CI/CD job that builds an image and uploads/pushes it to the Docker Hub account you created earlier.

## AWS Graviton EC2 (Arm) Compute Nodes

This project provisions an AWS ECS cluster that is powered by [AWS Graviton EC2 compute nodes][10] which are powered by [Arm based][1] processors. Arm based architectures are incompatible with [x86][11] architectures which means software and docker images must be compiled for the architectures that they're going to be deployed to. Since were deploying to an Arm architecture, we need to build our Docker image on an Arm based executor in our pipeline.

CircleCI has [Arm based resources classes][12] that can be defined and implemented to code on Arm executors. This enables you to build Arm compatible artifacts such as the Docker image we'll deploy to an ECS cluster. In the sections below we will build a job that leverages CircleCI's Arm resource class executor to build an Arm compatible Docker image.

## Pickup from previous module: DevSecOps

Using what you learned about CI/CD jobs in previous sections, you will create a new job that builds a [Docker image][2] and pushes it to [Docker Hub][10] from our pipeline.

To complete this module, your config.yml must be identical to the one at the end of the last module. If yours is different, that's ok! Just go ahead and copy this snippet and paste it into the file:

{{<highlight yaml>}}
version: 2.1
orbs:
  snyk: snyk/snyk@1.2.3
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

## Docker Image build job

In this section, you will learn how to create a Docker Image based on the project application. Then you will learn how to push it [Docker Hub][10] from within a CI/CD job.

The following code snippet shows how to define and provision a job that builds and pushes an [Arm compatible][1] Docker Image within your CI/CD pipeline.

Copy this code snippet and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  build_docker_image:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout  
      - docker/check
      - docker/build:
          image: $DOCKER_LOGIN/$CIRCLE_PROJECT_REPONAME
          tag: 0.1.<< pipeline.number >>
      - docker/push:
          image: $DOCKER_LOGIN/$CIRCLE_PROJECT_REPONAME
          tag: 0.1.<< pipeline.number >>
{{</highlight>}}

This `build_docker_image:` job demonstrates a few new elements that differ from jobs you previously built. You're already familiar with the *checkout* and *steps* commands so we'll explain the new bits in this job you're probably not familiar with.

**machine:** represents the CircleCI [machine executor][12] which executes jobs in a dedicated, ephemeral Virtual Machine.

**image:** specify an operating system image for the executor from [this list][14]. In this example we're using *ubuntu-2004:202101-01*

**resource_class:** enables you to configure CPU and RAM resources for each job. In this case we're specifying an **arm.medium** resource class which will provide an Arm powered executor to execute the pipeline on. 

**docker/check** this is a Docker orb command that validates the Docker client is installed on the executor.

**docker/build:** Docker orb command that executes the [Docker build][7] command

**image:** species the name of the Docker image to build. In this example, the image name is defined by the *$DOCKER_LOGIN* environment variable you created earlier and the native *$CIRCLE_PROJECT_REPONAME* environment variable supplied by the platform.

**tag:** specifies the Docker image tag. In this example, the *<< pipeline.number >>* adds a reference to the pipeline that executed the image build.

**docker/push:** Docker orb command that executes the [Docker push][9] command

**image:** species the name of the Docker image to push to Docker Hub.

**tag:** specifies the Docker image tag to push to Docker Hub.


Congratulations! You have created a new pipeline job that builds an Arm compatible Docker image and pushed it to Docker Hub.

At the end of this section your config.yml should be identical to this code snippet:

{{<highlight yaml>}}
version: 2.1
orbs:
  snyk: snyk/snyk@1.2.3
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
  build_docker_image:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout  
      - docker/check
      - docker/build:
          image: $DOCKER_LOGIN/$CIRCLE_PROJECT_REPONAME
          tag: 0.1.<< pipeline.number >>
      - docker/push:
          image: $DOCKER_LOGIN/$CIRCLE_PROJECT_REPONAME
          tag: 0.1.<< pipeline.number >>
{{</highlight>}}

## Summary

In this module you learned about Infrastructure as Code concepts and Hashicorp Terraform. You learned how to create pipeline jobs that executes essential docker commands to build a Docker image and push it to Docker Hub. You also learned about Arm architectures and how CircleCI and AWS supports Arm CI/CD pipelines and Arm capable infrastructures.

Jump over to the next module, Continuous Deployment, where you will learn how to:

- Create an Amazon ECS Cluster
- Deploy a Docker image
- Test the image deployment using smoke-tests
- Destroy all the infrastructure and resources using Terraform

<!-- URL Links index -->
[1]: https://developer.arm.com/architectures
[2]: https://circleci.com/docs/2.0/building-docker-images/
[3]: https://circleci.com/docs/2.0/persist-data/#using-workspaces
[4]: https://circleci.com/developer/orbs/orb/circleci/aws-cli
[5]: https://circleci.com/docs/2.0/pipeline-variables/#pipeline-values
[6]: https://linuxize.com/post/bash-source-command/#:~:text=The%20source%20command%20reads%20and,Linux%20and%20UNIX%20operating%20systems.
[7]: https://docs.docker.com/engine/reference/commandline/build/
[8]: https://docs.docker.com/engine/reference/builder/
[9]: https://docs.docker.com/engine/reference/commandline/push/
[10]: https://docs.docker.com/docker-hub/
[10]: https://aws.amazon.com/pm/ec2-graviton/
[11]: https://en.wikipedia.org/wiki/X86
[12]: https://circleci.com/docs/2.0/configuration-reference/#machine
[13]: https://circleci.com/docs/2.0/arm-resources/
[14]: https://circleci.com/docs/2.0/configuration-reference/#available-machine-images
[15]: https://circleci.com/docs/2.0/optimizations/#resource-class