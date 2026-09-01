# Linux enterprise: base, intermedio ed esperto

Comandi orientati a server aziendali Debian/Ubuntu/RHEL-like. Verificare sempre host,
ambiente e change approval prima di modificare servizi, rete o storage.

## Base

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

---

## Note operative

- Preferire `systemctl stop` e rollout controllati a kill forzati.
- Usare `--dry-run` con `rsync` e conservare checksum/evidenze dei backup.
- Limitare `strace`, `tcpdump` e `ausearch` al tempo necessario e proteggere i dati raccolti.
