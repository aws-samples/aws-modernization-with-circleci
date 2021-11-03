---
title: "2.1 Intro to LaunchDarkly"
chapter: true
weight: 8
---

## Intro to LaunchDarkly

LaunchDarkly is a continuous delivery platform that provides feature flags as a service and allows developers to iterate quickly and safely. We allow you to easily flag your features and manage them from the LaunchDarkly dashboard. With LaunchDarkly, you can:

- Roll out a new feature to a subset of your users (like a group of users who opt-in to a beta tester group), gathering feedback and bug reports from real-world use cases.
- Gradually roll out a feature to an increasing percentage of users, and track the effect that the feature has on key metrics (for instance, how likely is a user to complete a purchase if they have feature A versus feature B?).
- Turn off a feature that you realize is causing performance problems in production, without needing to re-deploy, or even restart the application with a changed configuration file.
- Grant access to certain features based on user attributes, like payment plan (eg: users on the ‘gold’ plan get access to more features than users in the ‘silver’ plan). Disable parts of your application to facilitate maintenance, without taking everything offline.

LaunchDarkly provides feature flag SDKs for:

- [Java](http://docs.launchdarkly.com/docs/java-sdk-reference)
- [JavaScript](http://docs.launchdarkly.com/docs/js-sdk-reference)
- [PHP](http://docs.launchdarkly.com/docs/php-sdk-reference)
- [Python](http://docs.launchdarkly.com/docs/python-sdk-reference)
- [Go](http://docs.launchdarkly.com/docs/go-sdk-reference)
- [Node.JS](http://docs.launchdarkly.com/docs/node-sdk-reference)
- [.NET](http://docs.launchdarkly.com/docs/dotnet-sdk-reference)
- [Ruby](http://docs.launchdarkly.com/docs/ruby-sdk-reference)
- [Python Twisted](http://docs.launchdarkly.com/docs/python-twisted-sdk-reference)
- [iOS](http://docs.launchdarkly.com/docs/ios-sdk-reference)
- [Android](http://docs.launchdarkly.com/docs/android-sdk-reference)

Many of the specific uses for LaunchDarkly’s features are covered within the [uses](https://github.com/launchdarkly/featureflags/blob/master/2%20-%20Uses.md) section of our guide, although we recommend you start with the [first chapter](https://github.com/launchdarkly/featureflags/blob/master/1%20-%20Introduction.md) for a comprehensive overview. For other questions, technical or otherwise, feel free to visit our site’s [FAQ](https://launchdarkly.com/faq.html) or our [support page](https://support.launchdarkly.com/)!

## Integrations

LaunchDarkly has numerous software integrations that are designed to make your experience smoother when using feature flags in combination with other products. These include:

- [Slack](http://docs.launchdarkly.com/docs/slack)
- [HipChat](http://docs.launchdarkly.com/docs/hipchat)
- [Webhooks](http://docs.launchdarkly.com/docs/webhooks)
- [Optimizely](http://docs.launchdarkly.com/docs/optimizely)
- [New Relic](http://docs.launchdarkly.com/docs/newrelic)
- [Visual Studio Team Services](http://docs.launchdarkly.com/docs/visual-studio-team-services-extension)
- [Bitbucket Pipelines](http://docs.launchdarkly.com/docs/bitbucket-pipelines)

## Progressive delivery
Companies utilizing continuous integration/continuous delivery (CI/CD) or Progressive Delivery rely on feature flag management to gradually roll out features to users. Continuous delivery is the ability to shorten release cycles and get new functionality in the user’s hands quickly and safely. Progressive Delivery kicks it up a notch and provides fine-grained control over the rollout and ownership of a feature. Progressive Delivery includes user segmentation, traffic management, observability, and automation to release features to small portions of users.

Through these delivery processes, teams develop a cadence for lean releases, where new code is wrapped in feature flags. Features are gradually rolled out to the user base to validate their integrity and minimize the risk to your platform and company.

