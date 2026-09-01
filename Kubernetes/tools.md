# Kubernetes Tools — Base, Intermedio ed Esperto

Ogni voce segue il formato **Comando**, **Spiegazione** ed **Esempio**.

## Base

### `kubectl version` e `cluster-info`
**Comando:** `kubectl version`; `kubectl cluster-info`
**Spiegazione:** Verificano versione del client e raggiungibilità degli endpoint del cluster.
**Esempio:** `kubectl version --client; kubectl cluster-info`

### `kubectl config`
**Comando:** `kubectl config get-contexts|current-context|use-context NOME`
**Spiegazione:** Elenca e seleziona cluster, credenziali e contesti locali.
**Esempio:** `kubectl config get-contexts; kubectl config current-context`

### `kubectl config set-context`
**Comando:** `kubectl config set-context --current --namespace=NAMESPACE`
**Spiegazione:** Imposta il namespace predefinito per evitare di ripeterlo nei comandi.
**Esempio:** `kubectl config set-context --current --namespace=production`

### `kubectl help`
**Comando:** `kubectl help [comando]`
**Spiegazione:** Mostra la sintassi e le opzioni disponibili.
**Esempio:** `kubectl explain deployment --recursive`

### `kubectl api-resources` e `api-versions`
**Comando:** `kubectl api-resources`; `kubectl api-versions`
**Spiegazione:** Elencano rispettivamente le risorse e le versioni API supportate dal cluster.
**Esempio:** `kubectl api-resources --namespaced=true`

### `kubectl explain`
**Comando:** `kubectl explain RISORSA.CAMPO`
**Spiegazione:** Spiega i campi di una risorsa usando lo schema pubblicato dal cluster.
**Esempio:** `kubectl explain deployment.spec.strategy`

### `kubectl get`
**Comando:** `kubectl get RISORSA [NOME] [-A] [-o wide]`
**Spiegazione:** Elenca risorse o mostra una risorsa specifica.
**Esempio:** `kubectl get pods -A -o wide`

### `kubectl describe`
**Comando:** `kubectl describe RISORSA NOME`
**Spiegazione:** Mostra dettagli, condizioni ed eventi utili per capire un problema.
**Esempio:** `kubectl describe pod api-123`

### `kubectl get events`
**Comando:** `kubectl get events --sort-by=.lastTimestamp`
**Spiegazione:** Elenca gli eventi recenti di scheduling, errori e aggiornamenti.
**Esempio:** `kubectl get events -A --sort-by=.lastTimestamp`

### `kubectl apply`
**Comando:** `kubectl apply -f FILE|DIRECTORY`
**Spiegazione:** Crea o aggiorna risorse dichiarate in manifest YAML/JSON.
**Esempio:** `kubectl apply -f k8s/`

### `kubectl diff`
**Comando:** `kubectl diff -f FILE|DIRECTORY`
**Spiegazione:** Mostra le differenze che verrebbero applicate senza cambiare il cluster.
**Esempio:** `kubectl diff -f deployment.yaml --server-side`

### `kubectl delete`
**Comando:** `kubectl delete -f FILE|RESOURCE NOME`
**Spiegazione:** Elimina risorse; controllare sempre namespace e ambiente prima di eseguirlo.
**Esempio:** `kubectl delete pod api-123 --grace-period=30`

### `kubectl logs`
**Comando:** `kubectl logs [-f] POD [-c CONTAINER]`
**Spiegazione:** Legge i log del container, con `-f` li segue in tempo reale.
**Esempio:** `kubectl logs -f pod/api-123 -c api --tail=100 --timestamps`

### `kubectl exec`
**Comando:** `kubectl exec -it POD [-c CONTAINER] -- COMANDO`
**Spiegazione:** Esegue un comando dentro un container in esecuzione.
**Esempio:** `kubectl exec -it pod/api-123 -c api -- sh`

### `kubectl port-forward`
**Comando:** `kubectl port-forward svc/SERVICE PORTA_LOCALE:PORTA_CLUSTER`
**Spiegazione:** Crea un tunnel locale temporaneo per test e debug.
**Esempio:** `kubectl port-forward svc/api 8080:80`

### `kubectl expose`
**Comando:** `kubectl expose deployment DEPLOYMENT --port=PORTA`
**Spiegazione:** Crea un Service che rende raggiungibile un Deployment.
**Esempio:** `kubectl expose deployment api --port=80 --target-port=8080`

### `kubectl create namespace`
**Comando:** `kubectl create namespace NAMESPACE`
**Spiegazione:** Crea un namespace per isolare risorse e permessi.
**Esempio:** `kubectl create namespace production`

