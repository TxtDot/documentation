# Reverse Proxy

## Nginx

Basically, you just need to set the domain, TLS certificates,
Host and X-Forwarded headers (so txtdot could know the hostname)
and pass all requests to txtdot.

```
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;

    # Replace the domain
    server_name txt.dc09.ru;

    ssl_certificate ...pem;
    ssl_certificate_key ...key;
    # More options here:
    # https://ssl-config.mozilla.org/#server=nginx&config=modern

    location / {
        # Replace 8080 port if needed
        proxy_pass http://127.0.0.1:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

On the official instance, TLS is configured in the main nginx config,
so we omit these options below.

Nginx serves static files faster than NodeJS, let's configure it:

```
server {
    ...

    location /static/ {
        alias /home/txtdot/src/dist/static/;
    }
}
```

What about rate-limiting? We don't want the hackers to overload our proxy.

The config below rate-limits to 2 requests per second,
allows to put up to 4 requests into the queue,
sets the maximum size for zone to 10 megabytes.
See the [Nginx blog post](https://www.nginx.com/blog/rate-limiting-nginx/) for detailed explanation.

```
limit_req_zone $binary_remote_addr zone=txtdotapi:10m rate=2r/s;

server {
    ...
    location / {
        limit_req zone=txtdotapi burst=4;
        ...
    }
    ...
}
```

Let's put all together.
Here's our [sample config](https://github.com/TxtDot/txtdot/blob/main/config/nginx.conf):

```
limit_req_zone $binary_remote_addr zone=txtdotapi:10m rate=2r/s;

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    
    server_name txt.dc09.ru;

    location / {
        limit_req zone=txtdotapi burst=4;
        proxy_pass http://127.0.0.1:8080;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /home/txtdot/src/dist/static/;
    }
}
```

## Apache

Coming soon.
If you are familiar with Apache httpd and want to help,
write a config here (a small explanation as above also would be great)
and open a [pull request](https://github.com/txtdot/documentation/pulls).
