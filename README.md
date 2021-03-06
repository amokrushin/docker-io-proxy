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
    build: https://github.com/amokrushin/docker-io-proxy.git
    ports:
      - 1080:1080
    volumes:
      - ./data/docker-io-proxy:/etc
    environment:
      - HOST=example.com
      - PORT=1080
    logging:
      driver: none
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


Docker configuration
---------------------------

<https://docs.docker.com/config/daemon/systemd/#httphttps-proxy>

> The Docker daemon uses the HTTP_PROXY, HTTPS_PROXY, and NO_PROXY environmental variables in its start-up environment 
> to configure HTTP or HTTPS proxy behavior. You cannot configure these environment variables using the daemon.json file.

Create a file called `/etc/systemd/system/docker.service.d/https-proxy.conf` that adds the `HTTPS_PROXY` environment variable:

```
[Service]
Environment="HTTPS_PROXY=socks5://[USER]:[PASSWORD]@[HOST]:[PORT]/" "NO_PROXY=localhost,127.0.0.1"
```

Flush changes and restart Docker:

```
sudo systemctl daemon-reload \
&& sudo systemctl restart docker
```

Test:
```
docker pull hello-world
```


Links
-----

* [schors/tgdante2](https://github.com/schors/tgdante2) Dockerized Dante socks5 proxy for telegram. Alpine version
