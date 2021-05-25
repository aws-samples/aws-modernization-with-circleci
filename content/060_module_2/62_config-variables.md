---
title: "3.2 Configure Elastic Beanstalk env variables"
chapter: true
weight: 12
---

## Step 1 &mdash; Configure environment variables

Navigate to `Project Settings > Environment Variables` in the CircleCI control panel and add these keys:

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```

Make sure the AWS user has the correct permissions. That's about it in terms of variables we need for our project. Let's move on to the next section and learn how to set up our deployment.