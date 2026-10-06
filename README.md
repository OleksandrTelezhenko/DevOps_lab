# DevOps Lab 2 — Volumes & Networks

Nginx app (`web`) connects to Redis (`db`) by container name over a custom network (`app-net`), with a named volume (`db_data`) for data and a bind mount for live code editing.

## Build

```bash
docker build -t my-image .
```

## Run
```
docker network create app-net
docker run -d --name db --network app-net -v db_data:/data redis:alpine
docker run -d --name web --network app-net -p 8080:80 -v ${PWD}:/usr/share/nginx/html nginx:alpine
```

## Verify

- Network: open http://localhost:8080 and run `docker exec -it web ping -c 3 db` — proving `web` resolves `db` by name
- Persistence:
  ```
  docker exec -it db redis-cli SET test "hello"
  docker exec -it db redis-cli SAVE
  docker exec -it db redis-cli GET test
  docker rm -f db
  docker run -d --name db --network app-net -v db_data:/data redis:alpine
  docker exec -it db redis-cli GET test
  ```
value should match — data survives container removal
- Live edit: change `index.html` on host, refresh the browser — no rebuild, no restart
