---
title: "1.1 Docker Hub: Create Docker Hub Access Token"
chapter: true
draft: true
hidden: true
weight: 10
---

## Docker Hub

[Docker Hub][1] is a service provided by Docker for finding and sharing container images with your team. It is the world’s largest repository of container images with an array of content sources including container community developers, open source projects and independent software vendors (ISV) building and distributing their code in containers.

## Create a Docker Hub Access Token

The pipeline will package the application into a Docker image. It then pushes that image a public Docker Hub image repository so that it will be available to the deployment segment of the pipeline. To push or upload the newly-built Docker image, the pipeline will need an access token to authorize transaction on your Docker Hub account. You will need to [create a new access token][2] and store it for use in later modules. To create your new access tokens:


1. Log in to hub.docker.com
1. Click your username in the top right corner and select **Account Settings**
1. Select **Security > New Access Token**.
1. Add a description for your token that indicates where the token will be used, or that sets a purpose for the token
1. Copy the token that appears on the screen and record it in a safe location for use in future modules. Make sure you do this now! Once you close this prompt, Docker will never show the token again and you will have to create a new one

{{% notice warning %}}
Docker Hub credentials and access tokens must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
{{% /notice %}}

Now that you have created and safely recorded your new access token, let's move to the next section and create a new Snyk Access token.


<!-- URL Links index -->
[1]: https://hub.docker.com/
[2]: https://docs.docker.com/docker-hub/access-tokens/
