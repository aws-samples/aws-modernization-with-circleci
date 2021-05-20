---
title: "1.1. Creating a new project"
chapter: true
weight: 10
---

Infrastructure at CircleCI is organized into projects. Each project is a single program that, when run, declares the desired infrastructure for us to manage.

## Step 1 &mdash; Create a new project

Each CircleCI project lives in its own directory. Create one now and change into it:

```
create-react-app $PROJECT_NAME
cd $PROJECT_NAME
```

## Step 2 &mdash; Initialize a Git repository

We need to set up a Git repo for this project, so `cd` into the root directory and run `git init`.

Then, create a new Git repo via the UI. Run these commands:

```bash
git remote add origin https://github.com/$GIT_USERNAME/$PROJECT_NAME.git
git branch -M main
git push -u origin main
```

## Step 3 &mdash; Inspect your new project

Our project is comprised of multiple files:

* **`app.js`** is your program's main entrypoint file
* **`package.json`** is your project's Python dependency information
* **`.circleci`** is your projects configuration and metadata
* **`app.test.js`** is where your apps tests will reside

Run `node app.js` to see the contents of your project's empty program.

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