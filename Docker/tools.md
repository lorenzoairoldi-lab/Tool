# Docker Tools — Base, Intermedio ed Esperto

Ogni voce segue il formato **Comando**, **Spiegazione** ed **Esempio**.

## Base

### `docker version`
**Comando:** `docker version`
**Spiegazione:** Mostra la versione del client e del Docker Engine.
**Esempio:** `docker version`

### `docker info`
**Comando:** `docker info`
**Spiegazione:** Mostra configurazione, storage, reti, runtime e risorse del daemon.
**Esempio:** `docker info`

### `docker help`
**Comando:** `docker help [comando]`
**Spiegazione:** Mostra l'aiuto generale o quello di un comando specifico.
**Esempio:** `docker run --help`

### `docker pull`
**Comando:** `docker pull IMMAGINE:TAG`
**Spiegazione:** Scarica un'immagine da un registry.
**Esempio:** `docker pull nginx:1.27`

### `docker image ls`
**Comando:** `docker image ls`
**Spiegazione:** Elenca le immagini presenti localmente.
**Esempio:** `docker image ls --format 'table {{.Repository}}\t{{.Tag}}\t{{.Size}}'`

### `docker build`
**Comando:** `docker build -t NOME:TAG PERCORSO`
**Spiegazione:** Costruisce un'immagine usando il Dockerfile.
**Esempio:** `docker build -t registry.example.com/api:1.0 .`

### `docker tag`
**Comando:** `docker tag ORIGINE DESTINAZIONE`
**Spiegazione:** Aggiunge un nuovo nome o tag alla stessa immagine.
**Esempio:** `docker tag api:1.0 registry.example.com/platform/api:1.0`

### `docker image inspect`
**Comando:** `docker image inspect IMMAGINE`
**Spiegazione:** Mostra metadati, layer, entrypoint e configurazione dell'immagine.
**Esempio:** `docker image inspect nginx:1.27`

### `docker history`
**Comando:** `docker history IMMAGINE`
**Spiegazione:** Mostra i layer creati dalle istruzioni del Dockerfile.
**Esempio:** `docker history --no-trunc api:1.0`

### `docker run`
**Comando:** `docker run [opzioni] IMMAGINE [comando]`
**Spiegazione:** Crea e avvia un nuovo container.
**Esempio:** `docker run --rm -it alpine:3.20 sh`

### `docker ps`
**Comando:** `docker ps [-a]`
**Spiegazione:** Elenca container attivi; con `-a` include quelli fermi.
**Esempio:** `docker ps -a`

### `docker start`, `stop` e `restart`
**Comando:** `docker start CONTAINER`; `docker stop CONTAINER`; `docker restart CONTAINER`
**Spiegazione:** Avviano, fermano con grazia o riavviano un container esistente.
**Esempio:** `docker stop --time 30 api; docker start api`

### `docker kill`
**Comando:** `docker kill CONTAINER`
**Spiegazione:** Termina immediatamente il processo principale; usarlo solo se `stop` non basta.
**Esempio:** `docker kill api`

### `docker rm`
**Comando:** `docker rm [-f] CONTAINER`
**Spiegazione:** Rimuove un container fermo; `-f` forza anche uno attivo.
**Esempio:** `docker rm api`

### `docker logs`
**Comando:** `docker logs [--follow] [--tail N] CONTAINER`
**Spiegazione:** Legge l'output del container e può seguirlo in tempo reale.
**Esempio:** `docker logs --since 30m --tail 200 api`

### `docker exec`
**Comando:** `docker exec -it CONTAINER COMANDO`
**Spiegazione:** Esegue un comando dentro un container attivo.
**Esempio:** `docker exec -it api sh`

### `docker cp`
**Comando:** `docker cp SORGENTE CONTAINER:PERCORSO`
**Spiegazione:** Copia file tra host e container.
**Esempio:** `docker cp ./config.yaml api:/app/config.yaml`

