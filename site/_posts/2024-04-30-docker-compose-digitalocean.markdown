---
layout: post
title: "Deploy using Docker Compose on DigitalOcean"
date: 2024-04-30 12:44:40 +0800
tags: docker compose easy
---

Welcome to our tutorial on hosting your Journey Sync Drive using Docker Compose on DigitalOcean. This guide is designed to be easy to follow, with an estimated setup time of just 30 minutes.

Before we dive in, make sure you have the following:
* DigitalOcean account
* Domain (Recommended)

# Create a Droplet
1. Log in to your DigitalOcean dashboard and navigate to "Create" > "Droplets".
![Image]({{ site.baseurl }}/assets/images/2024-04-30/docker-compose-digitalocean1.png)
2. Choose your preferred region to host your data.
> Tip: Select a region that's closer to you for optimal performance.
3. Under "Select Image", choose "Ubuntu 24.04 (LTS) x64" as your operating system.
4. Select your preferred Droplet type. For this tutorial, we'll choose "Basic" > "Regular (Disk type: SSD)" with specs "4 GB / 2 CPUs, 80 GB SSD Disk, 4 TB transfer".
5. We strongly recommend enabling the "Enable automated backup plan".
6. For authentication, choose "SSH key" instead of "Password". Follow the instructions to set up a new SSH key. 
> Note: Weak passwords can compromise security, so it's best to use SSH keys for authentication.
7. Give your Droplet a hostname, such as "john-appleseed-journal".
8. Click on "Create Droplet" to proceed.
9. Once your Droplet is created, you'll see it listed in your dashboard. It should look like this:
![Image]({{ site.baseurl }}/assets/images/2024-04-30/docker-compose-digitalocean2.png)

# Install Docker
1. Click on the "Launch Console" button located at the top right corner of the dashboard.
![Image]({{ site.baseurl }}/assets/images/2024-04-30/docker-compose-digitalocean3.png)
2. Once clicked, a console window will open. If you are prompted to install the console agent, please wait for a moment and then refresh the page. After refreshing, try launching the console again.
![Image]({{ site.baseurl }}/assets/images/2024-04-30/docker-compose-digitalocean4.png)
3. First, update your existing list of packages.
```sh
sudo apt update
```
4. Next, install a few prerequisite packages which let apt use packages over HTTPS. When prompted, type `Y` and press `ENTER` to continue with the installation.
```sh
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```
5. Then add the GPG key for the official Docker repository to your system.
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
6. Add the Docker repository to APT sources. Press `ENTER` to continue.
```sh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
7. This will also update our package database with the Docker packages from the newly added repo. Make sure you are about to install from the Docker repo instead of the default Ubuntu repo.
```sh
apt-cache policy docker-ce
```
8. You’ll see output like this, although the version number for Docker may be different. Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 20.04 (focal).
```
docker-ce:
  Installed: (none)
  Candidate: 5:26.1.0-1~ubuntu.20.04~focal
  Version table:
     5:26.1.0-1~ubuntu.20.04~focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
```
9. Finally, install Docker. When prompted, type `Y` and press `ENTER` to continue with the installation.
```sh
sudo apt install docker-ce
```
10. Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running.
```sh
sudo systemctl status docker
```
11. The output should be similar to the following, showing that the service is active and running.
```
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
     Active: active (running) since Tue 2024-04-30 08:07:26 UTC; 2min 11s ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 3690 (dockerd)
      Tasks: 9
     Memory: 27.8M (peak: 30.0M)
        CPU: 386ms
     CGroup: /system.slice/docker.service
             └─3690 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```
12. Navigate to your user directory and create a folder named "compose". Enter this folder.
```sh
cd ~/
mkdir compose
cd compose
```
13. Visit the GitHub repository [here](https://github.com/Journey-Cloud/self-hosted-boilerplate/blob/main/docker-compose/docker-compose.yml) and locate the docker-compose.yml file. Copy its contents and paste them into a text editor like PC Notepad. Within the copied content, locate and modify the following fields: 
  * DOMAIN: Replace both `journey-sync-self-hosted-service` & `journey-sync-self-hosted-media` services of this field with your desired domain name. Example: `johnappleseed.com`
  * SIGNED: Change this field to a unique value.
  * ADMIN_JS_COOKIE_PASSWORD: Modify this field with a unique password.
14. In the console, create docker-compose.yml with a text editor.
```sh
nano docker-compose.yml
```
15. Paste the content from your PC Notepad. Then enter Control+X `^X`. Enter `Y` and press `ENTER` to save.
16. Pull the images.
```sh
docker compose pull
```
17. Run the services.
```sh
docker compose up -d
```
18. You should see this.
```
[+] Running 10/10
 ✔ Network root_default                               Created                                               0.1s 
 ✔ Volume "root_journey-redis-volume"                 Created                                               0.0s 
 ✔ Volume "root_journey-typesense-volume"             Created                                               0.0s 
 ✔ Volume "root_journey-fs-volume"                    Created                                               0.0s 
 ✔ Volume "root_journey-mongodb-volume"               Created                                               0.0s 
 ✔ Container root-journey-redis-service-1             Started                                               0.9s 
 ✔ Container root-journey-typesense-service-1         Start...                                              0.9s 
 ✔ Container root-journey-mongodb-service-1           Started                                               1.0s 
 ✔ Container root-journey-sync-self-hosted-service-1  Started                                               0.0s 
 ✔ Container root-journey-sync-self-hosted-media-1    Started 
 ```
19. Once the services are loaded, check if all are loaded correctly.
```sh
docker compose ps -a
```
20. The output should show that all of the 5 services are alive.
```
NAME                                      IMAGE                                   COMMAND                  SERVICE                            CREATED          STATUS          PORTS
root-journey-mongodb-service-1            mongo:7.0                               "docker-entrypoint.s…"   journey-mongodb-service            46 seconds ago   Up 44 seconds   27017/tcp
root-journey-redis-service-1              redis:7.2.4-alpine                      "docker-entrypoint.s…"   journey-redis-service              46 seconds ago   Up 44 seconds   6379/tcp
root-journey-sync-self-hosted-media-1     journeycloud/journey-sync-self-hosted   "npm run launchMedia"    journey-sync-self-hosted-media     45 seconds ago   Up 43 seconds   8080/tcp
root-journey-sync-self-hosted-service-1   journeycloud/journey-sync-self-hosted   "npm run launch"         journey-sync-self-hosted-service   45 seconds ago   Up 43 seconds   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp
root-journey-typesense-service-1          typesense/typesense:26.0                "/opt/typesense-serv…"   journey-typesense-service          46 seconds ago   Up 44 seconds   8108/tcp
```

# Install Nginx


<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->

## Recommended Resources:
* https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04
* https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04
* https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04
