---
title: "1.1 Docker Hub: Create Docker Hub Access Token"
chapter: true
weight: 10
---

## Docker Hub

[Docker Hub][1] is a service provided by Docker for finding and sharing container images with your team. It is the world’s largest repository of container images with an array of content sources including container community developers, open source projects and independent software vendors (ISV) building and distributing their code in containers.

## Create a Docker Hub Access Token

The pipeline will package the application into a Docker image then push that image a public Docker Hub image repository so that it will be available to the deployment segment of the pipeline. In order to push or upload the newly build Docker image, the pipeline will need an Access Token to authorize transaction on your Docker Hub account. You will need to [create a new access token][2] and store it for use in latter modules. So create your new access tokens:


1. Log in to hub.docker.com.
1. Click on your username in the top right corner and select Account Settings.
1. Select Security > New Access Token.
1. Add a description for your token. Use something that indicates where the token is going to be used, or set a purpose for the token.
1. Copy the token that appears on the screen and record it in a safe location for use in future modules. Make sure you do this now! Once you close this prompt, Docker will never show the token again and you will have to create a new access token

{{% notice warning %}}
Docker Hub credentials and access token must be protected and not shared with unauthorized parties to prevent exposure and unauthorized access.
{{% /notice %}}

Now that you have created and safely recorded your new access token, lets move to the next section and create a new Snyk Access token.


<!-- URL Links index -->
[1]: https://hub.docker.com/
[2]: https://docs.docker.com/docker-hub/access-tokens/
