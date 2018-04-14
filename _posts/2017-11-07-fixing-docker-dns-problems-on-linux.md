---
layout: post
title: Fixing docker DNS problems on Linux
tags: [linux]
---
<div style="font-size:small; color: gray; font-style: italic">
  By <a href="https://twitter.com/dnl0x00">@dnl0x00</a>
</div>

Today, I run into docker DNS problems. Whenever I tried to build an image with `docker run` I got an error message which said:

```
docker build --no-cache -t ml/3 -f /tmp/tmp.FxOR4qC1OY/Dockerfile /tmp/tmp.FxOR4qC1OY
Sending build context to Docker daemon  6.144kB
Step 1/55 : FROM ubuntu:17.10
Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 127.0.0.1:53: read udp 127.0.0.1:57615->127.0.0.1:53: i/o timeout
```

It seems the docker daemon tried to resolve the hostname docker.io via a DNS server running on my local machine. Indeed, when I checked it via `sudo netstat -tnlp` it turned out, that there was a DNS server (here dnsmasq) running on that port 53. Maybe that DNS server wasn't configured properly.

Well, as I don't need a local DNS server I decided to deinstall it from my system. I did a `sudo apt remove dnsmasq` and doing a netstat again I saw that no DNS server was listening on port 53 anymore.

I tried to build the image again. But unfortunately, I ran into the next issue. Now, I did not get a timeout. Now, I got a "connection refused" error.

```
Error getting v2 registry: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 127.0.0.53:53: read udp 127.0.0.1:42653->127.0.0.53:53: read: connection refused
```

This did make sense as the DNS server was not running anymore. I tried to start the docker daemon with `--dns 8.8.8.8` to set the DNS server explicitly before I started to build the image but it didn't work.

When I checked my `/etc/resolve.conf` I found the problem. The only entry in that file was:

    nameserver 127.0.0.53

I have no idea why it was set to that value but when I changed it to

    nameserver 8.8.8.8

everything worked and I was able to build my image.
