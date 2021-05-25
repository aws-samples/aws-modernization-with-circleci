---
title: "Automating Elastic Beanstalk"
chapter: true
draft: false
weight: 6
---

# Introduction

In this section, we are going to be leveraging automation to continuously deploy our code to AWS Elastic Beanstalk. Elastic Beanstalk is a easy-to-use orchestration service for deploying and scaling web applications and services developed in a variety of languages. 

We will be leveraging a CLI tool called `awsebcli` and implenting it as a script in our CircleCI configuration. The goal of this module is to simulate a scenario where we are implementing continuous deployment to a hostiing environment. 