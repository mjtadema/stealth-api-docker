# stealth-api-docker
Self-host the [stealth-api](https://gitlab.com/cosmosapps/stealth-api) using docker, using caddy as a reverse-proxy.

If you want to just run stealth-api without caddy use `docker run ghcr.io/mjtadema/stealth-api-docker:latest`.

The base image was built using all the standard settings of Jib included in the [project](https://gitlab.com/cosmosapps/stealth-api).

A derivative image was then built using this Dockerfile:
```
FROM stealth-api:latest
RUN useradd -m stealthuser
RUN chown -R stealthuser /app
USER stealthuser
WORKDIR /home/stealthuser
```
To provide a little bit better security.

## Requirements:
- Your own domain*
- docker compose installed

*Stealth seems to only work with HTTPS using a properly trusted certificate (not self-signed).

## Instructions
1. clone the repository somewhere on your computer
2. edit `Caddyfile` to include your own domain
3. make sure you have a stealth.[your domain] dns record
4. make sure port 443 is forwarded to the machine on which you want to host stealth-api
5. run `docker compose up -d` to start the service in the background

Caddy then automatically tries to acquire a certificate from lets-encrypt, after which you can add your own stealth-api instance in the stealth settings (settings -> data -> stealth instance).

## Useful links
- [stealth app](https://gitlab.com/cosmosapps/stealth)
- [caddy automatic https](https://caddyserver.com/docs/automatic-https)
- [docker compose](https://docs.docker.com/compose/)

## Credits
All credit for stealth and stealth-api goes to [CosmosApps](https://gitlab.com/cosmosapps), thank you for creating the best privacy-conscious reddit app around.
