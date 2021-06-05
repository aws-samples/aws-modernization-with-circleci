---
title: "1.4 AWS: Create Access Keys"
chapter: true
weight: 16
---

## Create AWS Access Keys and Secrets 

Earlier, you [created your AWS account][1] and now you must create [AWS Access Keys and Secrets][2] that will enable you to access and interact with AWS programmatically, which is also essential for integrating AWS into your pipeline.

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

You have created and safely stored your newly created AWS Access Keys and Secrets, Now, let's fork the GitHub example project repository for this workshop.

<!-- URL Links index -->
[1]: /030_self_guided_setup/30_aws_setup_your_own.html
[2]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[3]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account