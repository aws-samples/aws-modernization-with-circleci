             ---
title: "1.4 AWS: Create Access Keys"
chapter: true
weight: 16
---

## Create AWS Access Keys and Secrets

Earlier, you [logged into your Event Engine dashboard][1]. Now you must grab your [AWS access keys and secrets][2] that will enable you to access and interact with AWS programmatically, which is also essential for integrating AWS into your pipeline.

When you try to access your AWS console via Event Engine, you should see this:

![Access key](/images/ee-access-key.png)

Copy the values for:

- **AWS_ACCESS_KEY_ID**
- **AWS_SECRET_ACCESS_KEY**
- **AWS_SESSION_TOKEN** 

to a local notepad or notes application. You will need these values so CircleCI can access your EE AWS account to create the CICD pipeline!

Also, click the SSH Key button on the top right of the dashboard. This will show you the private key string as well as the keypair name. Save the name as well, it should be `ee-default-keypair`
    ![img.png](/images/keypair.png)

{{% notice info %}}
**Store the secret access key** in a safe place. Once you click out of the box, that key will **no longer** be accessible. You will have to delete that access key pair and generate a new one.
{{% /notice %}}

You have safely stored your newly created AWS Access Keys and Secrets. Now, let's fork the GitHub example project repository for this workshop.

<!-- URL Links index -->
[1]: /020_event_engine_setup/20_aws_event_engine.html
[2]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[3]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account