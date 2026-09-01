# Linux enterprise: base, intermedio ed esperto

Comandi orientati a server aziendali Debian/Ubuntu/RHEL-like. Verificare sempre host,
ambiente e change approval prima di modificare servizi, rete o storage.

## Base

### Navigazione, file e testo

```bash
pwd; ls; ls -lah; cd /var/log
mkdir -p /srv/app/{config,data,logs}; touch /tmp/test.txt
cp -a source/ destination/; mv old.conf new.conf; rm -i file.txt
file archive.tar.gz; stat /etc/hosts; realpath ./config.yaml
cat file.txt; less file.txt; head -n 20 file.txt; tail -n 50 file.txt
grep -n 'pattern' file.txt; sort file.txt | uniq -c; wc -l file.txt
cut -d: -f1 /etc/passwd; sed -n '1,80p' file.txt; awk '{print $1}' file.txt
find /var/log -type f -name '*.log' -mtime -7 -print
tar -czf backup.tgz /srv/app; tar -tzf backup.tgz; tar -xzf backup.tgz
```

### Utenti, permessi e pacchetti

```bash
whoami; id; groups; who; w
sudo -l; chmod 640 config.yaml; chown app:app config.yaml
getent passwd app; getent group app; passwd -S app
sudo apt update; apt-cache policy nginx; sudo apt install nginx
sudo apt remove nginx; sudo apt autoremove
# RHEL/Fedora: dnf check-update; sudo dnf install nginx; rpm -qa | grep nginx
```

```bash
# Identità dell'host, versione e risorse
hostnamectl; cat /etc/os-release; uptime; free -h; df -hT
# Processi, porte e servizi
ps aux --sort=-%cpu | head -20; ss -lntup; systemctl --failed
# Log recenti e stato di un servizio
journalctl -p warning..alert -S today --no-pager
systemctl status nginx --no-pager
# Spazio occupato e file di log grandi
du -xhd1 /var | sort -h
find /var/log -type f -size +500M -ls
```

## Intermedio

```bash
# Seguire i log e filtrare errori/timeout
journalctl -u nginx --since '1 hour ago' -f
grep -RniE 'error|critical|failed|timeout' /var/log --include='*.log'
# Diagnostica CPU, memoria, I/O e rete
pidstat -dur 2 5; iostat -xz 2 5; vmstat 2 5; sar -n DEV 2 5
# DNS, TLS e raggiungibilità applicativa
resolvectl query api.example.com
curl -fsS --connect-timeout 5 -I https://api.example.com/health
openssl s_client -connect api.example.com:443 -servername api.example.com </dev/null
# Backup incrementale con ACL, xattr e dry-run
rsync -aHAX --numeric-ids --delete --dry-run /srv/app/ backup:/srv/app/
```

## Esperto

```bash
# Dipendenze, boot e limiti del servizio
systemctl list-dependencies nginx; systemd-analyze blame
systemctl show nginx -p MainPID,MemoryCurrent,CPUUsageNSec
cat /proc/$(pidof nginx | awk '{print $1}')/limits
# Audit di autenticazioni, sudo e file sensibili
last -ai | head -20; journalctl _COMM=sudo --since '7 days ago' --no-pager
ausearch -m USER_LOGIN,USER_AUTH -ts recent 2>/dev/null
find / -xdev -type f -perm /6000 -ls 2>/dev/null
# Tracing e socket aperti di un processo (usare durante una finestra approvata)
lsof -p $(pidof nginx | awk '{print $1}')
strace -ff -p <PID> -e trace=network,file -tt -o /tmp/trace.%p
# Verifica firewall senza cambiare configurazione
nft list ruleset; ufw status verbose 2>/dev/null || true
```

### Networking avanzato, storage e automazione

```bash
# Interfacce, route, ARP e diagnostica TCP
ip -br addr; ip route; ip neigh; ethtool eth0
tcpdump -ni eth0 host 10.0.0.10 and port 443 -c 100
traceroute -T -p 443 api.example.com; mtr -rwzc 20 api.example.com
dig +trace api.example.com; nc -vz api.example.com 443
# Filesystem, mount, LVM e quota (verificare device prima di modificare)
lsblk -f; blkid; findmnt; mount | column -t
df -ih; du -xhd1 /var | sort -h; lsof +L1
pvs; vgs; lvs -a -o +devices; quota -s
# Pianificazione e gestione delle unità
crontab -l; systemctl list-timers --all
systemctl cat app.service; systemctl daemon-reload
systemctl enable --now app.service; systemctl restart app.service
```

### Diagnostica di basso livello e compliance

