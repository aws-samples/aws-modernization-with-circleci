---
title: "1.4 Storing tests in S3"
chapter: true
weight: 16
---

## Step 1 &mdash; Setup Amazon S3

1. Sign into the [AWS Console](https://console.aws.amazon.com/console/home)

2. Create an S3 bucket in your preferred region.

3. Attach a [public bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-policy-language-overview.html) to the bucket.

![](https://tituschoi.com/wp-content/uploads/2017/08/aws-s3-bucket-policy.png)

4. Navigate to **Properties**.

5. Under **Static website hosting**, select Edit.

![](https://4sysops.com/wp-content/uploads/2019/04/Static-website-hosting-option.png)

6. Select **Use this bucket to host a website**.

![](https://i.stack.imgur.com/m1MQ9.png)

## Step 2 &mdash; Store tests in Amazon S3

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