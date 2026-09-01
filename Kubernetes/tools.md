# Kubernetes enterprise: base, intermedio ed esperto

## Indice

1. [Base](#base)
2. [Intermedio](#intermedio)
3. [Esperto](#esperto)
4. [Troubleshooting rapido](#troubleshooting-rapido)

## Base

### Orientamento e risorse

```bash
kubectl help; kubectl api-resources; kubectl api-versions
kubectl explain pod; kubectl explain deployment.spec.template.spec.containers
kubectl get all; kubectl get all -A; kubectl get ns
kubectl get pods -A -o wide; kubectl get pods -A --show-labels
kubectl get pod <pod> -o yaml; kubectl get pod <pod> -o json
kubectl describe node <node>; kubectl describe svc <service>
```

### Manifest e operazioni elementari

```bash
kubectl create namespace production
kubectl create deployment web --image=nginx:1.27 -n production
kubectl expose deployment web --port=80 --target-port=80 -n production
kubectl apply -f deployment.yaml; kubectl apply -k overlays/production
kubectl diff -f deployment.yaml; kubectl delete -f deployment.yaml
kubectl delete pod <pod> --grace-period=30
kubectl label pod <pod> team=platform; kubectl annotate pod <pod> owner=platform
kubectl scale deployment/web --replicas=3 -n production
kubectl wait --for=condition=available deployment/web --timeout=120s -n production
```

```bash
# Client, cluster, contesto e namespace
kubectl version --short
kubectl cluster-info
kubectl config get-contexts
kubectl config current-context
kubectl get ns
kubectl config set-context --current --namespace=production

# Risorse e dettagli essenziali
kubectl get pods -o wide
kubectl get deploy,svc,ingress
kubectl describe pod <pod>
kubectl get events --sort-by=.lastTimestamp

# Log e shell
kubectl logs <pod> --all-containers --tail=100 --timestamps
kubectl logs -f deploy/<deployment> -c <container>
kubectl exec -it <pod> -c <container> -- sh
kubectl port-forward svc/<service> 8080:80
```

## Intermedio

### Workload, job e configurazione

```bash
kubectl get deploy,rs,sts,ds,job,cronjob -A
kubectl create job batch --image=busybox:1.36 -- echo ok
kubectl create cronjob nightly --image=busybox:1.36 --schedule='0 2 * * *' -- date
kubectl get cm,secret,pvc,sa -n production
kubectl create configmap app-config --from-file=config/ -n production
kubectl create secret generic app-secret --from-literal=TOKEN='<value>' -n production
kubectl patch deployment web -p '{"spec":{"replicas":4}}' -n production
kubectl autoscale deployment web --min=2 --max=10 --cpu-percent=70 -n production
```

### Servizi, ingress e storage

```bash
kubectl get svc,endpoints,endpointslice,ingress -A
kubectl expose deployment web --type=ClusterIP --port=80 -n production
kubectl get storageclass; kubectl get pv,pvc -A
kubectl describe pvc <pvc> -n production
kubectl get networkpolicy -A -o yaml
kubectl get serviceaccount -n production
kubectl create role pod-reader --verb=get,list,watch --resource=pods -n production
kubectl create rolebinding app-reader --role=pod-reader --serviceaccount=production:app -n production

```bash
# Deploy, rollout e rollback controllati
kubectl apply -f k8s/ --server-side
kubectl rollout status deploy/<deployment> --timeout=5m
kubectl rollout history deploy/<deployment>
kubectl set image deploy/<deployment> app=registry.example.com/app:1.4.2
kubectl rollout pause deploy/<deployment>
kubectl rollout resume deploy/<deployment>
kubectl rollout undo deploy/<deployment> --to-revision=<revision>

# Filtri, label, risorse e YAML effettivo
kubectl get pods -A -l app=api -o wide
kubectl get pods -A --field-selector=status.phase=Failed
kubectl get pods -o custom-columns=NAME:.metadata.name,CPU:.status.containerStatuses[*].restartCount
kubectl top nodes; kubectl top pods -A --sort-by=memory
kubectl get deploy/<deployment> -o yaml

# Networking e configurazione
kubectl get endpointslice -l kubernetes.io/service-name=<service>
kubectl get networkpolicy -A
kubectl get configmap,secret
kubectl auth can-i get pods --as=system:serviceaccount:production:app
```

## Esperto

### API, patch e controllo accessi

```bash
kubectl proxy --port=8080
kubectl get --raw='/readyz?verbose'; kubectl get --raw='/version'
kubectl auth can-i --list -n production
kubectl auth can-i create deployments --as=system:serviceaccount:production:app -n production
kubectl get role,rolebinding,clusterrole,clusterrolebinding -A
kubectl get validatingwebhookconfiguration,mutatingwebhookconfiguration
kubectl patch deployment web --type=json -p='[{"op":"replace","path":"/spec/replicas","value":3}]'
kubectl replace --force -f manifest.yaml
kubectl api-resources --verbs=list --namespaced -o name | xargs -n1 kubectl get --ignore-not-found
```

### Scheduling, rollout e osservabilità

```bash
kubectl get nodes -o custom-columns=NAME:.metadata.name,TAINTS:.spec.taints
kubectl get pods -A --field-selector=status.phase!=Running
kubectl get pods -A --sort-by=.status.containerStatuses[0].restartCount
kubectl get --raw='/apis/metrics.k8s.io/v1beta1/nodes'
kubectl rollout history statefulset/<name>; kubectl rollout restart deployment/<name>
kubectl rollout status statefulset/<name> --timeout=10m
kubectl logs deployment/<name> --all-containers --since=1h --prefix
kubectl attach -it <pod> -c <container>; kubectl cp <pod>:/tmp/report ./report
kubectl auth reconcile -f rbac.yaml --dry-run=server
```

### Backup, manutenzione e debug

```bash
kubectl get customresourcedefinition; kubectl get crd <name> -o yaml
kubectl get leases -A; kubectl get componentstatuses 2>/dev/null
kubectl debug pod/<pod> -it --copy-to=<pod>-debug --container=<container> -- sh
kubectl debug node/<node> -it --image=nicolaka/netshoot --profile=general
kubectl cordon <node>; kubectl drain <node> --ignore-daemonsets --delete-emptydir-data
kubectl uncordon <node>
kubectl delete pod <pod> --grace-period=0 --force
kubectl get -A -o yaml > cluster-export.yaml
```

```bash
# Diagnostica nodi e scheduling
kubectl get nodes -o wide
kubectl describe node <node>
kubectl get pods -A --field-selector spec.nodeName=<node>
kubectl cordon <node>
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data --grace-period=60
kubectl uncordon <node>

# Debug effimero e rete senza modificare il workload
kubectl debug node/<node> -it --image=nicolaka/netshoot --image-pull-policy=IfNotPresent
kubectl run netshoot --rm -it --restart=Never --image=nicolaka/netshoot -- sh
kubectl get svc <service> -o jsonpath='{.spec.clusterIP}{"\n"}'
kubectl get endpoints <service> -o yaml

# Audit di sicurezza e RBAC
kubectl auth can-i --list --as=system:serviceaccount:production:app
kubectl get role,rolebinding,clusterrole,clusterrolebinding -A
kubectl get pod <pod> -o jsonpath='{.spec.securityContext}{"\n"}'
kubectl get pod <pod> -o jsonpath='{range .spec.containers[*]}{.name}{" ",.securityContext}{"\n"}{end}'

# Server-side diff e applicazione idempotente
kubectl diff -f k8s/ --server-side
kubectl apply -f k8s/ --server-side --field-manager=platform
kubectl get events -A --field-selector=type=Warning --sort-by=.lastTimestamp
```

## Troubleshooting rapido

```bash
# Pod Pending: scheduling, risorse, PVC e taint
kubectl describe pod <pod> | grep -E 'Events|Warning|FailedScheduling'
kubectl get resourcequota,limitrange,pvc
kubectl describe node <node>

# CrashLoopBackOff: stato precedente, exit code e log
kubectl logs <pod> --previous --all-containers
kubectl describe pod <pod>
kubectl get pod <pod> -o jsonpath='{range .status.containerStatuses[*]}{.name}{" exit="}{.lastState.terminated.exitCode}{" reason="}{.lastState.terminated.reason}{"\n"}{end}'

# Servizio non raggiungibile
kubectl get svc,endpointslice <service> -o wide
kubectl describe svc <service>
kubectl run dns-test --rm -it --restart=Never --image=busybox:1.36 -- nslookup <service>

# Stato del rollout e replica set associati
kubectl rollout status deploy/<deployment> --timeout=2m
kubectl get rs,pods -l app=<app> -o wide
```

> Nota: adattare l'immagine debug al registry aziendale e alle policy del cluster.

---

## Spiegazione comando per comando

| Comando | Spiegazione semplice |
|---|---|
| `kubectl help` | Mostra l'aiuto generale e la sintassi dei comandi. |
| `kubectl api-resources` | Elenca le risorse disponibili nel cluster. |
| `kubectl explain` | Spiega i campi di una risorsa, direttamente dalla API. |
| `kubectl cluster-info` | Verifica che il cluster risponda e mostra gli endpoint principali. |
| `kubectl config get-contexts` | Elenca i cluster e le identità configurate localmente. |
| `kubectl config current-context` | Mostra il cluster verso cui stai operando. |
| `kubectl config set-context` | Imposta il namespace predefinito del contesto corrente. |
| `kubectl get` | Elenca risorse o mostra una risorsa specifica. |
| `kubectl describe` | Mostra dettagli leggibili ed eventi di una risorsa. |
| `kubectl apply -f` | Crea o aggiorna risorse a partire da un manifest. |
| `kubectl apply -k` | Applica una directory Kustomize con overlay e variabili. |
| `kubectl diff` | Mostra cosa cambierebbe senza applicarlo. |
| `kubectl delete` | Elimina una risorsa; richiede particolare attenzione in produzione. |
| `kubectl label/annotate` | Aggiunge metadati usati da selettori e strumenti esterni. |
| `kubectl scale` | Cambia il numero desiderato di repliche. |
| `kubectl wait` | Attende che una condizione, ad esempio Available, diventi vera. |
| `kubectl logs` | Legge i log di uno o più container. Con `--previous` legge il container precedente. |
| `kubectl exec` | Esegue un comando dentro un container. |
| `kubectl port-forward` | Crea un tunnel locale verso pod o service per debug. |
| `kubectl create job/cronjob` | Crea un'attività una tantum o pianificata. |
| `kubectl create configmap` | Crea configurazione non segreta da file o valori. |
| `kubectl create secret` | Crea dati sensibili codificati per i pod. Non stampa i valori nei ticket. |
| `kubectl patch` | Modifica solo alcuni campi di una risorsa esistente. |
| `kubectl rollout status` | Attende e mostra l'avanzamento di un aggiornamento. |
| `kubectl rollout history` | Mostra le revisioni disponibili per un rollback. |
| `kubectl rollout undo` | Ripristina una revisione precedente. |
| `kubectl rollout restart` | Ricrea progressivamente i pod senza cambiare l'immagine. |
| `kubectl rollout pause/resume` | Sospende o riprende un rollout per applicare più modifiche insieme. |
| `kubectl set image` | Cambia l'immagine di un workload e avvia il relativo rollout. |
| `kubectl top` | Mostra consumo CPU e memoria, se Metrics Server è installato. |
| `kubectl get events` | Elenca eventi di scheduling, errori e aggiornamenti recenti. |
| `kubectl auth can-i` | Verifica se un utente o service account può compiere un'azione. |
| `kubectl get rolebinding` | Mostra quali ruoli sono assegnati a utenti o service account. |
| `kubectl get svc/endpointslice` | Verifica service e pod backend realmente raggiungibili. |
| `kubectl get pvc/pv` | Mostra richieste e volumi persistenti. |
| `kubectl get resourcequota/limitrange` | Mostra limiti e quote di risorse applicati a un namespace. |
| `kubectl get crd` | Elenca le risorse personalizzate installate nel cluster. |
| `kubectl get leases` | Mostra lease usati, tra l'altro, per leader election. |
| `kubectl run` | Crea un pod temporaneo, utile per test DNS o rete. |
| `kubectl attach` | Collega il terminale a un processo già attivo nel container. |
| `kubectl cp` | Copia file tra pod e macchina locale. |
| `kubectl cordon` | Impedisce nuovi pod su un nodo senza fermare quelli già presenti. |
| `kubectl drain` | Svuota un nodo spostando i pod gestiti da controller. |
| `kubectl uncordon` | Rende nuovamente schedulabili nuovi pod sul nodo. |
| `kubectl debug` | Avvia un ambiente temporaneo per diagnosticare pod o nodi. |
| `kubectl proxy` | Espone localmente l'API Kubernetes tramite il kubeconfig corrente. |
| `kubectl get --raw` | Interroga direttamente endpoint dell'API, utile per health check. |
| `kubectl replace --force` | Ricrea una risorsa: può causare downtime, usarlo solo eccezionalmente. |
| `kubectl auth reconcile` | Confronta e allinea regole RBAC con un manifest. |

## Sicurezza operativa

- Preferire namespace espliciti e `-n production` nei runbook condivisi.
- Non stampare o copiare valori di Secret nei ticket o nei log.
- Usare manifest versionati, `kubectl diff` e admission policy prima del deploy.
- Per `drain`, `delete`, patch manuali e modifica RBAC registrare motivazione e rollback.
