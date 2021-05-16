---
title: "1.1. Creating a new project"
chapter: true
weight: ADD WEIGHT
---

Infrastructure at CircleCI is organized into projects. Each project is a single program that, when run, declares the desired infrastructure for us to manage.

## Step 1 &mdash; Create a new project

Each CircleCI project lives in its own directory. Create one now and change into it:

```
create-react-app $PROJECT_NAME
cd $PROJECT_NAME
```

## Step 2 &mdash; Initialize a Git repository

We're going to want to set up a git repo for this project, so `cd` into the root directory and run `git init`. 

Then you'll want to create a new git repo via the UI and run these commands:

```bash
git remote add origin https://github.com/$GIT_USERNAME/$PROJECT_NAME.git
git branch -M main
git push -u origin main
```

## Step 3 &mdash; Inspect Your New Project

Our project is comprised of multiple files:

* **`app.js`**: your program's main entrypoint file
* **`package.json`**: your project's Python dependency information
* **`.circleci`**: Your projects configuration and metadata
* **`app.test.js`** This is where your apps tests will reside

Run `node app.js` to see the contents of your project's empty program:

Feel free to explore the other files, although we won't be editing any of them by hand.

## Step 4 &mdash; Adding the project to CircleCI

After you've pushed the new project to Github, you're going to want to create a project for it in CircleCI by opening the app and connecting your account. Log in with your GitHub login:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-login.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

If you belong to multiple GitHub organizations, select the one that applies to your project:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-choose-org.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

Then you’ll see the list of GitHub projects for your organization:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-org-projects.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

Click **Set Up Project** next to your new project:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-setup-project-coveralls-demo-ruby.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

Then you’ll see the **New Project Set Up** page:

![](https://production-cci-com.imgix.net/blog/media/2020-10-19-circleci-project-ready-prompt.png?ixlib=rb-3.2.1&auto=format&fit=max&q=60&ch=DPR%2CWidth%2CViewport-Width%2CSave-Data&w=1347)

Everything from here is pretty self-service, so we're going to move on to the next section.