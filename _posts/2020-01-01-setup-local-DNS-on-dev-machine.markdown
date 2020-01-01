---
layout: post
title:  "How to set up your own DNS server for local development"
date:   2020-01-01 16:00:00 +1100
categories: devops
---
This post talks about setting up a _local_ DNS "server" on your machine.

The reason for doing that is because sometimes we need to make network requests to a host name that is pointing to a _local_ IP address that is not accessible on the Internet. Naturally we can achieve that by just editing the `/etc/hosts` file, but the approach described here may feel less "hacky".

The key is using [dnamasq](https://en.wikipedia.org/wiki/Dnsmasq).
  
My machine is running Fedora, and the following steps might be different for other operating systems.

1. Create a file at `/etc/NetworkManager/conf.d/dnsmasq.conf` and put the following in it:
```
[main]
dns=dnsmasq
```
1. Restart NetworkManager.
```
sudo systemctl restart NetworkManager
```
Now if you look at `/etc/resolv.conf`, you will see the name server has been changed to point to `127.0.0.1` which is where `dnsmasq` is running.
1. Create a file in this directory: `/etc/NetworkManager/dnsmasq.d`, and name it `<your top level domain name>.conf`, such as `example.com.conf`.
1. In that file, you put in your "DNS A records", like so:
```
address=/my-web-server.example.com/127.0.0.1
``` 
1. Then you restart both `dnsmasq` and NetworkManager.
```
sudo systemctl restart dnsmasq NetworkManager
```

You can verify that the change is taking effect by
```
dig +trace <your host name>
```

That's it.

By the way, doing this will _not_ jeopardise the normal DNS lookup. Only your "fake domain and hosts" will be resolved locally.   