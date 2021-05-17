---
title: "2.3 Implementing security scans"
chapter: true
weight: ADD WEIGHT
---

# Implementing Security scans

Now we're going to add some DevSecOps to our pipeline by implementing OSS vulnerability scanning with FOSSA. 

## Setup environment variables

First, grab a [FOSSA API Key](https://docs.fossa.com/docs/api-reference) from your FOSSA account under your [Integration Settings](https://app.fossa.io/account/settings/integrations/api_tokens).

{{% notice info %}}
If you are the maintainer of a public repository you should consider making your API key a [Push Only Token](https://docs.fossa.com/docs/api-reference#section-push-only-api-token).
{{% /notice %}}

![](https://files.readme.io/7b30baf-Screen_Shot_2018-03-30_at_11.50.46_AM.png)

## Add FOSSA steps to `config.yml`

Once the environment variable is ready, it's time to edit your `.circleci/config.yml` file.

First, add a step to install `fossa-cli` when your build starts. Usually the best place to include this is right before the `checkout` step of your `build` job when you're still installing the environment pre-reqs:

```YAML
jobs:
  build:
    ...
    steps: 
      - run: |
          curl -H 'Cache-Control: no-cache' https://raw.githubusercontent.com/fossas/fossa-cli/master/install.sh | bash
      - checkout
```
Next, add a step to run the `fossa` command you just installed in order to upload dependency data from your CircleCI build:

```YAML
- run:
    command: fossa
    working_directory: $YOUR_CODE_DIRECTORY
```

## Blocking CI Builds w/ FOSSA Issue Status

You an also create a step in CircleCI that will allow you to pass/fail a build based off your scan status in FOSSA.

![](https://files.readme.io/07e7362-Screen_Shot_2018-03-30_at_12.02.25_PM.png)

To accomplish this, simply add a call to fossa test into your test section.

```YAML
- run:
    command: fossa test
    working_directory: <repo_dir>
```
The fossa test command will poll app.fossa.io or your local FOSSA appliance for updates on your scan status until it gets a response. 

Then, it will report a relevant exit status to the CI step (to block a failing build) and render rich details about issues directly inline your CircleCI test results.