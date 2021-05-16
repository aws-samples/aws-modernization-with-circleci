---
title: ""
chapter: true
weight: ADD WEIGHT
---

# Setup Amazon S3

1. Create an S3 bucket in your desired region

2. Add 

# Store tests in Amazon S3

1. Open the `config.yaml` file

2. Add a section for storing the test data:

```
steps:
      - checkout
      - run: npm install
      - run: mkdir ~/junit
      - run:
          command: mocha test --reporter mocha-junit-reporter
          environment:
            MOCHA_FILE: ~/junit/test-results.xml
          when: always
      - store_test_results:
          path: ~/junit
      - store_artifacts:
          path: ~/junit
```