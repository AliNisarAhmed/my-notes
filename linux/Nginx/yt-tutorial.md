# Nginx Tutorial 

https://www.youtube.com/watch?v=7VAI73roXaY&t=203s

![d367a1d3a344fe4f89b86adbfaeb2a24.png](d367a1d3a344fe4f89b86adbfaeb2a24.png)


Usually an nginx server is placed in front of actual servers. This setup is known as **Reverse Proxy**

Problem reverse proxy solves is that when we scale an application to inlcude multiple servers, the rev proxy can then act as a "load balancer" and directs the requests to individual servers. 


![f7f870b649e8b4a2886602be9e0859cf.png](f7f870b649e8b4a2886602be9e0859cf.png)


Another use case for nginx is encryption/HTTPS.


## Terminology

Assume this `nginx.conf` file 

```
http {

    include mime.types;

    upstream backendserver {
        server 127.0.0.1:1111;
        server 127.0.0.1:2222;
        server 127.0.0.1:3333;
    }

    server {
        listen 8080;
        root /Users/aa87/Desktop/site;

        rewrite ^/number/(\w+) /count/$1;

        location / {
            proxy_pass http://backendserver/;
        }

        location ~* /count/[0-9] {
            root /Users/aa87/Desktop/site;
            try_files /index.html =404;
        }

        location /fruits {
            root /Users/aa87/Desktop/site;
        }

        location /carbs {
            alias /Users/aa87/Desktop/site/fruits;
        }

        location /vegetables {
            root /Users/aa87/Desktop/site;
            try_files /vegetables/veggies.html /index.html =404;
        }

        location /crops {
            return 307 /fruits;
        }
    }
}

events {

}
```

**Directive**: the key value pair within the blocks (worker_connections: 1024 in the above snippet)
**Context**: The curly braces block of code containing one ore more directives

`root` directive needs a absolute path (and NOT a relative path)

`events` directive is needed, even if empty

`include` directive can add other contexts (like `types` from mime.types)

`location` directive: 
    - takes the `path` and the block, and appends the `path` to the `root`, thus pointing to sub-directory within root
    - can also be used with a regex path using `~*` to indicate start of a regex

`alias` directive: similar to `root`, but does not append the `path` at the end of its argument (thus working opposite to `root` directive)

`try_files` directive: instructs nginx to try one file after another relative to `root`, else return `404` using `=404`

`rewrite` directive: can match a location/route on regex and redirect to another location/route

`return` directive: we can return a status code and redirect to another location. 
- Is limited compared to `rewrite` since `rewrite` can use regex to pattern match

`upstream` directive: defines a group of servers that can be referenced by `proxy_pass`

`proxy_pass` directive: when used inside `location`, it "proxies" all the requests to `location`'s path to the proxied server at the specified address]
- more here: https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/