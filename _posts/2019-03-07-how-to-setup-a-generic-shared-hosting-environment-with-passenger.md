---
title: How to setup a generic shared hosting environment with Passenger
---

[Passenger](https://www.phusionpassenger.com/) is a nice little web server that has the capability to start, stop and scale web server applications with ease.

Here we will use it in conjunction with Nginx to setup a shared hosting environment with multiple users, each running their own web server with their environment of choice (we'll use Node.js, but you can use whatever you want as long as you install the dependencies). We will also start from a fresh Ubuntu server 18.04.

Note that you will need one subdomain per user (or a wildcard pointing to your server).

## Installation of Nginx and Passenger

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

Now we can install Nginx:

```bash
sudo apt-get install -y nginx
```

And the relevant Nginx plugin for passenger:

```bash
sudo apt-get install -y libnginx-mod-http-passenger
```

We also must activate the passenger plugin:

```bash
if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi
```

Time to restart Nginx:

```bash
sudo service nginx restart
```

That should be OK for the base configuration.

## Setup of a user

Here we'll install Node.js for our example:

```bash
sudo apt install nodejs npm
# Update npm
sudo npm install npm -g
```

Now we can create a user:

```bash
sudo adduser test
```

Let's make a little setup for that user by switching to him:

```bash
sudo su - test
```

OK, we're "him". Let's make a basic web application with Node.js:

```bash
mkdir app
cd app
npm init # enter what you want
npm install --save connect commander
cat > app.js << EOL
var connect = require('connect');
var http = require('http');
var program = require('commander');

program.option("-p, --port <port>", "The port", parseFloat)
    .parse(process.argv);

const port = program.port || 3000;

var app = connect();

app.use(function(req, res){
  res.end('Hello world!\n');
})

http.createServer(app).listen(port);
console.log("Example app listening on http://127.0.0.1:" + port);
EOL
```

We can test our simple website:

```bash
node app.js --port 4000 # Ctrl-C to stop
```

Now it's time we create some wrapper script for Passenger:

```bash
cat > app << EOL
#! /bin/bash
node app.js --port \$PORT
EOL
chmod a+x app
mkdir public # This is important for Passenger
```

Time to go back to our normal user with sudo capabilities:

```bash
exit
```

Let's go create some nginx configuration for our `test` user.

```bash
cd /etc/nginx/sites-enabled/
sudo nano test
```

Use this example configuration by adding the proper subdomain:

```
server {
    listen 80;
    server_name MY_SUBDOMAIN;

    # Tell Nginx and Passenger where your app's 'public' directory is
    root /home/test/app/public;

    # Turn on Passenger
    passenger_enabled on;
    passenger_app_start_command "./app $PORT";
}
```

Time to ask Nginx to reload its configuration:

```bash
sudo systemctl reload nginx
```

Now your application should be available on the configured subdomain.

## Going further

As you've seen it's just a matter of installing dependencies then writing a small wrapper script (and also providing a public folder for Passenger to serve public files). You could have multiple applications in completely different languages like Go, Python, Ruby, PHP, etc...

The downsides to using Passenger is that it's not really a tool monitoring the CPU, RAM or disk usage of the processes it launches. So it would necessitates additionnal setup to be absolutely sure none of your users are doing anything wrong. But that's another story...
