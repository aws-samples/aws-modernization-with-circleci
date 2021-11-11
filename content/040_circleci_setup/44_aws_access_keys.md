---
title: "1.4 AWS: Create Access Keys"
chapter: true
weight: 16
---

## Create AWS Access Keys and Secrets 

Earlier, you [created your AWS account][1]. Now you must create [AWS access keys and secrets][2] that will enable you to access and interact with AWS programmatically, which is also essential for integrating AWS into your pipeline.

In your AWS Console, navigate to **`IAM > User > your user > Security credentials`** tab. 

Your screen should look similar to this:

![Access Keys](/images/iam-user-screen.png)

Click the **Create access key** button.

{{% notice info %}}
**Store the secret access key** in a safe place. Once you click out of the box, that key will **no longer** be accessible. You will have to delete that access key pair and generate a new one.
{{% /notice %}}

After generating the access keys, your dashboard should look something like this:
![Create Access Key](/images/access-key.png)

{{% notice warning %}}
These access keys are part of an IAM User that has **administrative privileges**. Do not share these with anyone, or they will have access your AWS account with admin privileges.
{{% /notice %}}

## Create AWS Key Pair

You will need to create an [AWS EC2 Key Pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) which will grant Terraform access to provision the EC2 nodes in later sections.

1. Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.
1. In the navigation pane, under Network & Security, choose Key Pairs.
1. Choose Create key pair.
1. For Name, enter this value `ee-default-keypair` for the key pair
1. For Key pair type, choose **RSA** 
1. For Private key format, chose **.pem**
1. Click the **Create key pair** button

You have created and safely stored your newly created AWS Access Keys and Secrets and the **ee-default-keypair** AWS EC2 key pair. Now, let's fork the GitHub example project repository for this workshop.


<!-- URL Links index -->
[1]: /030_self_guided_setup/30_aws_setup_your_own.html
[2]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[3]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account