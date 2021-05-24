---
title: "2.5 Setup static hosting"
chapter: true
weight: 18
---

## Step 1 &mdash; Configure S3

To deploy to S3, we need to create an IAM user that can write to our bucket. The following security policy will allow CircleCI to send files to the bucket:

```JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::your-bucket-name",
                "arn:aws:s3:::your-bucket-name/*"
            ]
        }
    ]
}
```
Make note of the **Access Key ID** and **Secret Access Key** of this user. To provide this information to CircleCI, access your project’s build settings by clicking the cog next to the project name on the Builds page. On the settings page, locate the AWS Permissions link under the Permissions section. Then add the credentials for our IAM user.

## Step 2 &mdash; Deploy to S3

We will use CircleCI’s `deploy` command to ship the code. `deploy` works just like the `run` command, but is used instead of `run` when deploying code.

```YAML
- deploy:
    name: deploy to AWS
    command: |
        if [ "${CIRCLE_BRANCH}" = "master" ]; then
        aws s3 sync $HUGO_BUILD_DIR \
            s3://your-bucket-name/your-subfolder --delete
        else
              echo "Not master branch, dry run only"
        fi
```

We're now completed adding continuous vulnerability and test coverage analysis, it's now time to set up the pipeline to deploy the application on S3.