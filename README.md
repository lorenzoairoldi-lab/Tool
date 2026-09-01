# Tool

Raccolta pratica di comandi e quick reference per amministrazione di sistemi,
container e piattaforme cloud-native in contesti aziendali.

## Guide disponibili

| Area | Guida | Contenuti |
|---|---|---|
| Linux | [Linux/tools.md](./Linux/tools.md) | Comandi Linux da Base a Esperto: file, utenti, processi, rete, storage, sicurezza, systemd e troubleshooting. |
| Docker | [Docker/tools.md](./Docker/tools.md) | Docker e Compose: immagini, container, volumi, reti, registry, sicurezza, BuildKit e Swarm. |
| Kubernetes | [Kubernetes/tools.md](./Kubernetes/tools.md) | `kubectl` da Base a Esperto: workload, rollout, networking, storage, RBAC, nodi e debug. |
| Terraform | [Terraform/tools.md](./Terraform/tools.md) | Infrastructure as Code: provider, plan/apply, state, moduli, backend, workspace e troubleshooting. |

Ogni cartella contiene un unico file operativo `tools.md`. Ogni comando è documentato
con questo schema:

- **Comando** — la sintassi da eseguire;
- **Spiegazione** — cosa fa e quando è utile;
- **Esempio** — un caso pratico pronto da adattare.

## Livelli di difficoltà

- **Base:** operazioni quotidiane e comandi fondamentali.
- **Intermedio:** gestione dei servizi, deploy, risorse, rete e automazione.
- **Esperto:** troubleshooting avanzato, sicurezza, audit, performance e manutenzione.

## Uso sicuro

Prima di eseguire un comando verifica ambiente, contesto, namespace e target. I comandi
che eliminano risorse, modificano firewall o permessi, eseguono `drain`, `prune` o
rollback devono essere eseguiti con autorizzazione e con un piano di ripristino.

## Git

La guida Git è disponibile in [Git/tools.md](./Git/tools.md) e contiene i comandi dalla
configurazione iniziale fino a branch, merge, rebase, troubleshooting e gestione avanzata
della cronologia.

## Licenza

MIT
