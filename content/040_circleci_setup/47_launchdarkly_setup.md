---
title: "1.5 LaunchDarkly: Creating a flag"
chapter: true
weight: 17
---

## Creating a flag

You can create and modify feature flags from the dashboard.

<img src="https://docs.launchdarkly.com/static/10cf6c4c06a05bfa64dcd6dc205e7146/6af66/launchdarkly-dashboard.png" alt="LaunchDarkly Dashboard">

To create a feature flag:

1. Log into LaunchDarkly. The dashboard appears.
2. Click + Flag. The "Create a feature flag" screen appears:

<img src="https://docs.launchdarkly.com/static/8ffa9ff69ba073c0bf8d01cb3458b8a2/6af66/flag-new-callout.png" alt="Create a flag">

3. Enter a unique, human-readable Name. For today's session let's go with: 
`circleci-workshop`
4. Enter a unique flag Key. You'll use this key to reference the flag in your code. A suggested key auto-populates from the name you enter, but you can customize it if you wish.
5. Choose an option in the Flag variations dropdown. For today's session we'll be working with Boolean flags. To learn more about all of the different vairations, you can read up on [flag variations](https://docs.launchdarkly.com/home/flags/variations).
6. Click Save Flag.

<img src="https://docs.launchdarkly.com/static/c26632369691636aca47a7d708ec5091/6af66/flag-create.png" alt="Created flag">

