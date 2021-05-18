---
title: "3.3 Setup Elastic Beanstalk environment"
chapter: true
weight: ADD WEIGHT
---

# Configure `config.yaml`

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