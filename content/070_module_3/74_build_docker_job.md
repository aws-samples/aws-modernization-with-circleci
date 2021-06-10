---
title: "4.3 Build a Docker Image Job"
chapter: true
weight: 14
---

## Docker Images and Containers

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

A Docker image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

A Docker container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.

In this workshop, you will create a Docker image for the project application, so you will need to create a CI/CD job that builds an image and uploads/pushes it to the AWS Public ECR you previously created.

## Docker Image Build Job

In this section, you will learn how to create a Docker Image based off of the project application and push it to an AWS ECR from within a CI/CD job.

The code snippet below demonstrates how to define and provision a job that builds and pushes a Docker Image within your CI/Cd pipeline.

Copy the snippet below and append it to the bottom of your config.yml file:

{{<highlight yaml>}}
  build_push_docker_image:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - setup_remote_docker
      - attach_workspace:
          at: /tmp/ecr/      
      - aws-cli/install
      - aws-cli/setup:
          aws-access-key-id: AWS_ACCESS_KEY_ID
          aws-secret-access-key: AWS_SECRET_ACCESS_KEY
      - run:
          name: Build Docker image
          command: |
            export TAG=0.1.<< pipeline.number >>
            echo 'export TAG='$TAG >> /tmp/ecr/ecr_envars
            source /tmp/ecr/ecr_envars
            docker build -t $ECR_PUBLIC_URI -t $ECR_PUBLIC_URI:$TAG .
      - run:
          name: Push AWS ECR Public
          command: |
            source /tmp/ecr/ecr_envars
            aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_URL
            docker push $ECR_PUBLIC_URI
      - persist_to_workspace:
          root: /tmp/ecr/
          paths:
            - "*"

{{</highlight>}}

You should already be familiar with the *docker:*, *steps:* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

**- setup_remote_docker** block creates [a remote Docker environment][1] configured to execute Docker commands. See [Running Docker Commands][2] for details. This essentially provides the Docker executor access to the Docker CLI which inturn, enables a Docker image build with a Docker container.

**- attach_workspace:** block attaches the [CircleCI Workspace][3] that you created in the previous job. This command will provide the job access to the data saved earlier.

**- aws-cli/install** leverages the [AWS CLI Orb][4] which provides the job access to the AWS CLI to execute AWS centric commands. 

**- aws-cli/setup:** leverages the [AWS CLI Orb][4] and its *setup* command to initialize the AWS cli to access AWS services with the AWS credentials defined in the *$AWS_ACCESS_KEY_ID* and *$AWS_SECRET_ACCESS_KEY* project environment variables defined earlier.

**- run: name: Build Docker image** block has multiple commands listed in the **command:** element and we'll address them individually.

- **export TAG=0.1.<< pipeline.number >>**
    - Create an environment variable that incorporates the CircleCI [pipeline.number values][5] which serves as SemVer values that associate this image with this pipeline.
- **echo 'export TAG='$TAG >> /tmp/ecr/ecr_envars**
    - Saves the *$TAG* variable to the */tmp/ecr/ecr_envars* file for future use.
- **source /tmp/ecr/ecr_envars**
    - [sources][6] the */tmp/ecr/ecr_envars* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **docker build -t $ECR_PUBLIC_URI -t $ECR_PUBLIC_URI:$TAG .**
    - [Builds and tags][7] the Docker image to push to the AWS ECR. The image build will be conducted according to the projects existing [Dockerfile][8] in the root of the repo.

**- run: name: Push to AWS ECR Public** block has multiple commands listed in the **command:** element and we'll address them individually

- **source /tmp/ecr/ecr_envars**
    - [sources][6] the */tmp/ecr/ecr_envars* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **aws ecr-public get-login-password --region us-east-1 | docker login ---username AWS --password-stdin $ECR_URL**
    - Executes the login commands from the AWS CLI that authorizes and grants the job access to the AWS ECR.
- **docker push $ECR_PUBLIC_URI**
    - Executes the [Docker push][9] command which uploads the image to the AWS ECR.

