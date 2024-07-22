---
layout: post
title: "Deploy using Docker Compose on Windows"
author: Journey
date: 2024-07-16 00:00:00 +0800
categories: docker compose easy
---

This guide is designed to be easy to follow, with an estimated setup time of just 30 minutes.

## Prerequisites
Before we begin, make sure you have the following:
* Windows Operating System
* Docker
* Dynamic DNS service (e.g. Ngrok)


## Install Docker Desktop
1. Go to Docker to download the Windows App.
2. Install Docker.
3. Open the "Docker" app.
4. Accept the terms. Select "Use recommended settings" > "Finish".
5. Click on "Continue without signing in".


## Sign Up for Dynamic DNS Service
In this example, we'll use Ngrok.
1. Sign up for a free account on [ngrok](https://ngrok.com).
2. Go to the ngrok Dashboard and get your Authtoken. You’ll need it in Step 6.
3. Select Windows and download the [ngrok](https://ngrok.com/download) zip file.
![Image]({{ site.baseurl }}/images/posts/2024-07-16/ngrok.png)

4. Unzip the downloaded file and move it to a directory in your PATH.
5. Open the command prompt and change the directory to your PATH.
6. Run the following command: `ngrok config add-authtoken <token>` (Replace `<token>` with the Authtoken from step 2)
7. Go to the ngrok Dashboard and create a domain. The domain should look like this: `<random>.ngrok-free.app`.
8. Run the command: `ngrok http --domain=<random>.ngrok-free.app 8080`.


## Steps to Self-Host on Windows
### Deploy Docker Images
1. Visit the GitHub repository [here](https://github.com/Journey-Cloud/self-hosted-boilerplate/blob/main/docker-compose/docker-compose.yml) and locate the `docker-compose.yml` file. Download the file and open it in a plain text editor. In the YAML file, find and modify the following fields:

Replace the `DOMAIN` parameter with your domain name.
```yaml
services:
  journey-sync-self-hosted-media:
    ...
    environment:
      DOMAIN: <random>.ngrok-free.app
    ...
  journey-sync-self-hosted-service:
    ...
    environment:
      DOMAIN: <random>.ngrok-free.app
    ...
```

Change the `SIGNED` parameter to a unique value.
```yaml
services:
  journey-sync-self-hosted-service:
    environment:
      ...
      SIGNED: randomalphanumeric1
      ...
```

Modify `ADMIN_JS_COOKIE_PASSWORD` with a unique password.
```yaml
services:
  journey-sync-self-hosted-service:
    environment:
      ...
      ADMIN_JS_COOKIE_PASSWORD: randomalphanumeric2
      ...
```

2. Save `docker-compose.yml` in a designated directory. In this example, the PATH is `C:\Users\<username>\Documents\journey`.
3. Open Command prompt and change the directory to the PATH.
    ```sh
    cd .\Documents\journey
    ```
4. Ensure that `docker-compose.yml` is within the folder.
    ```console
    dir
      docker-compose.yml
    ```
5. Pull the images.
    ```sh
    docker compose pull
    ```
6. Run the services.
    ```sh
    docker compose up -d
    ```
  ![Image]({{ site.baseurl }}/images/posts/2024-07-16/docker-compose-up-d.png)
7. Go to `localhost:8080` in your browser. You should see that the Journey self-hosted server is installed.
  ![Image]({{ site.baseurl }}/images/posts/2024-07-16/journey-cloud-sync-windows-it-works-self-host.png)
8. Go to `<random>.ngrok-free.app` in your browser. You should see that the Journey self-hosted server is installed.
    ![Image]({{ site.baseurl }}/images/posts/2024-07-16/journey-cloud-sync-windows-it-works-self-host-ngrok.png)
9. Log in to the admin panel by going to `https://<random>.ngrok-free.app/admin`.
      ![Image]({{ site.baseurl }}/images/posts/2024-07-16/journey-cloud-sync-self-host-admin.png)
10. Now, you need to locate your credentials. In the Docker app, click "Go to the dashboard" to open the Docker dashboard.
        ![Image]({{ site.baseurl }}/images/posts/2024-07-16/docker-journey.png)
11. In the "Containers" tab, expand the "journey" project. Click on the service with a name that starts with `journey-sync-self-hosted-service-` running on port `8080`.
        ![Image]({{ site.baseurl }}/images/posts/2024-07-16/docker-journey2.png)
12. Scroll the logs to retrieve the username and password. Copy the username and password into the admin login page. For OTP, use an authenticator app such as Authy. Enter the secret code or scan the QR code.
        ![Image]({{ site.baseurl }}/images/posts/2024-07-16/docker-journey3.png)
        ![Image]({{ site.baseurl }}/images/posts/2024-07-16/journey-cloud-sync-self-host-admin-sign-in.png)
13. Done! You are in the admin dashboard.
14. Change the admin password. You may also add a new user.
15. Follow the instructions [here](https://help.journey.cloud/en/article/how-to-add-a-self-hosted-journey-cloud-sync-1ty6l1i/) to add a new sync drive.


## Update Journey Docker Images
1. Journey self-hosted is [updated](https://hub.docker.com/r/journeycloud/journey-sync-self-hosted) from time to time. To update, go to the folder where `docker-compose.yml` is located.
    ```sh
    cd .\Documents\journey
    ```
2. Pull the images.
    ```sh
    docker compose pull
    ```
3. Load the new images.
    ```sh
    docker compose up -d
    ```
