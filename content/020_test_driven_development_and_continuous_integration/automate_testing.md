---
title: ""
chapter: true
weight: ADD WEIGHT
---

# Automate Testing with CircleCI

1. Navigate to `/demo-app/` and open `.circleci`

2. Add a file called `config.yaml`

3. Create your test automation workflow:

```
jobs:
  build:
    docker:
      - image: circleci/node:9.9.0
        auth:
          username: $DOCKERHUB_USERNAME 
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    steps:
      - checkout
      - run: echo "this is the build job"
  test:
    docker:
      - image: circleci/node:9.9.0
        auth:
          username: $DOCKERHUB_USERNAME
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    steps:
      - checkout
      - run: echo "this is the test job"
workflows:
  build_and_test:
    jobs:
      - build
      - test
```

# Store tests locally

