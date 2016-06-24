# caddy-path-proxy
Automated [caddy](https://github.com/mholt/caddy) proxy for Docker containers using docker-gen. Inspired by [caddy-proxy](https://github.com/BlackGlory/caddy-proxy) and [nginx-proxy](https://github.com/jwilder/nginx-proxy).

### Usage

Start any containers you want proxied with env vars `VIRTUAL_PATH=/some/path` and `VIRTUAL_HOST=foo.bar.com`.
```sh
    $ docker run -e VIRTUAL_PATH=/foo -e VIRTUAL_HOST=foo.bar.com ...
```

If you want the container protected by HTTP Basic Authentication add a `BASIC_AUTH` env var with the path to protect (i.e. `/`), username, and password:
```sh
    $ docker run -e VIRTUAL_PATH=/foo -e VIRTUAL_HOST=foo.bar.com -e BASIC_AUTH="/ myname mysecret" ...
```

Then to run it:
```sh
    $ docker run -v /var/run/docker.sock:/tmp/docker.sock:ro -v /data/.caddy:/root/.caddy --name caddy-proxy -p 80:80 -p 443:443 -e CADDY_OPTIONS="--email youremail@example.com" -d davad/caddy-path-proxy
```

When you launch new (or stop) containers caddy-proxy will restart to make the new containers available.

If `VIRTUAL_PATH` is empty, `/` is assumed.

### Known bug

Containers with `VIRTUAL_PATH=/` sometimes block containers with `VIRTUAL_PATH=/something/else`.
