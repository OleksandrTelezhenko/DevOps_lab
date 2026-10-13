# DevOps Lab 3 — Docker Compose & Multi-stage Builds

Nginx app (`web`) connects to PostgreSQL (`db`) via Docker Compose with a named volume (`db_data`) and a bind mount. Multi-stage builds compare Node.js image sizes.

## Build & Run Compose

```yaml
services:
  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db_data:/var/lib/postgresql/data

  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./site:/usr/share/nginx/html
    depends_on:
      - db

volumes:
  db_data:
```

Run the Compose stack:

```powershell
docker compose up -d
```

## Verify Compose

**Status & Network:**

```powershell
docker compose ps
docker compose exec web ping -c 2 db
docker compose down
```

## Multi-stage Build

Build and compare the Docker images:

```powershell
docker build -f Dockerfile.single -t myapp-single .
docker build -f Dockerfile.multi -t myapp-multi .
docker images | Select-String "myapp-"
```

**Expected result:** `myapp-multi` should be significantly smaller than `myapp-single` because it uses an Alpine-based runtime image and excludes unnecessary build dependencies and files from the final image.

## Screenshots
(./Screenshots/2.png)
(./Screenshots/1.png)
(./Screenshots/3.png)
