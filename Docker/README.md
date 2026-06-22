# Docker Tools

Una raccolta strutturata di documentazione Docker — pensata per aiutare gli sviluppatori a trovare rapidamente i comandi, comprendere le best practice e lavorare con ambienti containerizzati in modo efficace.

---

## Scopo

Questo repository funge da riferimento pratico per l'uso quotidiano di Docker e Docker Compose. Che tu stia configurando un ambiente di sviluppo locale, gestendo container in produzione o facendo debug di un servizio attivo, qui trovi i comandi e il contesto necessari — organizzati e pronti all'uso.

---

## Struttura dei File

```
.
├── README.md          # Questo file — panoramica e orientamento
└── tools.md           # Riferimento completo ai comandi Docker, organizzato per categoria
```

---

## Descrizione dei File

### `README.md`
Il punto di ingresso. Fornisce una panoramica dello scopo, una mappa della struttura dei file e una breve introduzione a Docker. Inizia da qui se sei nuovo.

### `tools.md`
Il documento di riferimento principale. Contiene un elenco completo e categorizzato di comandi Docker e Docker Compose, ciascuno con esempi d'uso reali e spiegazioni concise. Le categorie includono: gestione delle immagini, ciclo di vita dei container, volumi, reti, flussi di lavoro con Compose e strumenti di debug.

---

## Introduzione a Docker

**Docker** è una piattaforma open-source per costruire, distribuire ed eseguire applicazioni all'interno di container leggeri e portabili. I container impacchettano un'applicazione insieme a tutte le sue dipendenze — librerie, runtime, configurazione — in modo che funzioni in modo coerente su qualsiasi ambiente, dal laptop dello sviluppatore a un server cloud.

### Concetti fondamentali

| Concetto | Descrizione |
|---|---|
| **Image** | Un modello di sola lettura per un container, costruito a partire da un `Dockerfile` |
| **Container** | Un'istanza in esecuzione di un'immagine — isolata, effimera e riproducibile |
| **Volume** | Storage persistente che sopravvive al riavvio e alla rimozione dei container |
| **Network** | Uno strato di rete virtuale che controlla la comunicazione tra container |
| **Registry** | Un archivio remoto per le immagini (es. Docker Hub, GitHub Container Registry) |
| **Compose** | Uno strumento per definire ed eseguire applicazioni multi-container tramite `docker-compose.yml` |

### Perché usare Docker?

- **Coerenza** — elimina il problema del "funziona solo sulla mia macchina"
- **Isolamento** — ogni servizio gira nel proprio ambiente
- **Velocità** — i container si avviano in millisecondi
- **Portabilità** — eseguibile ovunque sia installato Docker
- **Scalabilità** — facilmente replicabile e orchestrabile su larga scala

Per il riferimento completo ai comandi, consulta [tools.md](./tools.md).
