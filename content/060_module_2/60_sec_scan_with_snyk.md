---
title: "3.1 DevSecOps and CI/CD with Snyk"
chapter: true
weight: 10
---

## DevSecOps Adoption: Integrating Security into the CI/CD Pipeline
To integrate security objectives early in the development of an application, start before the first line of code is ever written. Security can integrate and begin effective threat modeling during the initial concept of the system, application, or individual user story. Static analysis, linters, and policy engines can be run any time a developer checks in code, ensuring that any low-hanging fruit is dealt with before the changes move further upstream.

Software composition analysis can be applied holistically to confirm that any open-source dependencies have compatible licenses and are free of vulnerabilities. A behavioral by-product of this is that developers feel a sense of ownership over the security of their applications, getting immediate feedback on the relative security of the code they’ve written.

Once the code is checked in and builds, you can start to employ security integration tests. Running the code in an isolated container sandbox allows for automated testing of things like network calls, input validation, and authorization. These tests generate fast feedback, enabling quick iteration and triage of any issues that are identified, causing minimal disruption to the overall stream. If things like unexplained network calls or un-sanitized input occur, the tests fail, and the pipeline generates actionable feedback in the form of reporting and notifications to the relevant teams.

Once the deployment artifact passes the first battery of integration tests, it moves on to the next stage of integration testing. Now it will be deployed to a wider sandbox, a limited copy of the eventual production environment. At this stage, further security integration testing can be performed, albeit with a different objective. 

Now, things like correct logging and access controls can be tested. Does the application log relevant security and performance metrics correctly? Is access limited to the correct subset of individuals (or prevented entirely)? Failure again results in action items to the relevant teams.

Finally, the application makes its way to production. However, the work of DevSecOps continues in earnest. Automated patching and configuration management ensure that the production environment is always running the latest and most secure versions of software dependencies. Ideally, immutable infrastructure means that the entire environment is frequently torn down and rebuilt, constantly subjected to the battery of tests along the breadth of the pipeline.

Utilizing a DevSecOps CI/CD pipeline helps integrate security objectives at each phase, without adding burdensome bureaucracy and gatekeeping, allowing the rapid delivery of business value to be maintained.

## Snyk

[Snyk][1] is an open source security platform designed to help software-driven businesses enhance developer security. Snyk's dependency scanner makes it the only solution that seamlessly and proactively finds, prioritizes, and fixes vulnerabilities and license violations in open source dependencies and container images.

Synk has an awesome [orb][2] that you'll implement into your CI/CD pipeline that can scan for vulnerabilities and provide valuable vulnerability reports along with potential mitigation actions. The Snyk orb is great mechanism for incorporating DevSecOps practices into your CI/CD pipelines,

Now that you learned about DevSecOps and Snyk, jump to the next section and learn how to build a security scanning job into your pipeline.


<!-- URL Links index -->
[1]: https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/
[2]: https://circleci.com/developer/orbs/orb/snyk/snyk