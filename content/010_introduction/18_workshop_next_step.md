---
title: "Workshop Next Steps"
chapter: true
weight: 18
---

## Workshop Overview

This workshop will guide you through creating a robust CI/CD pipeline using CircleCI and in this section we'll discuss how the overall flow of this workshop in order to familiarize you with the process. 

### CircleCI config.yml file
While working through the workshop modules, you will progressively build a CI/CD pipeline in the form of a CircleCI [config.yml][1] file. CircleCI believes in configuration as code. As a result, the entire delivery process from build to deploy is orchestrated through a single file called [config.yml][1]. The config.yml file is located in a folder called **.circleci/** at the top of your project. CircleCI uses the [YAML syntax][2] for defining pipeline configurations.

### Programming the config.yml

As previously mentioned, you will progressively build a fully functional [config.yml][1] file that will ultimately demonstrate how to build, test, security scan and deploy code changes with every commit. We will provide all of the code snippets elements, with explanations, you will need to complete the config.yml file through the modules. The code snippets will look similar to this example:

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
{{</highlight>}}

This example is the beginning of a config.yml pipeline and you will append more code snippet examples to the config.yml as you progress through the modules. After completing all the modules you will have a complete CI/CD pipeline that will execute on CircleCI.

{{% notice warning %}}
YAML requires indentation to properly structure elements and also requires those indentations to be **spaces** and not **Tabs**. Improper indentation will created formatting errors rendering your YAML invalid until fixed.
{{% /notice %}}


<!-- URL Links index -->
[1]: https://circleci.com/docs/2.0/config-intro/#getting-started-with-circleci-config
[2]: https://circleci.com/docs/2.0/writing-yaml/


{{% children showhidden="false" %}}