### `kubectl create deployment`
**Comando:** `kubectl create deployment NOME --image=IMMAGINE`
**Spiegazione:** Crea rapidamente un Deployment, utile per prove o bootstrap.
**Esempio:** `kubectl create deployment web --image=nginx:1.27`

### `kubectl scale`
**Comando:** `kubectl scale deployment/DEPLOYMENT --replicas=N`
**Spiegazione:** Cambia il numero desiderato di pod replicati.
**Esempio:** `kubectl scale deployment/api --replicas=3`

### `kubectl wait`
**Comando:** `kubectl wait --for=condition/CONDIZIONE RESOURCE/NOME --timeout=120s`
**Spiegazione:** Attende che una risorsa raggiunga una condizione specifica.
**Esempio:** `kubectl wait --for=condition=available deployment/api --timeout=120s`

## Intermedio

### `kubectl rollout`
**Comando:** `kubectl rollout status|history|pause|resume|restart|undo RESOURCE/NOME`
**Spiegazione:** Controlla avanzamento, pause, riavvio e rollback dei Deployment/StatefulSet.
**Esempio:** `kubectl rollout status deployment/api --timeout=5m; kubectl rollout undo deployment/api`

### `kubectl set image`
**Comando:** `kubectl set image deployment/NOME CONTAINER=IMMAGINE`
**Spiegazione:** Cambia l'immagine del container e avvia un rollout.
**Esempio:** `kubectl set image deployment/api api=registry.example.com/api:1.4.2`

### `kubectl create job`
**Comando:** `kubectl create job NOME --image=IMMAGINE -- COMANDO`
**Spiegazione:** Crea un'attività eseguita fino al completamento.
**Esempio:** `kubectl create job migration --image=registry.example.com/api:1.4 -- ./migrate`

### `kubectl create cronjob`
**Comando:** `kubectl create cronjob NOME --schedule='CRON' --image=IMMAGINE -- COMANDO`
**Spiegazione:** Crea un Job ricorrente secondo una pianificazione cron.
**Esempio:** `kubectl create cronjob backup --schedule='0 2 * * *' --image=backup:1.0 -- /backup`

### `kubectl create configmap`
**Comando:** `kubectl create configmap NOME --from-file=FILE|DIRECTORY`
**Spiegazione:** Crea configurazione non segreta da file o valori.
**Esempio:** `kubectl create configmap api-config --from-file=config.yaml`

### `kubectl create secret`
**Comando:** `kubectl create secret generic NOME --from-literal=CHIAVE=VALORE`
**Spiegazione:** Crea un Secret per dati sensibili; non stampare il valore nei log.
**Esempio:** `kubectl create secret generic api-secret --from-literal=TOKEN='<valore>'`

### `kubectl patch`
**Comando:** `kubectl patch RESOURCE/NOME --type=merge -p='JSON'`
**Spiegazione:** Modifica solo alcuni campi senza sostituire tutto il manifest.
**Esempio:** `kubectl patch deployment/api -p '{"spec":{"replicas":4}}'`

### `kubectl autoscale`
**Comando:** `kubectl autoscale deployment/NOME --min=N --max=N --cpu-percent=PERCENTO`
**Spiegazione:** Crea un Horizontal Pod Autoscaler basato sul consumo CPU.
**Esempio:** `kubectl autoscale deployment/api --min=2 --max=10 --cpu-percent=70`

### `kubectl get` per workload
**Comando:** `kubectl get deploy,rs,sts,ds,job,cronjob`
**Spiegazione:** Mostra Deployment, ReplicaSet, StatefulSet, DaemonSet, Job e CronJob.
**Esempio:** `kubectl get deploy,rs,sts,ds,job,cronjob -A`

### `kubectl get` per rete e storage
**Comando:** `kubectl get svc,ingress,endpointslice,networkpolicy,storageclass,pv,pvc`
**Spiegazione:** Mostra ingressi, backend, policy di rete e risorse persistenti.
**Esempio:** `kubectl get svc,endpointslice,ingress -n production`

### `kubectl auth can-i`
**Comando:** `kubectl auth can-i VERBO RISORSA --as=IDENTITÀ`
**Spiegazione:** Verifica se un utente o ServiceAccount possiede una specifica autorizzazione.
**Esempio:** `kubectl auth can-i get pods --as=system:serviceaccount:production:api`

### `kubectl create role` e `rolebinding`
**Comando:** `kubectl create role NOME ...`; `kubectl create rolebinding NOME ...`
**Spiegazione:** Creano permessi namespace-scoped e li assegnano a un'identità.
**Esempio:** `kubectl create role pod-reader --verb=get,list,watch --resource=pods`

