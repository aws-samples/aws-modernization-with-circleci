---
title: "3.3 Automate Elastic Beanstalk deployment"
chapter: true
weight: ADD WEIGHT
---

## Step 1 &mdash; Configure `config.yaml`

Add the awsebcli dependency:

```YAML
dependencies:
  pre:
    - sudo apt-get update
    - sudo apt-get install python-dev
    - sudo pip install awsebcli
```

Add the deployment config:

```YAML
deployment:
  production:
    branch: master
    commands:
      - bash ./setup-eb.sh
      - eb deploy
```