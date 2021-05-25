---
title: "1.4 Storing tests in S3"
chapter: true
weight: 16
---

## Step 1 &mdash; Setup Amazon S3

1. Create an S3 bucket in `us-east-1`.

2. Attach a [public bucket policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-policy-language-overview.html) to the bucket.

![](https://tituschoi.com/wp-content/uploads/2017/08/aws-s3-bucket-policy.png)

3. Navigate to **Properties**.

4. Under **Static website hosting**, select Edit.

![](https://4sysops.com/wp-content/uploads/2019/04/Static-website-hosting-option.png)

5. Select **Use this bucket to host a website**.

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
              export ARTIFACT_BUILD=$CIRCLE_PROJECT_REPONAME-$CIRCLE_BUILD_NUM.zip
              zip -r $ARTIFACT_BUILD test-results/
              aws s3 cp $ARTIFACT_BUILD s3://test-results-bohrman --metadata {\"git_sha1\":\"$CIRCLE_SHA1\"}
```

That's it for this module, we've learned how to add some simple tests and automate their integration, as well as storing the output in Amazon S3 and locally. Let's move on to the next module where we will learn how to implement coverage analysis and how to implement security scanning in our pipelines.