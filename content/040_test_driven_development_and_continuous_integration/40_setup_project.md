---
title: "1.1. Creating a new project"
chapter: true
weight: 10
---

Infrastructure at CircleCI is organized into [projects](https://circleci.com/docs/2.0/project-build/). Each project is a single program that, when run, declares the desired infrastructure for us to manage.

## Step 1 &mdash; Create a new project

We're going to leverage React in our demo application for this project. Let's create a React project now and `cd` into the directory:

```
npx create-react-app circleci-app-demo
cd circleci-app-demo
```

## Step 2 &mdash; Initialize a Git repository

Once we have our React application set up, we're going to want to [create a GitHub repository](https://docs.github.com/en/github/getting-started-with-github/quickstart/create-a-repo) via the UI for this project. Once that's finished let's run these commands to initialize Git:

1. Fork from https://github.com/eugenemu/aws-workshop-demo-app.git to your GitHub account
1. 

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
cat ~/.ssh/id_ed25519.pub
git clone git@github.com:{yourGithubUserName}/circleci-demo-app.git
git branch -M main
git push -u origin main
```

Let's also add a directory for our CircleCI configurations by running:

```bash
mkdir .circleci
```

## Step 3 &mdash; Inspect your new project

Our project comprises multiple files:

* **`App.js`** is your program's main entrypoint file
* **`package.json`** is your project's Python dependency information
* **`.circleci`** is your projects configuration and metadata
* **`App.test.js`** is where your apps tests will reside

Run `npm start` in the `/src/` directory to see the contents of your project's empty program.

Feel free to explore the other files, although we will not be editing any of them manually.

## Step 4 &mdash; Adding the project to CircleCI

After you have pushed the new project to Github, create a project for it in CircleCI by opening the app and connecting your account. Log in with your GitHub login:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-login.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

If you belong to multiple GitHub organizations, select the one that applies to your project:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-choose-org.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

You will get a list of GitHub projects for your organization:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-org-projects.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

Click **Set Up Project** (next to your new project):

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-setup-project-coveralls-demo-ruby.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

This opens the **New Project Set Up** page:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-project-ready-prompt.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

Everything from here is pretty self-service, so we can move on to the next section.