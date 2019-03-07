---
title: How to setup a generic shared hosting environment with Passenger
---

[Passenger](https://www.phusionpassenger.com/) is a nice little web server that has the capability to start, stop and scale web server applications with ease.

Here we will use it in conjunction with Nginx to setup a shared hosting environment with multiple users, each running their own web server with their environment of choice (we'll use Node.js, but you can use whatever you want as long as you install the dependencies). We'lle also start from a fresh Ubuntu server 18.04.

## Installation of dependencies

First things first, let's install Passenger:

```bash
# Install our PGP key and add HTTPS support for APT
sudo apt-get install -y dirmngr gnupg
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo apt-get install -y apt-transport-https ca-certificates

# Add our APT repository
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger bionic main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update

# Install Passenger
sudo apt-get install -y passenger
```

A little command to validate it's working correctly:

```bash
passenger-config validate-install
```

Now we can install nginx:

```bash
sudo apt-get install -y nginx
```

And the relevant nginx plugin for passenger:

```bash
sudo apt-get install -y libnginx-mod-http-passenger
```

We also must activate the passenger plugin:

```bash
if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi
```

Time to restart nginx:

```bash
sudo service nginx restart
```
