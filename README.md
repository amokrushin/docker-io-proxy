Dockerized Dante socks5 proxy for *.docker.io.
==========================================================

Features
--------
* User management scripts
* Only docker.io usage restrictions

Usage with docker-compose
-----------------

```yml
version: '3.6'

services:
  docker-io-proxy:
    build: https://github.com/schors/tgdante2.git
    ports:
      - 1080:1080
    volumes:
      - ./data/docker-io-proxy:/etc
    restart: unless-stopped
```


Manage users
---------------------------

```console
dc run --rm docker-io-proxy /scripts/add user password
dc run --rm docker-io-proxy /scripts/chp user password
dc run --rm docker-io-proxy /scripts/list
dc run --rm docker-io-proxy /scripts/del user
```

> Note: `dc` is alias for `docker-compose`


Links
-----

* [schors/tgdante2](https://github.com/schors/tgdante2) Dockerized Dante socks5 proxy for telegram. Alpine version
