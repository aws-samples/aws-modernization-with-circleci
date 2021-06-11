---
title: "2.2 Testing"
chapter: true
weight: 12
---

## Testing using CircleCI

All code must be tested to minimize unexpected behaviors, errors and potential bugs in software that could have severe negative consequences on critical applications and services. Testing code is very easy to accomplish in CircleCI by essentially triggering your automated tests whenever you change and push code to an upstream repository. Before you can execute your killer test scripts in a pipeline, you will need to create a new pipeline for this workshop,

## Pipeline Skeleton in config.yml

The [project repo][1] for this project has an existing config.yml file in the .circleci/ directory which you will be modifying throughout the sections of this workshop.

The first thing, open the config.yml in an editor of choice, select all the text in the file and delete it.

Next copy and paste the following code snippet into your config.yml file:

{{<highlight yaml>}}
version: 2.1
jobs:
workflows:
{{</highlight>}}

The code snippet above is a bare bones skeleton of a pipeline. Executing this as is will definitely result in a "build" error on CircleCI, but the intention here is to show a realistic starting point for building a job that will execute tests on the code. As you progress you will fill in the skeleton with jobs and other elements that will result in a fully functional pipeline.

In the next section you will build a pipeline job that executes unit tests when the code is modified.


<!-- URL Links index -->
[1]: https://github.com/CircleCI-Public/aws-circleci-modernization-workshop-code


