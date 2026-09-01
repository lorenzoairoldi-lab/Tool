# Kubernetes Tools

Riferimento progressivo ai comandi `kubectl` per operazioni aziendali: osservabilità,
deploy, networking, storage, sicurezza e troubleshooting.

- [tools.md](./tools.md) — comandi organizzati in Base, Intermedio ed Esperto.

Prima di operare verificare sempre il contesto (`kubectl config current-context`), il
namespace e l'ambiente. I comandi con `delete`, `cordon`, `drain`, `patch` o `rollout undo`
richiedono change approval e vanno eseguiti con least privilege.
