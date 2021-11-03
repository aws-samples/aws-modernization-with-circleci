---
title: "2.5 Getting started with LaunchDarkly"
chapter: true
weight: 10
---

## Creating a flag

You can create and modify feature flags from the dashboard.

<img src="https://docs.launchdarkly.com/static/10cf6c4c06a05bfa64dcd6dc205e7146/6af66/launchdarkly-dashboard.png" alt="LaunchDarkly Dashboard">

To create a feature flag:

1. Log into LaunchDarkly. The dashboard appears.
2. Click + Flag. The "Create a feature flag" screen appears:

<img src="https://docs.launchdarkly.com/static/8ffa9ff69ba073c0bf8d01cb3458b8a2/6af66/flag-new-callout.png" alt="Create a flag">

3. Enter a unique, human-readable Name.
4. Enter a unique flag Key. You'll use this key to reference the flag in your code. A suggested key auto-populates from the name you enter, but you can customize it if you wish.
5. Choose an option in the Flag variations dropdown. For today's session we'll be working with Boolean flags. To learn more about all of the different vairations, you can read up on [flag variations](https://docs.launchdarkly.com/home/flags/variations).
6. Click Save Flag.

<img src="https://docs.launchdarkly.com/static/c26632369691636aca47a7d708ec5091/6af66/flag-create.png" alt="Created flag">

## Add the dependency 

The first step is to install the LaunchDarkly SDK as a dependency in your application using your application's dependency manager.

First, open the terming and run this command: 

`npm install launchdarkly-node-server-sdk --save`

Next, import the LaunchDarkly client in your application code:

`const LaunchDarkly = require('launchdarkly-node-server-sdk');`

Once the SDK is installed and imported, you'll want to create a single, shared instance of the LaunchDarkly client. You should specify your SDK key here so that your application will be authorized to connect to LaunchDarkly and for your application and environment.

Here's how:

`client = LaunchDarkly.init("YOUR_SDK_KEY");`

The client will emit a ready event when it has been initialized and can serve feature flags.

Using client, you can check which variation a particular user should receive for a given feature flag.

Head [here](https://github.com/CircleCI-Public/aws-circleci-modernization-workshop-code/blob/main/app.js) to view an example of this using our current forked repo. 