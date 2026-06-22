# Docker Tools — Riferimento ai Comandi

Un riferimento completo e categorizzato ai comandi Docker e Docker Compose più utilizzati, con esempi pratici per l'uso quotidiano.

---

## Indice

1. [Comandi Docker di Base](#1-comandi-docker-di-base)
2. [Gestione delle Immagini](#2-gestione-delle-immagini)
3. [Gestione dei Container](#3-gestione-dei-container)
4. [Gestione dei Volumi](#4-gestione-dei-volumi)
5. [Gestione delle Reti](#5-gestione-delle-reti)
6. [Docker Compose — Essenziali](#6-docker-compose--essenziali)
7. [Debug e Ispezione](#7-debug-e-ispezione)

---

## 1. Comandi Docker di Base

Comandi generali per verificare l'installazione di Docker, lo stato del sistema e ottenere aiuto.

| Comando | Descrizione |
|---|---|
| `docker version` | Mostra la versione del client e del daemon Docker |
| `docker info` | Visualizza informazioni generali sul sistema |
| `docker help` | Elenca tutti i comandi disponibili |
| `docker login` | Autenticazione su un registro di container |
| `docker logout` | Disconnessione da un registro |

```bash
# Verificare che Docker sia installato e in esecuzione
docker version

# Visualizzare l'utilizzo del disco (immagini, container, volumi)
docker system df

# Rimuovere tutti i dati inutilizzati (immagini, container, reti, cache di build)
docker system prune -a

# Accedere a Docker Hub
docker login

# Accedere a un registro privato
docker login registry.esempio.com
```

---

## 2. Gestione delle Immagini

Le immagini sono i modelli da cui vengono creati i container. Questi comandi coprono la creazione, il download, il tagging, la pubblicazione e la rimozione delle immagini.

```bash
# Scaricare un'immagine da Docker Hub
docker pull nginx
docker pull node:20-alpine

# Elencare tutte le immagini disponibili in locale
docker images
docker image ls

# Costruire un'immagine dal Dockerfile nella directory corrente
docker build -t mia-app:1.0 .

# Costruire con un Dockerfile specifico e argomenti di build
docker build -f Dockerfile.prod --build-arg ENV=production -t mia-app:prod .

# Assegnare un nuovo tag a un'immagine esistente
docker tag mia-app:1.0 registry.esempio.com/mia-app:1.0

# Pubblicare un'immagine su un registro
docker push registry.esempio.com/mia-app:1.0

# Rimuovere un'immagine specifica
docker rmi mia-app:1.0

# Rimuovere tutte le immagini dangling (senza tag)
docker image prune

# Rimuovere tutte le immagini inutilizzate (non solo le dangling)
docker image prune -a

# Salvare un'immagine in un archivio tar
docker save -o mia-app.tar mia-app:1.0

# Caricare un'immagine da un archivio tar
docker load -i mia-app.tar

# Ispezionare i metadati di un'immagine (layer, variabili d'ambiente, entrypoint, ecc.)
docker inspect mia-app:1.0
```

---

## 3. Gestione dei Container

I container sono istanze in esecuzione di un'immagine. Questi comandi coprono l'intero ciclo di vita — creazione, esecuzione, arresto e rimozione.

### Schema del ciclo di vita

```
docker run → docker stop → docker start → docker rm
                 ↓
           docker kill (forzato)
```

```bash
# Avviare un container in modalità interattiva (rimosso all'uscita)
docker run --rm -it ubuntu bash

# Avviare un container in background (modalità detached)
docker run -d --name webserver nginx

# Avviare con mappatura delle porte (host:container)
docker run -d -p 8080:80 --name webserver nginx

# Avviare con variabili d'ambiente
docker run -d -e NODE_ENV=production -e PORT=3000 mia-app:1.0

# Avviare con un volume montato
docker run -d -v /dati/host:/app/dati mia-app:1.0

# Elencare i container in esecuzione
docker ps

# Elencare tutti i container (inclusi quelli fermi)
docker ps -a

# Fermare un container in modo controllato
docker stop webserver

# Forzare l'arresto immediato di un container
docker kill webserver

# Avviare un container fermo
docker start webserver

# Riavviare un container
docker restart webserver

# Rimuovere un container fermo
docker rm webserver

# Forzare la rimozione di un container in esecuzione
docker rm -f webserver

# Rimuovere tutti i container fermi
docker container prune

# Eseguire un comando all'interno di un container in esecuzione
docker exec -it webserver bash
docker exec webserver ls /etc/nginx

# Copiare file tra host e container
docker cp ./config.json webserver:/app/config.json
docker cp webserver:/app/log ./log-locali
```

---

## 4. Gestione dei Volumi

I volumi forniscono storage persistente disaccoppiato dal ciclo di vita del container. I dati salvati in un volume sopravvivono al riavvio e alla rimozione del container.

| Tipo | Sintassi | Caso d'uso |
|---|---|---|
| Volume con nome | `-v meidati:/app/data` | Dati persistenti gestiti da Docker |
| Bind mount | `-v /percorso/host:/percorso/container` | Sviluppo locale, ricarica del codice in tempo reale |
| Mount tmpfs | `--tmpfs /tmp` | Dati temporanei in memoria |

```bash
# Creare un volume con nome
docker volume create meidati

# Elencare tutti i volumi
docker volume ls

# Ispezionare un volume (punto di mount, driver, ecc.)
docker volume inspect meidati

# Avviare un container con un volume con nome
docker run -d -v meidati:/var/lib/postgresql/data postgres:16

# Avviare un container con bind mount (utile in sviluppo)
docker run -d -v $(pwd):/app -p 3000:3000 mia-app-node

# Rimuovere un volume specifico
docker volume rm meidati

# Rimuovere tutti i volumi inutilizzati
docker volume prune

# Eseguire il backup di un volume in un archivio tar
docker run --rm \
  -v meidati:/dati \
  -v $(pwd):/backup \
  alpine tar czf /backup/meidati-backup.tar.gz -C /dati .

# Ripristinare un volume da un backup
docker run --rm \
  -v meidati:/dati \
  -v $(pwd):/backup \
  alpine tar xzf /backup/meidati-backup.tar.gz -C /dati
```

---

## 5. Gestione delle Reti

Le reti Docker controllano come i container comunicano tra loro e con il mondo esterno.

| Driver di rete | Descrizione |
|---|---|
| `bridge` | Predefinito. Rete isolata su un singolo host |
| `host` | Il container condivide lo stack di rete dell'host |
| `none` | Nessuna rete — completamente isolato |
| `overlay` | Rete multi-host (usata con Swarm) |

```bash
# Elencare tutte le reti
docker network ls

# Creare una rete bridge personalizzata
docker network create mia-rete

# Creare una rete con una subnet specifica
docker network create --subnet=172.20.0.0/16 mia-rete

# Avviare un container collegato a una rete personalizzata
docker run -d --name app --network mia-rete mia-app:1.0

# Collegare un container già in esecuzione a una rete
docker network connect mia-rete webserver

# Disconnettere un container da una rete
docker network disconnect mia-rete webserver

# Ispezionare una rete (container collegati, range IP, ecc.)
docker network inspect mia-rete

# Rimuovere una rete
docker network rm mia-rete

# Rimuovere tutte le reti inutilizzate
docker network prune

# I container sulla stessa rete si raggiungono tramite nome
docker run -d --name db --network mia-rete postgres:16
docker run -d --name app --network mia-rete \
  -e DATABASE_URL=postgres://db:5432/miodb mia-app:1.0
```

---

## 6. Docker Compose — Essenziali

Docker Compose definisce ed esegue applicazioni multi-container tramite un file dichiarativo `docker-compose.yml`.

### Esempio minimo di `docker-compose.yml`

```yaml
version: "3.9"

services:
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    volumes:
      - .:/app
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: segreto
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

### Comandi principali di Compose

```bash
# Avviare tutti i servizi (costruisce le immagini se necessario)
docker compose up

# Avviare in modalità detached (background)
docker compose up -d

# Forzare la ricostruzione delle immagini prima di avviare
docker compose up -d --build

# Fermare e rimuovere container e reti
docker compose down

# Rimuovere anche i volumi con nome
docker compose down -v

# Fermare i servizi senza rimuoverli
docker compose stop

# Avviare i servizi precedentemente fermati
docker compose start

# Riavviare i servizi
docker compose restart

# Visualizzare i log di tutti i servizi
docker compose logs

# Seguire i log di un servizio specifico
docker compose logs -f app

# Elencare i servizi Compose in esecuzione
docker compose ps

# Eseguire un comando una tantum in un container di servizio
docker compose run --rm app bash
docker compose run --rm app python manage.py migrate

# Eseguire un comando in un container di servizio già in esecuzione
docker compose exec app bash

# Scalare un servizio a più istanze
docker compose up -d --scale app=3

# Scaricare le ultime immagini per tutti i servizi
docker compose pull

# Validare e visualizzare la configurazione Compose risolta
docker compose config
```

---

## 7. Debug e Ispezione

Comandi per il debug dei container, l'ispezione dello stato, il monitoraggio delle risorse e la lettura dei log.

```bash
# Seguire i log in tempo reale di un container
docker logs -f webserver

# Mostrare le ultime 100 righe di log con timestamp
docker logs --tail=100 --timestamps webserver

# Monitorare l'utilizzo delle risorse in tempo reale (CPU, memoria, I/O, rete)
docker stats

# Statistiche per un container specifico
docker stats webserver

# Ispezionare i metadati completi di un container (JSON)
docker inspect webserver

# Estrarre un campo specifico con la sintassi Go template
docker inspect --format='{{.NetworkSettings.IPAddress}}' webserver
docker inspect --format='{{.State.Status}}' webserver

# Visualizzare i processi in esecuzione all'interno di un container
docker top webserver

# Mostrare le mappature delle porte di un container
docker port webserver

# Visualizzare le modifiche al filesystem effettuate da un container
docker diff webserver

# Visualizzare la cronologia di build di un'immagine (layer e dimensioni)
docker history mia-app:1.0

# Monitorare gli eventi del daemon Docker in tempo reale
docker events

# Filtrare gli eventi per nome del container
docker events --filter container=webserver

# Aprire una shell in un container in esecuzione
docker exec -it webserver sh       # Immagini Alpine / minimali
docker exec -it webserver bash     # Immagini basate su Debian/Ubuntu

# Avviare un container di debug che condivide il namespace di rete di un altro container
docker run --rm -it --network container:webserver nicolaka/netshoot
```

### Flusso di diagnosi rapida

```bash
# 1. Verificare se il container è in esecuzione
docker ps -a | grep mia-app

# 2. Leggere i log recenti
docker logs --tail=50 mia-app

# 3. Ispezionare lo stato e la configurazione del container
docker inspect mia-app

# 4. Aprire una shell se il container è attivo
docker exec -it mia-app sh

# 5. Controllare il consumo di risorse
docker stats mia-app --no-stream
```

---

*Per la documentazione ufficiale completa, visita [docs.docker.com](https://docs.docker.com).*
