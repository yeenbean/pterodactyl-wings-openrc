# pterodactyl-wings-openrc

## Setup

This guide assumes a fresh installation of Alpine Linux on the target server. If
your server is running another operating system, adjust accordingly.

While this guide does cover the installation of Wings, it does **not** cover the
installation of Pterodactyl panel. However, you can easily deploy it using
[Docker Compose](https://github.com/pterodactyl/panel/blob/1.0-develop/docker-compose.example.yml).

Additionally, this guide **assumes a development environment** and does **not**
follow best practices. If your target server is a production machine and your
team doesn't want to take a risk, use a
[supported operating system](https://pterodactyl.io/wings/1.0/installing.html#supported-systems)
instead.

### 1. Install Docker

```sh
apk update
apk add docker docker-compose
rc-update add docker
rc-service docker start
```

### 2. Install Wings

```sh
mkdir -p /etc/pterodactyl
curl -L -o /usr/local/bin/wings "https://github.com/pterodactyl/wings/releases/latest/download/wings_linux_$([[ "$(uname -m)" == "x86_64" ]] && echo "amd64" || echo "arm64")"
chmod u+x
```

### 3. Connect your node to your Pterodactyl panel

You can follow
[this guide](https://pterodactyl.io/wings/1.0/installing.html#configure) for
instructions. Autodeploy does work and I recommend using it. Before continuing
below, I recommend running `wings --debug` to confirm it works.

### 4. Add the OpenRC script

```sh
touch /etc/init.d/wings
chmod +x /etc/init.d/wings
nano /etc/init.d/wings
```

Paste the contents of the
[OpenRC script for wings](https://github.com/yeenbean/pterodactyl-wings-openrc/blob/main/wings)
into this file, then save and quit.

### 5. Enable and start the service

```sh
rc-update add wings
rc-service wings start
```

After 5-10 seconds, your node should be connected to your panel.