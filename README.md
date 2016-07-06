A fork of the official HAProxy Docker image. The usage is identical, see https://hub.docker.com/_/haproxy/

Built with OpenSSL 1.0.2h as can be seen by running:

```shell
$ docker run --rm haproxy-alpn haproxy -vv
HA-Proxy version 1.6.6 2016/06/26
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
Built with OpenSSL version : OpenSSL 1.0.2h  3 May 2016
Running on OpenSSL version : OpenSSL 1.0.2h  3 May 2016
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
