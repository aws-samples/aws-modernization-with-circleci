---
title: "1.3 Terraform Cloud: Create Access Token"
chapter: true
weight: 14
---

## Terraform Cloud

Terraform Cloud is an application that helps teams use Terraform together. It manages Terraform runs in a consistent and reliable environment, and includes easy access to shared state and secret data, access controls for approving changes to infrastructure, a private registry for sharing Terraform modules, detailed policy controls for governing the contents of Terraform configurations, and more.

You will be using Terraform Cloud to store the [Terraform state][] of the infrastructures your pipeline will provision and deploy using Terraform in future modules.



## Create Terraform Cloud Access Token

1. Visit your [Snyk account (Account Settings > API Token section)][3]
1. In the KEY field, click **click to show**, then select and copy your API token from the field.
1. Paste the token that appears on the screen in a safe location for use in future modules.

{{% notice warning %}}
Your Snyk access token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
{{% /notice %}}

You can read more about [Snyk Access Token from their docs here][2]

Great, you have created and safely stored your newly created Snyk Access Token, Now, let's create the Terraform Cloud Access Token.

<!-- URL Links index -->
[1]: https://app.snyk.io/login
[2]: https://support.snyk.io/hc/en-us/articles/360004008258-Authenticate-the-CLI-with-your-account#UUID-4f46843c-174d-f448-cadf-893cfd7dd858_section-idm4557419555668831541902780562
[3]: https://app.snyk.io/account