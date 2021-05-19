---
title: "2.2 Implement continuous test coverage analysis"
chapter: true
weight: 12
---

# Add step for test coverage

Now that we've added another test, let's add a `store_artifacts` step to track our test coverage:

```YAML
    steps:
      - store_artifacts:
          path: coverage
```

# Setting a threshold for test coverage

The ultimate goal of continuous integration is continuous deployment and the ability to deploy with confidence. Let's implement this in our pipeline that will enable this by adding coverage threshholds which will stop deployments when these thresholds are not met.

## Setting up CodeCov

First, we will link [Github and CodeCov](https://codecov.io/gh)

![](https://res.cloudinary.com/practicaldev/image/fetch/s--OLqX5Cx6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/5424/1%2ACRJK8E1Vxa4ZaApgLbZI0w.png)

We will then will need to create an environment variable in the CircleCI control panel called CODECOV_TOKEN. You will find your token directly when you add the repository to CodeCov.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--3K3VQSMY--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/4096/1%2Awc9DhJlo4GVU0FmGCFY6pQ.png)

## Add `codecov.yml`

Now you're going to want to add a YAML file to your project to configure the threshholds for CodeCov. Create a file called `codecov.yml` and add this config into it:

```YAML
codecov:
  require_ci_to_pass: no

coverage:
  status:
    project:
      default:
        target: 80%    # the required coverage value
        threshold: 1%  # the leniency in hitting the target
```

## Add step to `config.yaml`

Then, we need to customize our `config.yml` to add the `Send to codecov` step:

```YAML
- run:
    name: Send to codecov
    command: |
      bash <(curl -s https://codecov.io/bash) -Z
```