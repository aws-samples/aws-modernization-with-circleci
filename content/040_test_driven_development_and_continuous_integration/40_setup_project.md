---
title: "1.1. Creating a new project"
chapter: true
weight: 10
---
Infrastructure at CircleCI is organized into [projects](https://circleci.com/docs/2.0/project-build/). Each project is a single program that, when run, declares the desired infrastructure for us to manage. 

## Step 1 &mdash; Set up SSH with your GitHub account
```bash
# This is to generate a SSH public key on your Cloud9 machine
# Press enter 3 times: do not rename the public key filename nor enter the passphrase
ssh-keygen -t ed25519 -C "your_email@example.com"

# We're copying the public key value onto your clipboard 
cat ~/.ssh/id_ed25519.pub
```
1. Go [to your Github](https://github.com/) 
2. Go to your settings in top right corner and go to the SSH section
3. Paste your public key value
   
    ![ssh-key](/images/ssh-key.png)
   
4. Click Add SSH key
5. You have now told your GitHub to trust the local machine (Cloud9 instance) that contains your public key!

## Step 2 &mdash; Fork demo React application
 
1. We are going to fork over [a sample React application](https://github.com/CircleCI-Public/aws-workshop-demo-app/)
2. The fork button is on the top right hand corner of your browser, under your account:
    
    ![fork-repo](/images/fork-repo.png)

3. Now that you have forked over the sample application, git clone it to your Cloud9 instance via **SSH** 

```bash
# Command to clone the repo via SSH
git clone git@github.com:{GithubUserName}/aws-workshop-demo-app.git
```

4. Next add a directory for our CircleCI configurations by running:

```bash
# This directory is where config.yml file will live
mkdir .circleci/
```
## Step 3 &mdash; Inspect your new project

Our project comprises multiple files:
- **`App.js`** is your program's main entry point file
- **`package.json`** is your project's dependency information
- **`.circleci`** is your project's pipeline configuration
- **`App.test.js`** is where your apps tests will reside

Run `npm start` in the  directory to see the contents of your project's empty program.
Feel free to explore the other files, although we will not be editing any of them manually.

## Step 4 &mdash; Run npm install
1. Now that you have a package.json, we need install the dependencies on there.
```bash
npm install
```
2. This will take a few minutes, and you should a node_module folder created as a result.

## Step 5 &mdash; Adding the project to CircleCI

1. Now go to your [CircleCI dashboard](https://circleci.com/vcs-authorize/). If you haven't signed up at this point, log in with your GitHub credentials.
2. ![CircleCI Login](/images/circleci-signup.png) 
3. On the left-hand side, click the Projects section and will land here:
4. ![CircleCI Projects Dashboard](/images/circleci-project-dashboard.png)
5. Click **Set Up Project** (next to your new project).
6. This opens the **New Project Set Up** page, select the **Skip this step** option.
7. ![Skip Config](/images/skip-config.png)

## Step 6 &mdash; Configure environment variables

On your CircleCI dashboard, navigate to `Project Settings > Environment Variables` in the CircleCI control panel and add these keys:

```bash
Name: AWS_ACCESS_KEY_ID 
Value: Your AWS IAM Accounts Access Key ID

Name: AWS_SECRET_ACCESS_KEY 
Value: Your AWS IAM Accounts Secret Access Key
```

![Add Env Variable](/images/add-env-var.png)

You may be wondering where you're going to this information! Earlier, you [created your AWS account](30_aws_setup_your_own.html). 

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