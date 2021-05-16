---
title: "1.4 Storing tests in S3"
chapter: true
weight: ADD WEIGHT
---

# Setup Amazon S3

1. Create an S3 bucket in your desired region

2. Add 

# Store tests in Amazon S3

1. Open the `config.yaml` file

2. Add a section for storing the test data:

```YAML
steps:
     - store_artifacts:
          path: test-results
      - deploy:
          command: 
              mv -- test-results $CIRCLE_ARTIFACTS/$ARTIFACT_BUILD
              aws s3 cp $CIRCLE_ARTIFACTS/$ARTIFACT_BUILD s3://test-results-bohrman/test-data/ --metadata {\"git_sha1\":\"$CIRCLE_SHA1\"}
```