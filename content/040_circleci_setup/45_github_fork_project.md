---
title: "1.6 GitHub: Fork Example Project"
chapter: true
weight: 18
---

## GitHub

GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere. In this section, you will fork the [example repository][1] for this workshop so we can authorize CircleCI to monitor the repository in future modules.

## Set up SSH with your GitHub account

You can connect to GitHub using SSH. Using the SSH protocol, you can connect and authenticate to remote servers and services. With SSH keys, you can connect to GitHub without supplying your username and personal access token at each visit. Because you are using Cloud9 IDE, you need to create new SSH keys for the Cloud9 IDE host. The following direction can be used to create new SSH on any host or machine that you intend to access GitHub using the SSH protocol. You can learn more about [accessing GitHub with SSH here][2].

{{% notice info %}}
**Note:** When creating these SSH keys, **do not** specify or enter an SSH passphrase. If you set an SSH passphrase, you will have to enter it every time you access GitHub with it. When accessing GitHub programmatically, the application will be prompted for the passphrase, which stops the execution until the passphrase is provided. For that reason, do not set the passphrase for these SSH keys.
{{% /notice %}}

Execute the code snippet below to create the SSH keys to access GitHub:

```bash
# This is to generate a SSH public key on your Cloud9 machine
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press enter 3 times: do not rename the public key filename nor enter the passphrase.

Next, execute the below command and copy the output
```
# We're copying the public key value onto your clipboard 
cat ~/.ssh/id_ed25519.pub
```


1. Go [to your Github](https://github.com/) 
2. Go to your settings in top right corner and go to the SSH section
3. Paste your public key value
    ![ssh-key](/images/ssh-key.png)
4. Click Add SSH key
5. You have now told your GitHub to trust the local machine (Cloud9 instance) that contains your public key!

There are more details about [adding SSH Keys to GitHub here][3].

## Step 1 &mdash; Fork example application
 
1. We will fork over [this workshop's project repository][1]

2. The fork button is on the top right-hand corner of your browser, under your account:
    
3. Now that you have forked over the project repo, git clone it to your Cloud9 instance via **SSH** 

The {YourGithubUserName} represents your GitHub user name
```bash
# Command to clone the repo via SSH
git clone git@github.com:{YourGithubUserName}/aws-circleci-modernization-workshop-code.git
```

Now that you have created new SSH Keys, assigned them in GitHub, and forked over the project repo, you can start setting up this project repo in CircleCI.

<!-- URL Links index -->
[1]: https://github.com/CircleCI-Public/aws-circleci-modernization-workshop-code
[2]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[3]: https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account