```bash
# Processi, file descriptor, namespace e cgroup
ps -ef --forest; pgrep -a -f app; pstree -ap <PID>
readlink /proc/<PID>/exe; ls -l /proc/<PID>/fd | head
nsenter -t <PID> -m -u -i -n -p -- ps aux
systemd-cgtop; ulimit -a; sysctl -a 2>/dev/null | sort
# Integrità, cifratura e MAC
sha256sum release.bin; gpg --verify release.sig release.bin
getfacl -R /srv/app; sestatus 2>/dev/null || aa-status 2>/dev/null
find / -xdev -type f -perm /6000 -ls 2>/dev/null
# Core dump, syscall e performance (solo con approvazione)
coredumpctl list; coredumpctl info <PID>
perf top -p <PID>; strace -ff -p <PID> -e trace=network,file -tt
```

---

## Spiegazione comando per comando

| Comando | Spiegazione semplice |
|---|---|
| `pwd` | Mostra la directory in cui ti trovi. |
| `ls -lah` | Elenca anche file nascosti, dimensioni e permessi in formato leggibile. |
| `cd /var/log` | Entra nella directory indicata. |
| `mkdir -p` | Crea directory e, con `-p`, anche quelle intermedie mancanti. |
| `cp -a` | Copia file o directory preservando permessi e metadati. |
| `mv` | Sposta o rinomina un file. |
| `rm -i` | Elimina chiedendo conferma: utile per evitare errori. |
| `stat` | Mostra dimensione, proprietario, permessi e date di un file. |
| `less` | Legge un file pagina per pagina senza modificarlo. |
| `grep -n` | Cerca testo e mostra anche il numero di riga. |
| `sort \| uniq -c` | Ordina le righe e conta quante volte compare ciascuna. |
| `sed` | Seleziona o modifica testo automaticamente. |
| `awk` | Estrae colonne e produce report da testo strutturato. |
| `find` | Cerca file usando nome, età, dimensione o altri criteri. |
| `tar -czf` | Crea un archivio compresso `.tgz`. |
| `tar -tzf` | Elenca il contenuto di un archivio senza estrarlo. |
| `tar -xzf` | Estrae un archivio compresso. |
| `whoami` / `id` | Mostrano l'utente corrente, UID, GID e gruppi. |
| `sudo -l` | Mostra quali comandi l'utente può eseguire come amministratore. |
| `chmod 640` | Imposta i permessi: proprietario lettura/scrittura, gruppo sola lettura. |
| `chown app:app` | Cambia proprietario e gruppo di un file. |
| `getent` | Cerca utenti o gruppi usando la configurazione directory del sistema. |
| `apt update` | Aggiorna l'elenco dei pacchetti disponibili. |
| `apt install` | Installa un pacchetto e le sue dipendenze. |
| `rpm -qa` | Elenca i pacchetti installati su sistemi RPM. |
| `hostnamectl` | Mostra o modifica il nome e le informazioni dell'host. |
| `uptime` | Mostra da quanto il server è attivo e il carico medio. |
| `free -h` | Mostra memoria RAM e swap in unità leggibili. |
| `df -hT` | Mostra spazio libero e tipo dei filesystem. |
| `ps aux` | Elenca i processi attivi e il loro consumo. |
| `ss -lntup` | Mostra porte in ascolto e processi associati. |
| `systemctl status` | Mostra lo stato e gli ultimi log di un servizio systemd. |
| `journalctl -u` | Legge i log di uno specifico servizio. |
| `journalctl -p` | Filtra i log per gravità, ad esempio warning o errori. |
| `pidstat` | Misura CPU, memoria, disco e I/O per processo. |
| `iostat` | Mostra carico CPU e tempi di risposta dei dischi. |
| `vmstat` | Riassume memoria, processi, swap e I/O. |
| `sar` | Legge statistiche di sistema raccolte nel tempo. |
| `resolvectl query` | Verifica come il sistema risolve un nome DNS. |
| `curl -I` | Invia una richiesta HTTP e mostra solo gli header. |
| `openssl s_client` | Verifica handshake, certificato e protocollo TLS. |
| `ip route` | Mostra le rotte usate per raggiungere le reti. |
| `tcpdump` | Cattura pacchetti per capire cosa transita sulla rete. |
| `lsblk` / `findmnt` | Mostrano dischi, partizioni e relativi mount point. |
| `lsof +L1` | Trova file eliminati ma ancora tenuti aperti da processi. |
| `crontab -l` | Mostra i job pianificati dell'utente corrente. |
| `systemctl enable --now` | Abilita un servizio all'avvio e lo avvia subito. |
| `sha256sum` | Calcola un checksum per verificare l'integrità di un file. |
| `ausearch` | Cerca eventi di sicurezza nel Linux Audit log. |
| `nft list ruleset` | Mostra le regole firewall attive senza modificarle. |
| `strace` | Mostra le system call eseguite da un processo per diagnosticare blocchi. |
| `perf top` | Mostra in tempo reale dove un processo usa CPU. |

## Note operative

- Preferire `systemctl stop` e rollout controllati a kill forzati.
- Usare `--dry-run` con `rsync` e conservare checksum/evidenze dei backup.
- Limitare `strace`, `tcpdump` e `ausearch` al tempo necessario e proteggere i dati raccolti.
