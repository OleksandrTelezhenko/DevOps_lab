# DevOps Lab 1 — Dockerized Nginx App

## Files

* `Dockerfile`
* `index.html`
* `commands.txt`

## Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
