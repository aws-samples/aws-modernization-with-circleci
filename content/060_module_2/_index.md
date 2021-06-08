---
title: "DevSecOps"
chapter: true
draft: false
weight: 6
---

# Introduction

[DevSecOPs][1] is the philosophy of developing applications and infrastructure securely from ideation to deployment. It requires consideration of security risks at all stages of the development lifecycle. While DevOps teams have historically focused on automating the building, testing, and deployment of their applications, DevSecOps includes automating security practices to allow teams to increase security without losing velocity.

To build, deploy, and test your application, your CI/CD pipelines access every resource in your technology stack. These resources include analytics keys, code signing credentials, secure secrets, proprietary code, and data. It is imperative that you secure your CI/CD pipeline so that you never expose protected information to unwanted parties. Without a deliberate effort to protect and secure your pipeline, any one of these resources is a potential security vulnerability.

Recently, we defined three important categories of security practices for CI/CD. They include securing your pipeline configuration, securing code and Git history analysis, and enforcing security policy. CircleCI orbs allow you to easily add integrations to tools and services to address these categories in your pipeline with just a few lines of code. Orbs are reusable, shareable, open source packages of CircleCI config that enable the immediate integration of these services. With orbs, you get an out-of-the-box solution for securing your pipeline.

In this section, you will learn how to easily add an application vulnerability scan job to your CI/CD pipeline using [Synk's Orb][2] .

<!-- URL Links index -->
[1]: https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/
[2]: https://circleci.com/developer/orbs/orb/snyk/snyk