**- persist_to_workspace:** is a special key that represents a [CircleCI Workspace][13]. When a workspace is declared in a job, files and directories can be added to it. Each addition creates a new layer in the workspace filesystem. Downstream jobs can then use this workspace for their own needs or add more layers on top. Workspaces are not shared between pipeline runs. The only time a workspace can be accessed after the pipeline has run is when a workflow is rerun within the 15 day limit.

Congratulations! You have created a new pipeline job that builds and pushes your Docker image to the AWs ECR.

## Module Summary

In this module you learned about Infrastructure as Code concepts and Hashicorp Terraform. You also learned how to create pipeline jobs that builds and AWS ECR and executes essential docker commands to build a Docker image and push it to the respective AWS ECR.

At the end of this section your config.yml should be identical to this code snippet:

{{<highlight yaml>}}
version: 2.1
orbs:
  snyk: snyk/snyk@0.1.0
  aws-cli: circleci/aws-cli@2.0.2
  node: circleci/node@4.2.0
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
  create_ecr_repo:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - run:
          name: Create .terraformrc file locally
          command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
      - terraform/install:
          terraform_version: "0.14.10"
          arch: "amd64"
          os: "linux"
      - run:
          name: Create ECR Repo
          command: echo 'Create AWS ECR Repo with Terraform'
      - terraform/init:
          path: ./terraform/ecr
      - terraform/apply:
          path: ./terraform/ecr
      - run: 
          name: "Retrieve ECR URIs"
          command: |
            cd ./terraform/ecr
            mkdir -p /tmp/ecr/
            terraform init
            echo 'export ECR_NAME='$(terraform output ECR_NAME) >> /tmp/ecr/ecr_envars
            export ECR_PUBLIC_URI=$(terraform output ECR_URI)
            echo 'export ECR_PUBLIC_URI='$ECR_PUBLIC_URI >> /tmp/ecr/ecr_envars
            echo 'export ECR_URL='$(echo ${ECR_PUBLIC_URI:1:-1} | cut -d"/" -f1,2) >> /tmp/ecr/ecr_envars
      - persist_to_workspace:
          root: /tmp/ecr/
          paths:
            - "*"
  build_push_docker_image:
    docker:
      - image: cimg/node:14.16.0
    steps:
      - checkout
      - setup_remote_docker
      - attach_workspace:
          at: /tmp/ecr/      
      - aws-cli/install
      - aws-cli/setup:
          aws-access-key-id: AWS_ACCESS_KEY_ID
          aws-secret-access-key: AWS_SECRET_ACCESS_KEY
      - run:
          name: Build Docker image
          command: |
            export TAG=0.1.<< pipeline.number >>
            echo 'export TAG='$TAG >> /tmp/ecr/ecr_envars
            source /tmp/ecr/ecr_envars
            docker build -t $ECR_PUBLIC_URI -t $ECR_PUBLIC_URI:$TAG .
      - run:
          name: Push to AWS ECR Public
          command: |
            source /tmp/ecr/ecr_envars
            aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_URL
            docker push $ECR_PUBLIC_URI
      - persist_to_workspace:
          root: /tmp/ecr/
          paths:
            - "*"

{{</highlight>}}

You have completed this module, Now jump over to the next module **Continuos Deployment**, where you will learn how to:

- Create AWS App Runner infrastructure
- Deploy the Docker image
- Test the image deployment using smoke-tests
- Destroy all the infrastructure and resources using Terraform

<!-- URL Links index -->
[1]: https://circleci.com/docs/2.0/configuration-reference/#setupremotedocker
[2]: https://circleci.com/docs/2.0/building-docker-images/
[3]: https://circleci.com/docs/2.0/persist-data/#using-workspaces
[4]: https://circleci.com/developer/orbs/orb/circleci/aws-cli
[5]: https://circleci.com/docs/2.0/pipeline-variables/#pipeline-values
[6]: https://linuxize.com/post/bash-source-command/#:~:text=The%20source%20command%20reads%20and,Linux%20and%20UNIX%20operating%20systems.
[7]: https://docs.docker.com/engine/reference/commandline/build/
[8]: https://docs.docker.com/engine/reference/builder/
[9]: https://docs.docker.com/engine/reference/commandline/push/
