---
layout: post
title: Linux Bash Tricks
tags: [linux]
---

The Linux bash is quite powerful. It has so many useful commands (like sort, jq, awk, sed, grep, xargs, ...) and a very powerful pipe mechanism which allows the output from one command to become the input of another command. With this mechanism you get for many use cases parallelism for free. For example, two processes connected via a pipe are running at the same time. If the first process (which produces the input for the second process) outputs something, the second process can take this output and process it while the first process is already producing the next output. 

In this post I summarize some useful bash tricks that I've learned in the past and I'm going to update this post with new tricks that I will learn in the future. So let's start.

Get your current external IP:

    curl ifconfig.co

Compute the sum and the average of numbers:

    echo -e "1\n5\n7\n9" | \
    awk '{ s += $1; c += 1; } END  {print c " " s " " s/c}'

Inspect TLS parameters and the server certificate of a connection:

    openssl s_client -connect google.com:443

Make a connection from a remote host via forwarding of a local port:

    ssh remote.host [-p <sshd port of remote host>] -D <local port>

