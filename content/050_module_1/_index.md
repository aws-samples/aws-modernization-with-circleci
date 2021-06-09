---
title: "Continuous Integration"
chapter: true
draft: false
weight: 5
---

# Introduction

Continuous Delivery is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time without manual or human intervention. Continuous Delivery aims at building, testing, and releasing software with greater speed and frequency. The approach helps reduce the cost, time, and risk of delivering changes by allowing for more incremental updates to applications deployed to production environments.

Continuous Integration (CI), the **"CI"** in CI/CD, is a software development practice, that developers adopt and enables them to better collaborate and communicate around code, while adding consistency, reliability, quality and velocity to their efforts. CI focuses on efficiently producing quality code in smaller more manageable scopes resulting in increased quality and faster code releases.

The core principals in CI are:

- Commit all code to a shared repository
- Write + commit code often
- Test code on every commit (surface errors and bugs)
- Fast Feedback Loops (Be alerted to status of code changes "Pass" or "Fail")
- Quickly fix and recover from "Failed" builds 

Implementing CI practices in software development processes addresses how teams can efficiently write and collaborate around code, but these practices aren't actually realized until automation is integrated to facilitate the automatic execution of critical tasks on code changes. This automation executes these tasks which are defined as a continuous delivery pipelines, also known as CI/CD pipeline. CircleCI provides a very powerful, robust, developer friendly form of automation that meets developers where they are enabling them to focus on what really matters... their code. 

In this section, you will learn how to leverage CircleCI in automating CI related tasks such as:

- Creating continuous delivery pipelines
- Test code changes
- Trigger pipelines

Now that you have a clearer understanding of Continuous Integration, jump into the section and learn how to define pipelines and the core elements of the **config.yml** file. 