---
layout: post
title: Home Surveillance System Using Docker
description: Using Docker container and Zoneminder to setup a home surveillance system
---

## Intro

My original home surveillance system was two D-Link DCS-932LB cameras.
The cameras had motion detection enabled and would upload photos to an FTP server.
This served its purpose well until one night that I actually needed the footage.
Unfortunately, the frame rate of this approach was so low that we couldn't see what we needed.
[ZoneMinder](https://zoneminder.com/) would have given me what I needed, so I installed it and have been using it ever since.

Fast forward to now, and I want to move the ZoneMinder installation to another machine.
Instead of going through the painstaking installation process again, I opted to used Docker.
The ZoneMinder devs [provide Docker images](https://github.com/ZoneMinder/ZoneMinder/wiki/Docker), which made this especially easy.

## Prerequisites

I started with a clean installation of Ubuntu 16.04 Xenial and installed Docker following the [official instuctions](https://store.docker.com/editions/community/docker-ce-server-ubuntu?tab=description).

## Installation instructions

#### Build the image, specifying the version in the tag list.

{% highlight bash %}
docker build -t='kylejohnson/release-1.30' github.com/ZoneMinder/ZoneMinder
{% endhighlight %}

#### Create a container named zoneminder:

{% highlight bash %}
docker create -t -p 80:80  --shm-size 512M --name zoneminder kylejohnson/release-1.30
{% endhighlight %}

This took some experimentation to get right.  ZoneMinder uses `/dev/shm` for its shared memory, and docker only uses `64M` by default.  I was able to create a single monitor with the default setting, but anything beyond that didn't work.  `/dev/shm` was at 100%.  With `512M`, I'm sitting at 35% utilization.  This gives me some room to grow.   

#### Create a systemd service for starting zoneminder on boot.

{% highlight kconfig %}
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
{% endhighlight %}


This should be saved in `/etc/systemd/system/zoneminder.service`, started using `systemctl start zoneminder` and enabled using `systemctl enable zoneminder`.

#### Setup your monitors.

## Troubleshooting

There were a couple times I needed to access the ZoneMinder logs.  To do this, I used docker to start a shell in the container.

{% highlight bash %}
docker exec -i -t zoneminder /bin/bash
{% endhighlight %}

## Conclusions

This is quick and easy to setup.  From start to finish, it took me less than an hour to have a functioning installation.  
