---
layout: post
title: Home Surveillance System Using Docker
description: Using Docker container and Zoneminder to setup a home surveillance system
---

Create the image:

```
docker build -t='kylejohnson/release-1.30' github.com/ZoneMinder/ZoneMinder
```

Create a container named zoneminder:

```
docker create -t -p 80:80 --name zoneminder kylejohnson/release-1.30 --shm-size 256M
```

Create a systemd service for starting zoneminder on boot:

```
[Unit]
Description=Zoneminder Container
Requires=docker.service
After=docker.service

[Service]
Restart=always
ExecStart=/usr/bin/docker start -a zoneminder
ExecStop=/usr/bin/docker stop -t 2 zoneminder

[Install]
WantedBy=default.target
```

Open a shell inside the zoneminder container:

```
docker exec -i -t zoneminder /bin/bash
```
