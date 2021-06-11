---
title: "4.3 Build a Docker Image Job"
chapter: true
weight: 14
---

## Docker Images and Containers

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

A Docker image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, and includes the configuration details needed to make your application run.

You might create your own images or you might only use images created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers that have changed are rebuilt. This is part of what makes images so lightweight, small, and fast when compared to other virtualization technologies.

A Docker container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.

In this workshop section, you will create a Docker image for the project application, so you will need to create a CI/CD job that builds an image and uploads/pushes it to the AWS Public ECR you created earlier.

## Docker Image build job

In this section, you will learn how to create a Docker Image based on the project application. Then you will learn how to push it to an AWS ECR from within a CI/CD job.

The following code snippet shows how to define and provision a job that builds and pushes a Docker Image within your CI/Cd pipeline.

Copy this code snippetand append it to the bottom of your config.yml file:

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

You should already be familiar with the *docker:*, *step:s* and *checkout* job elements so we'll skip discussing them and focus on the remaining *- run:* elements in this job.

The **- setup_remote_docker** block creates [a remote Docker environment][1] configured to execute Docker commands. See [Running Docker Commands][2] for details. This essentially provides the Docker executor access to the Docker CLI which in turn, enables a Docker image build with a Docker container.

The **- attach_workspace:** block attaches the [CircleCI Workspace][3] that you created in the previous job. This command will provide the job access to the data saved earlier.

**- aws-cli/install** uses the [AWS CLI orb][4] which provides the job access to the AWS CLI to execute AWS-centric commands. 

**- aws-cli/setup:** uses the [AWS CLI orb][4] and its *setup* command to initialize the AWS CLI to access AWS services with the AWS credentials defined in the *$AWS_ACCESS_KEY_ID* and *$AWS_SECRET_ACCESS_KEY* project environment variables

The **- run: name: Build Docker image** block has multiple commands listed in the **command:** element. I will address them individually:

- **export TAG=0.1.<< pipeline.number >>**
    - Create an environment variable that incorporates the CircleCI [pipeline.number values][5] that serves as SemVer values that associate this image with this pipeline.
- **echo 'export TAG='$TAG >> /tmp/ecr/ecr_envars**
    - Saves the *$TAG* variable to the */tmp/ecr/ecr_envars* file for future use.
- **source /tmp/ecr/ecr_envars**
    - [sources][6] the */tmp/ecr/ecr_envars* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **docker build -t $ECR_PUBLIC_URI -t $ECR_PUBLIC_URI:$TAG .**
    - [Builds and tags][7] the Docker image to push to the AWS ECR. The image build will be conducted according to the projects existing [Dockerfile][8] in the root of the repo.

The **- run: name: Push to AWS ECR Public** block has multiple commands listed in the **command:** element. I will address them individually:

- **source /tmp/ecr/ecr_envars**
    - [sources][6] the */tmp/ecr/ecr_envars* file which reads and executes commands from the file specified as its argument in the current shell environment.
- **aws ecr-public get-login-password --region us-east-1 | docker login ---username AWS --password-stdin $ECR_URL**
    - Executes the log-in commands from the AWS CLI that authorize and grant the job access to the AWS ECR.
- **docker push $ECR_PUBLIC_URI**
    - Executes the [Docker push][9] command, which uploads the image to the AWS ECR.

Congratulations! You have created a new pipeline job that builds and pushes your Docker image to the AWs ECR.

## Summary

In this module you learned about Infrastructure as Code concepts and Hashicorp Terraform. You also learned how to create pipeline jobs that build an AWS ECR and how to execute essential Docker commands to build a Docker image and push it to the respective AWS ECR.

Jump over to the next module, Continuous Deployment, where you will learn how to:

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