### `docker inspect`
**Comando:** `docker inspect CONTAINER`
**Spiegazione:** Mostra stato, mount, rete, variabili e limiti in JSON.
**Esempio:** `docker inspect --format '{{.State.Status}}' api`

### `docker top`
**Comando:** `docker top CONTAINER`
**Spiegazione:** Mostra i processi eseguiti nel container.
**Esempio:** `docker top api`

### `docker stats`
**Comando:** `docker stats [CONTAINER]`
**Spiegazione:** Misura CPU, RAM, rete e I/O del container.
**Esempio:** `docker stats api --no-stream`

### `docker volume`
**Comando:** `docker volume create|ls|inspect|rm|prune`
**Spiegazione:** Gestisce storage persistente separato dal container.
**Esempio:** `docker volume create appdata; docker volume inspect appdata`

### `docker network`
**Comando:** `docker network create|ls|inspect|connect|disconnect|rm`
**Spiegazione:** Gestisce reti isolate per la comunicazione tra container.
**Esempio:** `docker network create app-net; docker network connect app-net api`

## Intermedio

### `docker update`
**Comando:** `docker update --cpus N --memory RAM CONTAINER`
**Spiegazione:** Modifica i limiti runtime senza ricreare il container.
**Esempio:** `docker update --cpus 2 --memory 1g api`

### `docker run` con limiti e riavvio
**Comando:** `docker run --restart unless-stopped --cpus 2 --memory 1g --pids-limit 200 IMMAGINE`
**Spiegazione:** Avvia un container con policy di restart e limiti di risorse.
**Esempio:** `docker run -d --name api --restart=unless-stopped --cpus=2 --memory=1g --pids-limit=200 api:1.0`

### `docker run` hardening
**Comando:** `docker run --read-only --cap-drop=ALL --security-opt=no-new-privileges IMMAGINE`
**Spiegazione:** Riduce la superficie d'attacco del container.
**Esempio:** `docker run -d --read-only --cap-drop=ALL --tmpfs /tmp api:1.0`

### `docker login`
**Comando:** `docker login REGISTRY`
**Spiegazione:** Autentica il client a un registry; preferire token a password.
**Esempio:** `docker login registry.example.com`

### `docker push`
**Comando:** `docker push REGISTRY/IMMAGINE:TAG`
**Spiegazione:** Pubblica un'immagine nel registry.
**Esempio:** `docker push registry.example.com/platform/api:1.0`

### `docker compose config`
**Comando:** `docker compose -f FILE config [--quiet]`
**Spiegazione:** Valida e risolve la configurazione Compose senza avviare servizi.
**Esempio:** `docker compose -f compose.yml -f compose.prod.yml config --quiet`

### `docker compose up` e `down`
**Comando:** `docker compose up -d [--build]`; `docker compose down [-v]`
**Spiegazione:** Avviano oppure fermano e rimuovono i servizi Compose; `-v` elimina i volumi.
**Esempio:** `docker compose up -d --build --remove-orphans`

### `docker compose logs` e `exec`
**Comando:** `docker compose logs [-f] [SERVIZIO]`; `docker compose exec SERVIZIO COMANDO`
**Spiegazione:** Leggono i log o eseguono un comando in un servizio attivo.
**Esempio:** `docker compose logs --since 15m api; docker compose exec api sh`

### `docker compose pull`
**Comando:** `docker compose pull [SERVIZIO]`
**Spiegazione:** Scarica le immagini definite nel file Compose.
**Esempio:** `docker compose pull api`

### `docker system df`
**Comando:** `docker system df [-v]`
**Spiegazione:** Analizza lo spazio usato da immagini, container, volumi e cache.
**Esempio:** `docker system df -v`

### `docker system prune`
**Comando:** `docker system prune --filter 'until=168h'`
**Spiegazione:** Elimina dati inutilizzati; usare sempre filtri in produzione.
**Esempio:** `docker system prune --filter 'until=168h'`

## Esperto

