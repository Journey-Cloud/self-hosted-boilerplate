---
layout: post
title: "Deploy using Docker Compose on MacOS 14"
author: Journey
date: 2024-05-02 00:00:00 +0800
categories: docker compose easy
---

Welcome to our tutorial on hosting your Journey Sync Drive using Docker Compose on MacOS Sonoma 14. This guide is designed to be easy to follow, with an estimated setup time of just 30 minutes.

Before we dive in, make sure you have the following:
* Macintosh E.g. Mac Studio
* Ngrok account

## Install Docker Desktop
1. Go to [Docker](https://www.docker.com/products/docker-desktop/) to download the MacOS App. Choose between Intel chip and Apple chip depending on your Mac.

2. Open the downloaded dmg file and drag it into the "Applications" folder.

3. Open "Docker" app.

4. Accept the terms. Select "Use recommended settings" > "Finish".

5. Click on "Continue without signing in".

6. Click on the red traffic light button to close the window. You should still see that the Docker app is running in the background.

## Sign up for Dynamic DNS service
1. In this example, we use Ngrok. Sign up a free account in [ngrok](https://ngrok.com).

2. Head over to [ngrok](https://dashboard.ngrok.com/get-started/setup/macos) MacOS setup page.

## Install Homebrew
1. Open "Terminal" app from "LaunchPad".
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac1.png)

2. Install Homebrew. Input your password when prompted and press `ENTER`. Press `ENTER` again to install command line tools.
```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

3. Update the repository index of the Homebrew package installer.
```sh
brew update
```

4. Install ngrok.
```sh
brew install ngrok/ngrok/ngrok
```

5. From "ngrok" MacOS setup page, copy the authtoken shown. Enter the command below in your terminal:
```sh
ngrok config add-authtoken <token>
```

6. Under "Deploy your app online", click "Static Domain". Take note that `<random>.ngrok-free.app` is your domain.
```sh
ngrok http --domain=<random>.ngrok-free.app 8080
```

## Deploy Docker Images
1. Visit the GitHub repository [here](https://github.com/Journey-Cloud/self-hosted-boilerplate/blob/main/docker-compose/docker-compose.yml) and locate the `docker-compose.yml` file. Copy its contents and paste them into a text editor like TextEdit. Within the copied content, locate and modify the following fields: 
  * DOMAIN: Replace both `journey-sync-self-hosted-service` & `journey-sync-self-hosted-media` services of this field with your domain name. Example: `<random>.ngrok-free.app`
  * SIGNED: Change this field to a unique value.
  * ADMIN_JS_COOKIE_PASSWORD: Modify this field with a unique password.
  > Ensure that "Make Plain Text" is selected under "Format" menu in TextEdit app.
  
  ![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac2.png)

2. Save text file as `docker-compose.yml` in a designated directory. In this example, we save in `~/Documents/journey`.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac3.png)

3. Go to the designated directory and list the files in the fol der.
```sh
cd ~/Documents/journey
ls
```

4. Ensure that `docker-compose.yml` is within the folder.
```console
johnappleseed@MacBook journey % ls
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

7. Go to `locahost:8080` in your browser. You should see that the Journey self-hosted server is installed.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac4.png)

8. Go to `<random>.ngrok-free.app` in your browser. You should see that the Journey self-hosted server is installed.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac5.png)

9. Log in to the admin panel by going to `https://<random>.ngrok-free.app/admin`.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac6.png)

10. Now, you need to locate your credentials. On the menu, click "Go to the dashboard" to open Docker dashboard.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac7.png)

11. In "Containers" tab, expand "journey" project. Click on the service with name that starts with `journey-sync-self-hosted-service-` running on port `8080`.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac8.png)

12. Scroll the logs to retrive the user name and password. Copy the user name and password into the admin login page. For OTP, use authenticator app such as Authy. Enter the secret code or scan the QR code.
![Image]({{ site.baseurl }}/images/posts/2024-05-02/docker-compose-mac9.png)

13. Done! You are in the admin dashboard.

14. Change the admin password. You may also add new user.

15. Follow the instructions [here](https://help.journey.cloud/en/article/how-to-add-a-self-hosted-journey-cloud-sync-1ty6l1i/) to add a new sync drive.

## Update Journey Docker images
1. Journey self-hosted is [updated](https://hub.docker.com/r/journeycloud/journey-sync-self-hosted) from time to time. To update, go to the folder where `docker-compose.yml` is located.
```sh
cd ~/Documents/journey
```

2. Pull the images.
```sh
docker compose pull
```

3. Load the new images.
```sh
docker compose up -d
```