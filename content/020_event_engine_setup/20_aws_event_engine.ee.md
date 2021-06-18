---
title: "1. AWS Event Engine"
chapter: true
weight: 10
---

## Attending an AWS hosted event

To complete this workshop, you will be provided with an AWS account via the AWS Event Engine service. The AWS staff will send an Event Engine dashboard link with a hash parameter, please follow the below instructions on how to get access into your account.

{{% notice warning %}}
If you are currently logged in to an AWS Account, you can log out using this [link](https://console.aws.amazon.com/console/logout!doLogout)
{{% /notice %}}

### Logging into Event Engine Dashboard

1. Click on the link that the AWS staff has given you, it should look something like this: `https://dashboard.eventengine.run/login?hash=s0m3-r4nd0mh4shval-ue`. You should land on this page:
   ![Event Engine Sign In](/images/ee-sign-in.png)
1. Click the Email One-Time Password option and enter your email address. You'll wait 2-3 minutes for an email with the confirmation code. Please enter the confirmation code.
1. Your page should look like this:
   ![EE Successful OTP Sign In](/images/ee-sign-in-success.png)
1. You should now be on the Event Engine dashboard:
   ![Event Engine Dashboard](/images/setup/event-engine-dashboard.png)
1. Select the AWS Console button. Click the **Open AWS Console** to be sent to your AWS Dashboard.
   ![Access key](/images/ee-access-key.png)
   {{% notice note %}}
   This popup also includes the AWS Access Key and AWS Secret Access Key that you'll need to add to your CircleCI's environment variable later on in the workshop!
   {{% /notice %}}
1. Make sure you're in the us-east-1 region.
Please select **US East (N.Virginia)** in the top right corner.
   ![Event Engine Region](/images/setup/event-engine-region.png)

{{% notice warning %}}
This account will expire at the end of the workshop and  all the resources created will be automatically de-provisioned. You will not be able to access this account after today.
{{% /notice %}}