### `kubectl top`
**Comando:** `kubectl top nodes|pods [-A]`
**Spiegazione:** Mostra CPU e memoria se Metrics Server è installato.
**Esempio:** `kubectl top pods -A --sort-by=memory`

### `kubectl cp` e `attach`
**Comando:** `kubectl cp POD:PERCORSO LOCALE`; `kubectl attach -it POD`
**Spiegazione:** Copiano file oppure collegano il terminale a un processo già attivo.
**Esempio:** `kubectl cp api-123:/tmp/report ./report`

## Esperto

### `kubectl cordon`, `drain` e `uncordon`
**Comando:** `kubectl cordon NODE`; `kubectl drain NODE`; `kubectl uncordon NODE`
**Spiegazione:** Bloccano nuovi pod, svuotano un nodo per manutenzione e riabilitano lo scheduling.
**Esempio:** `kubectl drain worker-1 --ignore-daemonsets --delete-emptydir-data`

### `kubectl debug`
**Comando:** `kubectl debug pod/POD|node/NODE -it --image=IMMAGINE`
**Spiegazione:** Avvia un ambiente diagnostico temporaneo senza modificare il workload originale.
**Esempio:** `kubectl debug node/worker-1 -it --image=nicolaka/netshoot --profile=general`

### `kubectl proxy`
**Comando:** `kubectl proxy --port=8080`
**Spiegazione:** Espone localmente l'API tramite le credenziali del kubeconfig.
**Esempio:** `kubectl proxy --port=8080`

### `kubectl get --raw`
**Comando:** `kubectl get --raw='/readyz?verbose'`
**Spiegazione:** Interroga direttamente un endpoint dell'API, utile per health check.
**Esempio:** `kubectl get --raw='/version'`

### `kubectl get` con field selector
**Comando:** `kubectl get pods -A --field-selector=FIELD=VALUE`
**Spiegazione:** Filtra risorse usando campi dello stato o della specifica.
**Esempio:** `kubectl get pods -A --field-selector=status.phase=Failed`

### `kubectl get` con jsonpath
**Comando:** `kubectl get RESOURCE -o jsonpath='ESPRESSIONE'`
**Spiegazione:** Estrae solo un valore da una risorsa, utile negli script.
**Esempio:** `kubectl get svc/api -o jsonpath='{.spec.clusterIP}{"\n"}'`

### `kubectl apply --server-side`
**Comando:** `kubectl apply --server-side --field-manager=TEAM -f FILE`
**Spiegazione:** Fa gestire il merge dei campi al server e identifica il team proprietario.
**Esempio:** `kubectl apply --server-side --field-manager=platform -f k8s/`

### `kubectl replace --force`
**Comando:** `kubectl replace --force -f FILE`
**Spiegazione:** Elimina e ricrea la risorsa; può causare downtime e va usato eccezionalmente.
**Esempio:** `kubectl replace --force -f job.yaml`

### `kubectl auth reconcile`
**Comando:** `kubectl auth reconcile -f rbac.yaml --dry-run=server`
**Spiegazione:** Verifica o allinea regole RBAC dichiarate in un manifest.
**Esempio:** `kubectl auth reconcile -f rbac.yaml --dry-run=server`

### `kubectl get` RBAC e admission
**Comando:** `kubectl get role,rolebinding,clusterrole,clusterrolebinding -A`
**Spiegazione:** Ispeziona assegnazioni RBAC a livello namespace e cluster.
**Esempio:** `kubectl get validatingwebhookconfiguration,mutatingwebhookconfiguration`

### `kubectl get` CRD e risorse custom
**Comando:** `kubectl get crd`; `kubectl get RISORSA_CUSTOM`
**Spiegazione:** Mostra estensioni API installate e oggetti gestiti da operatori.
**Esempio:** `kubectl get crd certificates.cert-manager.io -o yaml`

### `kubectl get` quote e scheduling
**Comando:** `kubectl get resourcequota,limitrange`; `kubectl describe node NODE`
**Spiegazione:** Controlla limiti namespace, capacità, taint e condizioni del nodo.
**Esempio:** `kubectl get resourcequota,limitrange -n production`

### `kubectl delete pod --force`
**Comando:** `kubectl delete pod POD --grace-period=0 --force`
**Spiegazione:** Rimuove forzatamente un pod bloccato; può lasciare processi o dati non sincronizzati.
**Esempio:** `kubectl delete pod api-123 --grace-period=0 --force`

---

## Sicurezza operativa

- Verificare sempre `kubectl config current-context` e namespace prima di operare.
- Usare `kubectl diff` e manifest versionati prima di ogni modifica.
- Non stampare Secret nei ticket o nei log.
- Per `delete`, `drain`, `patch`, modifiche RBAC e `replace --force` registrare motivazione e rollback.
