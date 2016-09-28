# Intro
A fork of the official HAProxy Docker image with a newer OpenSSL so we can use ALPN for HTTP/2. Only version HAProxy 1.6 is built. 

# Usage
The usage is identical to the original, see https://hub.docker.com/_/haproxy/
With the obvious exception that the image name should be `arienkock/haproxy-alpn`.

## Reloading
As the official image README suggests, using signals you can reload the configuration for near-zero-downtime updates. However, __!!!IMPORTANT!!!__: if you mount the haproxy .cfg file as a file mount instead of a directory mount in Docker, changes will not be reflected and reloading will simply pick up the old config. That's because the container gets a COPY of the file if you mount a file rather than a directory containing the cfg file. Here is an example _Systemd_ service file you can use (notice the `ExecReload` line).

```
[Unit]
Description=HAProxy
After=docker.service

[Service]
ExecStart=/usr/bin/docker run --rm --name %n -t \
  -p 80:80 -p 443:443 \
  -v /etc/haproxy:/usr/local/etc/haproxy \
  arienkock/haproxy-alpn
ExecReload=/usr/bin/docker kill -s HUP %n
ExecStop=/usr/bin/docker stop -t 10 %n
ExecStopPost=/usr/bin/docker rm -f %n
Restart=always

[Install]
WantedBy=multi-user.target
```

# OpenSSL version

Built with OpenSSL 1.0.2h as can be seen by running:

```sh
$ docker run --rm arienkock/haproxy-alpn haproxy -vv
HA-Proxy version 1.6.9 2016/08/30
Copyright 2000-2016 Willy Tarreau <willy@haproxy.org>

Build options :
  TARGET  = linux2628
  CPU     = generic
  CC      = gcc
  CFLAGS  = -O2 -g -fno-strict-aliasing -Wdeclaration-after-statement
  OPTIONS = USE_ZLIB=1 USE_OPENSSL=1 USE_STATIC_PCRE=1 USE_PCRE_JIT=1

Default settings :
  maxconn = 2000, bufsize = 16384, maxrewrite = 1024, maxpollevents = 200

Encrypted password support via crypt(3): yes
Built with zlib version : 1.2.8
Compression algorithms supported : identity("identity"), deflate("deflate"), raw-deflate("deflate"), gzip("gzip")
Built with OpenSSL version : OpenSSL 1.0.2j  26 Sep 2016
Running on OpenSSL version : OpenSSL 1.0.2j  26 Sep 2016
OpenSSL library supports TLS extensions : yes
OpenSSL library supports SNI : yes
OpenSSL library supports prefer-server-ciphers : yes
Built with PCRE version : 8.35 2014-04-04
PCRE library supports JIT : yes
Built without Lua support
Built with transparent proxy support using: IP_TRANSPARENT IPV6_TRANSPARENT IP_FREEBIND

Available polling systems :
      epoll : pref=300,  test result OK
       poll : pref=200,  test result OK
     select : pref=150,  test result OK
Total: 3 (3 usable), will use epoll.

<7>haproxy-systemd-wrapper: executing /usr/local/sbin/haproxy -p /run/haproxy.pid -vv -Ds
<5>haproxy-systemd-wrapper: exit, haproxy RC=0
```
