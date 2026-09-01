# Kubernetes enterprise: base, intermedio ed esperto

## Indice

1. [Base](#base)
2. [Intermedio](#intermedio)
3. [Esperto](#esperto)
4. [Troubleshooting rapido](#troubleshooting-rapido)

## Base

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

## Sicurezza operativa

- Preferire namespace espliciti e `-n production` nei runbook condivisi.
- Non stampare o copiare valori di Secret nei ticket o nei log.
- Usare manifest versionati, `kubectl diff` e admission policy prima del deploy.
- Per `drain`, `delete`, patch manuali e modifica RBAC registrare motivazione e rollback.