### `docker context`
**Comando:** `docker context ls|create|use`
**Spiegazione:** Gestisce connessioni a daemon Docker locali o remoti.
**Esempio:** `docker context create prod --docker 'host=ssh://ops@docker-host'; docker --context prod ps`

### `docker buildx build`
**Comando:** `docker buildx build --platform PLATFORMS --push -t IMMAGINE PERCORSO`
**Spiegazione:** Costruisce con BuildKit e può pubblicare immagini multi-architettura.
**Esempio:** `docker buildx build --platform linux/amd64,linux/arm64 --push -t registry.example.com/api:1.0 .`

### `docker buildx imagetools inspect`
**Comando:** `docker buildx imagetools inspect IMMAGINE`
**Spiegazione:** Mostra manifest, digest e architetture pubblicate.
**Esempio:** `docker buildx imagetools inspect registry.example.com/api:1.0`

### `docker scout cves`
**Comando:** `docker scout cves IMMAGINE`
**Spiegazione:** Cerca vulnerabilità note nei pacchetti dell'immagine.
**Esempio:** `docker scout cves registry.example.com/api:1.0`

### `docker events`
**Comando:** `docker events --since DURATA [--filter FILTRO]`
**Spiegazione:** Mostra gli eventi del daemon per audit e diagnosi.
**Esempio:** `docker events --since 1h --filter type=container`

### `docker diff`
**Comando:** `docker diff CONTAINER`
**Spiegazione:** Elenca file aggiunti, modificati o rimossi dal container.
**Esempio:** `docker diff api`

### `docker save` e `docker load`
**Comando:** `docker save -o FILE IMMAGINE`; `docker load -i FILE`
**Spiegazione:** Esportano o importano immagini tramite archivio offline.
**Esempio:** `docker save -o api.tar api:1.0; docker load -i api.tar`

### `docker export` e `docker import`
**Comando:** `docker export CONTAINER -o FILE`; `docker import FILE IMMAGINE:TAG`
**Spiegazione:** Trasferiscono il filesystem, senza conservare la storia dei layer.
**Esempio:** `docker export api -o api-fs.tar`

### `docker secret` e `docker config`
**Comando:** `docker secret ls|create|inspect`; `docker config ls|create|inspect`
**Spiegazione:** Gestiscono secret e configurazioni in Docker Swarm.
**Esempio:** `printf '%s' "$TOKEN" | docker secret create api-token -`

### `docker swarm`
**Comando:** `docker swarm init|join|leave|update`
**Spiegazione:** Crea o gestisce un cluster di nodi Docker Swarm.
**Esempio:** `docker swarm init --advertise-addr <manager-ip>`

### `docker node`
**Comando:** `docker node ls|inspect|update|drain`
**Spiegazione:** Gestisce nodi, ruoli e disponibilità nel cluster Swarm.
**Esempio:** `docker node update --availability drain <node-id>`

### `docker service`
**Comando:** `docker service create|ls|ps|logs|update|rollback|rm`
**Spiegazione:** Distribuisce e aggiorna servizi replicati in Swarm.
**Esempio:** `docker service create --name api --publish 8080:8080 --replicas 3 api:1.0`

### `docker stack`
**Comando:** `docker stack deploy|ls|services|ps|rm`
**Spiegazione:** Distribuisce applicazioni multi-servizio da YAML su Swarm.
**Esempio:** `docker stack deploy -c stack.yml production`

### `docker plugin`
**Comando:** `docker plugin ls|install|enable|disable|rm`
**Spiegazione:** Gestisce plugin per storage, rete e integrazioni del daemon.
**Esempio:** `docker plugin ls`

---

## Regole operative

- Non usare `--privileged` senza motivazione documentata.
- Non inserire password nei Dockerfile, nei comandi o nei log.
- Verificare immagine, digest, SBOM e vulnerabilità prima della promozione.
- Per `rm`, `prune`, `drain` e `stack rm` verificare sempre target e change approvato.
