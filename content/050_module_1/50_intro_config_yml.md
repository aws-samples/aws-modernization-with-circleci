---
title: "2.2 Intro to config.yml"
chapter: true
weight: 10
---

## Intro to config.yml file

As previously mentioned, while working through the workshop modules, you will progressively build a CI/CD pipeline in the form of a CircleCI [config.yml][1] file. CircleCI is a proponent of configuration as code. As a result, the entire delivery process from build to deploy is orchestrated through a single file called [config.yml][1]. The config.yml file is located in a folder called **.circleci/** at the top of your project. CircleCI uses the [YAML syntax][2] for defining pipeline configurations.

## Programming the config.yml

You will progressively build a fully functional [config.yml][1] file that will ultimately demonstrate how to build, test, security scan. and deploy code changes with every commit. We will provide all of the code snippet elements, with explanations, you will need to build the config.yml file through the modules. The code snippets will look similar to this example:

{{<highlight yaml>}}
version: 2.1
jobs:
  test:
    docker:
      - image: circleci/node:14.0
    steps:
      - checkout
      - run: npm install
{{</highlight>}}

This example is the beginning of a config.yml pipeline and you will append more code snippet examples to the config.yml as you progress through the modules. After completing all the modules you will have a complete CI/CD pipeline that will execute on CircleCI.

## Core elements of the config.yml

Let's take moment to learn about the most critical elements within the config.yml file so that you have a better understanding of how to define effective CI/CD pipelines.

The **config.yml** is composed of four core elements:

- [Executors][3]: Executors define the environment in which the steps of a job will be run, allowing you to reuse a single executor definition across multiple jobs.
- [Jobs][4]: Jobs are a grouping of [commands][5] the produce a desired result when executed.
- [Commands][5]: Commands are the directives to be executed such as an **npm install** or unit test execution.
- [Workflows][6]: Workflows orchestrates how and when jobs are executed when pipelines are triggered.

As you progress through this workshop you should gain familiarity and a better understanding of these elements and how they function.

{{% notice warning %}}
YAML requires indentation to properly structure elements and also requires those indentations to be **spaces** and not **Tabs**. Improper indentation will created formatting errors rendering your YAML invalid until fixed.
{{% /notice %}}

In the next you will learn how create the begginings of a config.yml file and implement automated testing job.

<!-- URL Links index -->
[1]: https://circleci.com/docs/2.0/config-intro/#getting-started-with-circleci-config
[2]: https://circleci.com/docs/2.0/writing-yaml/
[3]: https://circleci.com/docs/2.0/configuration-reference/#executors-requires-version-21
[4]: https://circleci.com/docs/2.0/configuration-reference/#jobs
[5]: https://circleci.com/docs/2.0/configuration-reference/?section=reference#commands-requires-version-21
[6]: https://circleci.com/docs/2.0/configuration-reference/#workflows