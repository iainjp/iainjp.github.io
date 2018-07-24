---
title: "From Jekyll to Ghost"
date: 2015-11-25T11:59:58Z
draft: false
tags:
 - "ghost"
 - "github"
 - "docker"
permalink: "posts/from-jekyll-to-ghost/"
---

When I started this blog ([on a bit of a whim](https://iainjp.com/up-and-running/)), it was intended as a place to braindump some topics and opinions and little guides in a _“this was good; this is how i did it”_ way.

The more I thought about it though, the more I realised that a personal blog that’s backed with a passion for learning and sharing has a lot potential to teach you things! I wasn’t getting this from the Github + Jekyll set up. And that’s totally okay - Jekyll on Github is intended for this purpose, getting out of your way to focus on the content. However I actually wanted it in my way a little bit (though just a little…). Just enough to learn from it.

That’s when I started looking around at other solutions that I could have a play with, and when i subsequently found **Ghost**.

## What is Ghost?

Ghost is a beautiful Patrick Swayze movie about sexy pottery making.

![](/assets/images/from-jekyll-to-ghost/Ghost-patrick-swayze-31227069-2560-1722.jpg)

**It’s also a blogging platform**. Unlike Wordpress, that’s become this crazy giant multi-missioned platform that powers so much of internet in lots of different ways, Ghost aims to just be for blogging. And from what I have seen so far, it does a great job of this.

### Why?

It looks clean, it allows for custom theming, it has a lovely admin panel for creating content in Markdown (which made the move even easier) with a live preview view.

It’s built from NodeJS, and seems to have a huge community driving it’s development.


### How did I do it?

The process from moving from Github Pages/Jekyll was really simple.

1. I signed up for a [DigitalOcean Droplet](https://www.digitalocean.com/). I chose the Docker droplet - this will hopefully give me more freedom later if I want to use the droplet for multiple things. It’s also really cheap - something like £3.10 a month.

- I set up keys and things - the usual for a new remote machine. (DigitalOcean has some great documentation - including [how to set up SSH keys](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-keys-with-digitalocean-droplets))

- I installed the Ghost docker image. This was surprisingly simple and effective. More information on how to do this is on the [Ghost Dockerhub page](https://hub.docker.com/_/ghost/)

- I installed nginx on the host machine. My machine image is an Ubuntu box, so this was as simple as `apt-get install nginx`

- I created a simple nginx server block to point to the docker image (routing inbound port 80 to instance port 2368) [1]

- After opening port 80, I logged into Ghost by going to `<ip_address>/ghost` and “registering” for a Ghost account

- I logged into my domain registrar (I'm with [LCN](lcn.com)) and changed the IP address my domain name points at.


And this was me. The rest was just updating my Ghost profile information, and moving the content across from Github. The markdown syntax is a little bit different in places, but the live preview that Ghost provides makes finding these differences really simple.

### What next?

Since I’m self-hosting, I wanted to protect the blog via SSL. Especially since my login credentials to the blog interface would be passed in plain-text otherwise.

I recently did this! I’ll write another post to cover this (spoiler: it was dead easy).




---
[1]: Something like this:

```
server {
    listen 80;
    server_name iainjp.com;

    location / {
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   Host      $http_host;
        proxy_pass http://localhost:2368;
    }
}
```
