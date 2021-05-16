---
title: "1.1. Creating a new project"
chapter: true
weight: ADD WEIGHT
---

Infrastructure at CircleCI is organized into projects. Each project is a single program that, when run, declares the desired infrastructure for us to manage.

## Step 1 &mdash; Create a Directory

Each CircleCI project lives in its own directory. Create one now and change into it:

```
create-react-app ci-demo-app
cd ci-demo-app
```

## Step 2 &mdash; Inspect Your New Project

Our project is comprised of multiple files:

* **`app.js`**: your program's main entrypoint file
* **`package.json`**: your project's Python dependency information
* **`.circleci`**: Your projects configuration and metadata. 

Run `node app.js` to see the contents of your project's empty program:

Feel free to explore the other files, although we won't be editing any of them by hand.