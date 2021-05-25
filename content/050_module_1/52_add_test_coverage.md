---
title: "2.2 Implement continuous test coverage analysis"
chapter: true
weight: 12
---

In this section we will be using a tool called CodeCov to implement continuous test coverage analysis of our application. This is an important practice because it allows teams to ensure that their tests are testing all of their code and how much of their code of being exercised. 

## Step 1 &mdash; Add step for test coverage

Now that we have added another test, we can add a `store_artifacts` step to track our test coverage:

```YAML
    steps:
      - store_artifacts:
          path: coverage
```

## Step 2 &mdash; Setting a threshold for test coverage

The ultimate goal of continuous integration is continuous deployment and the ability to deploy with confidence. Implementing this in our pipeline will help us meet this goal by adding coverage threshholds. When thresholds are not met, the pipeline stops the deployment.

## Step 3 &mdash; Setting up CodeCov

First, link [Github and CodeCov](https://codecov.io/gh)

![](https://res.cloudinary.com/practicaldev/image/fetch/s--OLqX5Cx6--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/5424/1%2ACRJK8E1Vxa4ZaApgLbZI0w.png)

Next, create an environment variable in the CircleCI control panel called CODECOV_TOKEN. You will find your token directly when you add the repository to CodeCov.

![](https://res.cloudinary.com/practicaldev/image/fetch/s--3K3VQSMY--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://cdn-images-1.medium.com/max/4096/1%2Awc9DhJlo4GVU0FmGCFY6pQ.png)

## Step 4 &mdash; Add `codecov.yml`

Now we need to add a YAML file to your project to configure the thresholds for CodeCov. Create a file called `codecov.yml` and add this config into it:

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

## Step 5 &mdash; Add step to `config.yaml`

Then, we need to customize our `config.yml` to add the `Send to codecov` step:

```YAML
- run:
    name: Send to codecov
    command: |
      bash <(curl -s https://codecov.io/bash) -Z
```

In this section, we've learned how to leverage CodeCov to implement continuous coverage analysis in our pipelines. Let's move on to the next section where we will add some vulnerability analysis with FOSSA. 