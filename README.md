# Docker for modx revolution 

*This is a modified version of the FPM variant of [the official Docker Image](https://github.com/modxcms/docker-modx).*

## Simplified Usage
1. Copy the `.env.example` file to `.env`
   1. Change domain name in Caddyfile from localhost to your domain name, for example: ``{your-domain-name} { ... }``
2. Run `docker-compose up`
3. The Webserver will now listen on Ports `80` and `443`
4. Copy MODX code into html folder
5. Visit `http://localhost/manager` for local dev and `https://{your-domain}/manager` for prod env

# Additional information
1. For changing php version you have to change build/Dockerfile: ``FROM php:8.1-fpm-alpine``
2. For changing modx version you have to change build/Dockerfile: ``ARG MODX_VERSION=2.8.3``
   1. The last version of modx is located [here](https://modx.com/download)
3. The Caddy web server is an extensible, cross-platform, open-source web server written in Go. More information [here](https://caddyserver.com/)


## Problems
1. The Friendly URLs part doesn't work properly !!!
   2. Possible solution: https://caddy.community/t/caddy2-matcher-rewrite-modx/7206
