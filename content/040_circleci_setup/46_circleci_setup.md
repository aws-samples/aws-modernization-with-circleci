---
title: "1.6 CircleCI: Setup Project"
chapter: true
weight: 20
---

In the previous section, you forked the [example project repository][1] in GitHub and next you will learn how to add that project to CircleCI. 

## Step 1 &mdash; Adding the project to CircleCI

1. Now go to your [CircleCI dashboard](https://circleci.com/vcs-authorize/). If you haven't signed up at this point, log in with your GitHub credentials.
2. ![CircleCI Login](/images/circleci-signup.png) 
3. On the left-hand side, click the Projects section and you will land here:
4. ![CircleCI Projects Dashboard](/images/circleci-project-dashboard.png)
5. Click **Set Up Project** next to the name of your new project.
6. This opens the **New Project Set Up** page, select the **Skip this step** option.
7. ![Skip Config](/images/skip-config.png)

<!-- Start added new content here Angel -->

## Step 1 &mdash; Configure project environment variables

On your CircleCI dashboard, navigate to `Project Settings > Environment Variables` in the CircleCI control panel and add these keys:

```bash
Name: AWS_ACCESS_KEY_ID 
Value: Your AWS IAM Accounts Access Key ID

Name: AWS_SECRET_ACCESS_KEY 
Value: Your AWS IAM Accounts Secret Access Key
```

![Add Env Variable](/images/add-env-var.png)

You may be wondering where you're going to get this information! Earlier, you [created your AWS account](30_aws_setup_your_own.html). 

In your AWS Console, navigate to `IAM > User > your user > Security credentials tab`. 

Your screen should look similar to this:

![Access Keys](/images/iam-user-screen.png)

Click the Create access key button

{{% notice info %}}
**Please store the secret access key**, once you click out of the box, that key will **NO LONGER** be accessible. You will have to delete that access key pair and generate new ones.
{{% /notice %}}

After generating the access keys, your dashboard should look something like this:
![Create Access Key](/images/access-key.png)

{{% notice warning %}}
These access keys are part of an IAM User that has **administrative privileges**, do not share these with anyone, or they will have access your AWS account with admin privileges.
{{% /notice %}}

<!-- URL Links index -->
[1]: https://github.com/aws-samples/aws-modernization-with-circleci
[2]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[3]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account