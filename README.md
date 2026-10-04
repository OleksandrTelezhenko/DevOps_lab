# DevOps Lab 1 — Dockerized Nginx App

## Files

* `Dockerfile`
* `index.html`
* `commands.txt`

## Dockerfile

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/
```

## Build
```bash
docker build -t my-devops-app .
```

## Run
```bash
docker run -d -p 8080:80 --name my-web my-devops-app
```

## Verify
* http://localhost:8080
* Enter container: docker exec -it my-web /bin/sh

## Screenshots
![Browser](./Screenshotes/Br.png)
![Bash](./Screenshotes/bash.png)
