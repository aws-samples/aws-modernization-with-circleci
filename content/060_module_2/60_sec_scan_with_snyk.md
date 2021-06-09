---
title: "3.1 DevSecOps and CI/CD with Snyk"
chapter: true
weight: 10
---

## Integrating Security into the CI/CD Pipeline

DevSecOps is the philosophy of weaving security practices into the software development life cycle (SDLC). This means adding security checks during the application’s development and not as an afterthought. Building security into DevOps practices is not a new concept, but in my experience, security practices have generally been tacked on at the end of the SDLC.

You may be asking, “What difference does it make where and when security operations are performed, as long as they are getting done?” Well, here is a scenario that might clear things up. Imagine that you just developed some awesome, new features for a project that you’re working on. You’ve programmed the new features, written all the corresponding tests, and your work is passing on your CI/CD platform. You’re excited to get your work deployed, but at the end of this process, your release gets flagged by security. They manually performed a scan of your software and the scan reported high-risk vulnerabilities in some of the dependency libraries being used in the project. You now have to remediate all of the security issues and then run through the whole process all over again.

DevOps enables us to deliver quality software rapidly and efficiently. However, the scenario mentioned above is not efficient. Automating and implementing security processes into CI/CD pipelines rectifies this. By integrating security scans into segments of CI/CD pipelines, teams will be alerted to security discrepancies very early on in the development process. Fixing vulnerable libraries is easier to mitigate the earlier they are surfaced. Consider the possibility that the updated libraries have deprecated some features in the vulnerable version which were heavily used in the app. This is a huge problem and it will require some re-engineering to accommodate the changes in the updated libraries, tacking on unexpected time and effort to the development process. Again, integrating security into CI/CD pipelines will alert you to security issues early on and will enable you to fix them sooner in the process rather than later.

Now that we’ve shared what DevSecOps is and some of its benefits, let's demonstrate how to easily add security scans to your CI/CD pipelines using a security scan tool called [Snyk][1].

Snyk enables you to find, and more importantly, fix known vulnerabilities in your applications and containers. Snyk is a CircleCI technology partner and they created the [Snyk orb][2] which enables developers to add security scans to their CI/CD pipelines.

## Snyk

[Snyk][1] is an open source security platform designed to help software-driven businesses enhance developer security. Snyk's dependency scanner makes it the only solution that seamlessly and proactively finds, prioritizes, and fixes vulnerabilities and license violations in open source dependencies and container images.

Synk has an awesome [orb][2] that you'll implement into your CI/CD pipeline that can scan for vulnerabilities and provide valuable vulnerability reports along with potential mitigation actions. The Snyk orb is great mechanism for incorporating DevSecOps practices into your CI/CD pipelines,

Now that you learned about DevSecOps and Snyk, jump to the next section and learn how to build a security scanning job into your pipeline.


<!-- URL Links index -->
[1]: https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/
[2]: https://circleci.com/developer/orbs/orb/snyk/snyk