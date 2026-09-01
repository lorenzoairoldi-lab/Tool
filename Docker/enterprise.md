# Docker enterprise: base, intermedio ed esperto

Esempi per immagini, container e Compose in CI/CD e produzione. Usare digest o tag
immutabili, registry privati e privilegi minimi.

## Base

### CLI e immagini

```bash
docker --help; docker <command> --help; docker version; docker info
docker images; docker image ls; docker pull alpine:3.20
docker build -t myapp:dev .; docker tag myapp:dev registry.example.com/myapp:dev
docker history myapp:dev; docker image inspect myapp:dev
docker rmi myapp:dev; docker image prune
docker save -o myapp.tar myapp:dev; docker load -i myapp.tar
```

### Container, volumi e reti

```bash
docker run --rm hello-world
docker run -d --name web -p 8080:80 nginx:1.27
docker ps; docker ps -a; docker start web; docker stop web; docker restart web
docker rename web web-prod; docker logs -f web; docker exec -it web sh
docker cp ./index.html web:/usr/share/nginx/html/index.html
docker inspect web; docker top web; docker port web; docker stats --no-stream web
docker volume create appdata; docker volume ls; docker volume inspect appdata
docker run -d --mount source=appdata,target=/data alpine sleep 3600
docker network create app-net; docker network ls; docker network inspect app-net
docker network connect app-net web; docker network disconnect app-net web
docker rm -f web; docker volume rm appdata; docker network rm app-net
```

```bash
# Stato del daemon, spazio e container
docker version; docker info; docker system df
docker ps -a --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}'
# Ciclo di vita e log
docker start <container>; docker stop --time 30 <container>
docker logs --tail 100 --timestamps <container>
docker exec -it <container> sh
# Compose essenziale
docker compose config --quiet; docker compose up -d; docker compose ps
```

## Intermedio

```bash
# Avvio con limiti, restart policy e filesystem read-only
docker run -d --name api --restart=unless-stopped --cpus=2 --memory=1g \
  --pids-limit=200 --read-only --tmpfs /tmp:rw,noexec,nosuid,size=64m api:1.4.2
# Ispezione di rete, mount, processi e consumo
docker inspect api; docker port api; docker top api; docker stats api --no-stream
# Registry e release versionata
docker login registry.example.com
docker tag api:1.4.2 registry.example.com/platform/api:1.4.2
docker push registry.example.com/platform/api:1.4.2
# Compose in produzione e aggiornamento mirato
docker compose -f compose.yml -f compose.prod.yml config --quiet
docker compose -f compose.yml -f compose.prod.yml pull api
docker compose -f compose.yml -f compose.prod.yml up -d --no-deps api
```

## Esperto

```bash
# Build multi-arch con BuildKit, SBOM e provenance
docker buildx create --name ci-builder --use
docker buildx build --platform linux/amd64,linux/arm64 \
  -t registry.example.com/platform/api:1.4.2 --push --provenance=true --sbom=true .
docker buildx imagetools inspect registry.example.com/platform/api:1.4.2
docker scout cves registry.example.com/platform/api:1.4.2
# Evidenze e sicurezza runtime
docker inspect --format '{{.HostConfig.Privileged}} {{json .HostConfig.CapDrop}}' api
docker inspect --format '{{json .Mounts}}' api; docker diff api
docker events --since 1h --filter type=container
# Debug isolato sulla rete del container
docker run --rm -it --network container:api nicolaka/netshoot
# Pulizia limitata nel tempo; evitare prune -a indiscriminato in produzione
docker system prune --filter 'until=168h'
```

### Compose completo, context e manifest

```bash
docker compose version; docker compose config; docker compose ps --all
docker compose build --pull --no-cache; docker compose pull
docker compose up -d --build --remove-orphans
docker compose exec api sh; docker compose run --rm api sh
docker compose restart api; docker compose stop; docker compose start
docker compose cp api:/app/logs ./evidence/logs
docker compose down --remove-orphans; docker compose down -v
docker context ls; docker context show
docker context create prod --docker 'host=ssh://ops@docker-host'
docker --context prod ps
docker manifest inspect registry.example.com/platform/api:1.4.2
docker buildx ls; docker buildx du; docker buildx prune --filter until=168h
```

### Security, backup e Docker Engine

```bash
# Secrets e configurazione
docker secret ls; docker config ls
docker trust inspect --pretty registry.example.com/platform/api:1.4.2
docker scout recommendations registry.example.com/platform/api:1.4.2
# Backup di un volume e verifica dell'archivio
docker run --rm -v appdata:/data -v "$PWD":/backup alpine \
  tar -czf /backup/appdata.tgz -C /data .
tar -tzf appdata.tgz >/dev/null
# Daemon, plugin, swarm e nodi (solo se l'organizzazione usa Swarm)
docker events --since 1h; docker plugin ls; docker info
docker swarm init --advertise-addr <manager-ip>
docker node ls; docker service ls; docker stack ls
docker service ps <service>; docker service logs --follow <service>
docker stack deploy -c stack.yml production; docker stack services production
```

---

## Regole rapide

- Non usare `--privileged` se non indispensabile; preferire `--cap-drop=ALL` e capability mirate.
- Non inserire password nei comandi o nelle immagini: usare secret manager/Compose secrets.
- Validare immagini e configurazione in CI prima della promozione al registry di produzione.
