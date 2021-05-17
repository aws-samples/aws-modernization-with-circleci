---
title: "1.3 Automating test integration"
chapter: true
weight: ADD WEIGHT
---

# Automate Testing with CircleCI

1. Navigate to `/demo-app/` and open `.circleci`

2. Add a file called `config.yaml`

3. Create your test automation workflow:

```YAML
jobs:
  test:
    docker:
      - image: circleci/node:9.9.0
        auth:
          username: $DOCKERHUB_USERNAME
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    steps:
      - checkout
      - run: echo "this is the test job"
      - run: npm install
      - run:
          name: Run tests with JUnit as reporter
          command: npm test --ci --runInBand --reporters=default --reporters=jest-junit
          environment:
            JEST_JUNIT_OUTPUT_DIR: test-results
```

# Store tests locally

