# Docker enterprise: base, intermedio ed esperto

Esempi per immagini, container e Compose in CI/CD e produzione. Usare digest o tag
immutabili, registry privati e privilegi minimi.

## Base

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

---

## Regole rapide

- Non usare `--privileged` se non indispensabile; preferire `--cap-drop=ALL` e capability mirate.
- Non inserire password nei comandi o nelle immagini: usare secret manager/Compose secrets.
- Validare immagini e configurazione in CI prima della promozione al registry di produzione.
