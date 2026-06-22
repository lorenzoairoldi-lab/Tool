# Linux Tools — Guida Essenziale

Elenco completo dei tool più utilizzati nei sistemi Linux (Debian, Ubuntu e derivate), organizzati per categoria. Ogni tool include nome, descrizione, comando principale, esempio d'uso e distribuzioni target.

---

## Indice

1. [Networking](#1-networking)
2. [System Monitoring & Performance](#2-system-monitoring--performance)
3. [Package Management](#3-package-management)
4. [File Management & Text Processing](#4-file-management--text-processing)
5. [Process Management](#5-process-management)
6. [Disk & Storage](#6-disk--storage)
7. [System Information](#7-system-information)
8. [Security](#8-security)
9. [User & Group Management](#9-user--group-management)
10. [Task Scheduling & Automation](#10-task-scheduling--automation)
11. [Shell & Terminal Utilities](#11-shell--terminal-utilities)

---

## 1. Networking

### `curl`
**Trasferimento dati da o verso un server tramite protocolli supportati (HTTP, FTP, ecc.).**

- **Comando:** `curl [opzioni] URL`
- **Esempio:** `curl -O https://example.com/file.zip`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch, openSUSE

### `wget`
**Download ricorsivo e non interattivo di file dal web.**

- **Comando:** `wget [opzioni] URL`
- **Esempio:** `wget -c https://example.com/iso/debian.iso`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch, openSUSE

### `netstat`
**Visualizza connessioni di rete, tabelle di routing e statistiche delle interfacce.**

- **Comando:** `netstat [opzioni]`
- **Esempio:** `netstat -tulpn`
- **Distribuzioni:** Debian, Ubuntu, CentOS (net-tools)

### `ss`
**Alternativa moderna a `netstat` per investigare socket di rete.**

- **Comando:** `ss [opzioni]`
- **Esempio:** `ss -tuln`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (iproute2)

### `ip`
**Gestione indirizzi, route e interfacce di rete (sostituisce ifconfig).**

- **Comando:** `ip [sotto-comando] [opzioni]`
- **Esempio:** `ip addr show`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch, openSUSE

### `nmap`
**Scanner di rete per scoprire host e servizi attivi.**

- **Comando:** `nmap [opzioni] target`
- **Esempio:** `nmap -sS -A 192.168.1.0/24`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch, Kali

### `traceroute`
**Traccia il percorso dei pacchetti verso un host remoto.**

- **Comando:** `traceroute [opzioni] host`
- **Esempio:** `traceroute -n 8.8.8.8`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `mtr`
**Combina `traceroute` e `ping` in un unico tool diagnostico in tempo reale.**

- **Comando:** `mtr [opzioni] host`
- **Esempio:** `mtr -r -c 10 google.com`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `ping`
**Verifica la raggiungibilità di un host tramite ICMP echo request.**

- **Comando:** `ping [opzioni] host`
- **Esempio:** `ping -c 4 8.8.8.8`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `dig`
**Esegue query DNS dettagliate.**

- **Comando:** `dig [@server] nome [tipo]`
- **Esempio:** `dig +short example.com A`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (dnsutils / bind-utils)

### `nslookup`
**Interroga server DNS in modo interattivo o diretto.**

- **Comando:** `nslookup [host] [server]`
- **Esempio:** `nslookup google.com`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `tcpdump`
**Cattura e analizza pacchetti di rete da riga di comando.**

- **Comando:** `tcpdump [opzioni] [espressione]`
- **Esempio:** `tcpdump -i eth0 port 80`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `iptables`
**Firewall a riga di comando basato su tabelle e catene di regole (legacy).**

- **Comando:** `iptables [comando] [catena] [regole]`
- **Esempio:** `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
- **Distribuzioni:** Debian, Ubuntu, Fedora, CentOS

### `nftables`
**Successore di `iptables` con sintassi unificata e migliori performance.**

- **Comando:** `nft [comando] [regole]`
- **Esempio:** `nft add rule inet filter input tcp dport 22 accept`
- **Distribuzioni:** Debian (≥10), Ubuntu (≥20.04), Fedora, Arch

### `ufw`
**Interfaccia semplificata per gestire `iptables`/`nftables` (Uncomplicated Firewall).**

- **Comando:** `ufw [comando] [opzioni]`
- **Esempio:** `ufw enable && ufw allow ssh`
- **Distribuzioni:** Ubuntu, Debian

### `socat`
**Multipurpose relay per connessioni bidirezionali tra socket.**

- **Comando:** `socat [opzioni] indirizzo1 indirizzo2`
- **Esempio:** `socat TCP-LISTEN:8080,fork TCP:192.168.1.10:80`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `nc` (netcat)
**Legge e scrive dati attraverso connessioni di rete (TCP/UDP).**

- **Comando:** `nc [opzioni] host porta`
- **Esempio:** `nc -zv 192.168.1.1 22`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

---

## 2. System Monitoring & Performance

### `top`
**Visualizza i processi in esecuzione in tempo reale con uso di CPU/memoria.**

- **Comando:** `top [opzioni]`
- **Esempio:** `top -u www-data`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (procps-ng)

### `htop`
**Alternativa interattiva e colorata a `top` con supporto mouse.**

- **Comando:** `htop [opzioni]`
- **Esempio:** `htop -p 1234`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `btop`
**Monitoraggio di sistema moderno con interfaccia TUI avanzata (CPU, RAM, dischi, rete).**

- **Comando:** `btop`
- **Esempio:** `btop`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (pacchetti di terze parti o compilazione)

### `vmstat`
**Report di memoria virtuale, processi, CPU e I/O.**

- **Comando:** `vmstat [intervallo] [conteggio]`
- **Esempio:** `vmstat 2 5`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (procps-ng)

### `iostat`
**Statistiche CPU e I/O dei dispositivi di storage.**

- **Comando:** `iostat [opzioni] [intervallo] [conteggio]`
- **Esempio:** `iostat -x 2 3`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (sysstat)

### `mpstat`
**Report dettagliati per CPU individuale.**

- **Comando:** `mpstat [opzioni] [intervallo] [conteggio]`
- **Esempio:** `mpstat -P ALL 2`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (sysstat)

### `sar`
**Raccoglie e riporta statistiche di sistema storiche e in tempo reale.**

- **Comando:** `sar [opzioni] [intervallo] [conteggio]`
- **Esempio:** `sar -u 2 5`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (sysstat)

### `dstat`
**Sostituto versatile di `vmstat`, `iostat`, `netstat` e `ifstat`.**

- **Comando:** `dstat [opzioni]`
- **Esempio:** `dstat -c --top-cpu -n 2`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `strace`
**Traccia le system call e i segnali di un processo.**

- **Comando:** `strace [opzioni] comando`
- **Esempio:** `strace -e open,read ls /tmp`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `ltrace`
**Traccia le chiamate a librerie dinamiche di un programma.**

- **Comando:** `ltrace [opzioni] comando`
- **Esempio:** `ltrace -e malloc+free ls`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `time`
**Misura il tempo di esecuzione di un comando.**

- **Comando:** `time [opzioni] comando`
- **Esempio:** `time ls -R /usr`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `uptime`
**Mostra da quanto tempo il sistema è in esecuzione e il carico medio.**

- **Comando:** `uptime`
- **Esempio:** `uptime`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (procps-ng)

### `free`
**Mostra la quantità di memoria fisica e swap libera e usata.**

- **Comando:** `free [opzioni]`
- **Esempio:** `free -h`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (procps-ng)

### `df`
**Mostra lo spazio disponibile e usato dei filesystem montati.**

- **Comando:** `df [opzioni] [filesystem]`
- **Esempio:** `df -h`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (coreutils)

### `du`
**Stima lo spazio occupato da file e directory.**

- **Comando:** `du [opzioni] [path]`
- **Esempio:** `du -sh /var/log`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (coreutils)

---

## 3. Package Management

### `apt`
**Gestore pacchetti avanzato per Debian/Ubuntu (interfaccia utente).**

- **Comando:** `apt [comando] [pacchetto]`
- **Esempio:** `apt install nginx`
- **Distribuzioni:** Debian (≥9), Ubuntu

### `apt-get`
**Interfaccia storica di APT per la gestione dei pacchetti.**

- **Comando:** `apt-get [comando] [pacchetto]`
- **Esempio:** `apt-get update && apt-get upgrade -y`
- **Distribuzioni:** Debian, Ubuntu

### `dpkg`
**Gestore pacchetti di basso livello per file `.deb`.**

- **Comando:** `dpkg [opzioni] [azione]`
- **Esempio:** `dpkg -i pacchetto.deb`
- **Distribuzioni:** Debian, Ubuntu

### `aptitude`
**Interfaccia testuale e interattiva per APT con gestione dipendenze avanzata.**

- **Comando:** `aptitude [comando] [pacchetto]`
- **Esempio:** `aptitude search nginx`
- **Distribuzioni:** Debian, Ubuntu

### `snap`
**Sistema di pacchetti universali confinati (snap) di Canonical.**

- **Comando:** `snap [comando] [pacchetto]`
- **Esempio:** `snap install lxd`
- **Distribuzioni:** Ubuntu, Debian (con snapd)

### `flatpak`
**Sistema di distribuzione universale di applicazioni desktop con sandbox.**

- **Comando:** `flatpak [comando] [app-id]`
- **Esempio:** `flatpak install flathub org.gimp.GIMP`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `dpkg-deb`
**Strumento per creare, estrarre e ispezionare archivi `.deb`.**

- **Comando:** `dpkg-deb [opzioni] file.deb`
- **Esempio:** `dpkg-deb --contents pacchetto.deb`
- **Distribuzioni:** Debian, Ubuntu

### `add-apt-repository`
**Aggiunge repository PPA o sorgenti APT al sistema.**

- **Comando:** `add-apt-repository [repository]`
- **Esempio:** `add-apt-repository ppa:deadsnakes/ppa`
- **Distribuzioni:** Ubuntu, Debian (con software-properties-common)

---

## 4. File Management & Text Processing

### `ls`
**Elenca il contenuto di una directory.**

- **Comando:** `ls [opzioni] [path]`
- **Esempio:** `ls -lah /etc`
- **Distribuzioni:** Tutte (coreutils)

### `find`
**Cerca file e directory in base a nome, tipo, dimensione, data, ecc.**

- **Comando:** `find [path] [criteri] [azioni]`
- **Esempio:** `find /var -name "*.log" -size +10M`
- **Distribuzioni:** Tutte (findutils)

### `locate`
**Ricerca veloce di file tramite database pre-indicizzato.**

- **Comando:** `locate [opzioni] pattern`
- **Esempio:** `locate -i nginx.conf`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (mlocate / plocate)

### `grep`
**Ricerca di pattern all'interno di file o flussi di testo.**

- **Comando:** `grep [opzioni] pattern [file]`
- **Esempio:** `grep -r "error" /var/log/syslog`
- **Distribuzioni:** Tutte (grep)

### `sed`
**Editor di flusso per trasformazioni testuali non interattive.**

- **Comando:** `sed [opzioni] 'script' [file]`
- **Esempio:** `sed -i 's/old/new/g' file.txt`
- **Distribuzioni:** Tutte (sed)

### `awk`
**Linguaggio di scripting per elaborazione di testo strutturato e report.**

- **Comando:** `awk [opzioni] 'programma' [file]`
- **Esempio:** `awk '{print $1, $3}' /var/log/auth.log`
- **Distribuzioni:** Tutte (gawk / mawk)

### `cut`
**Estrae sezioni (colonne) da ogni linea di un file o input.**

- **Comando:** `cut [opzioni] [file]`
- **Esempio:** `cut -d: -f1 /etc/passwd`
- **Distribuzioni:** Tutte (coreutils)

### `sort`
**Ordina le linee di un file o input.**

- **Comando:** `sort [opzioni] [file]`
- **Esempio:** `sort -t: -k3 -n /etc/passwd`
- **Distribuzioni:** Tutte (coreutils)

### `uniq`
**Rimuove o segnala linee duplicate consecutive.**

- **Comando:** `uniq [opzioni] [input] [output]`
- **Esempio:** `sort file.txt | uniq -c`
- **Distribuzioni:** Tutte (coreutils)

### `wc`
**Conta linee, parole e caratteri di un file o input.**

- **Comando:** `wc [opzioni] [file]`
- **Esempio:** `wc -l /var/log/syslog`
- **Distribuzioni:** Tutte (coreutils)

### `tee`
**Legge da stdin e scrive simultaneamente su stdout e file.**

- **Comando:** `comando | tee [opzioni] file`
- **Esempio:** `echo "debug" | tee -a log.txt`
- **Distribuzioni:** Tutte (coreutils)

### `xargs`
**Costruisce ed esegue comandi a partire dall'input standard.**

- **Comando:** `[input] | xargs [opzioni] comando`
- **Esempio:** `find . -name "*.tmp" | xargs rm -f`
- **Distribuzioni:** Tutte (findutils)

### `rsync`
**Sincronizzazione veloce e incrementale di file locali e remoti.**

- **Comando:** `rsync [opzioni] sorgente destinazione`
- **Esempio:** `rsync -avz /dati/ user@host:/backup/`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `tar`
**Archiviatore multi-formato (tar, gz, bz2, xz).**

- **Comando:** `tar [opzioni] [archivio] [file]`
- **Esempio:** `tar -czvf backup.tar.gz /home/user`
- **Distribuzioni:** Tutte (tar)

### `gzip` / `gunzip`
**Compressione/decompressione con algoritmo DEFLATE.**

- **Comando:** `gzip [opzioni] [file]`
- **Esempio:** `gzip -9 file.iso`
- **Distribuzioni:** Tutte (gzip)

### `bzip2` / `bunzip2`
**Compressione/decompressione con algoritmo Burrows-Wheeler (migliore ratio).**

- **Comando:** `bzip2 [opzioni] [file]`
- **Esempio:** `bzip2 -k file.tar`
- **Distribuzioni:** Tutte (bzip2)

### `xz`
**Compressione/decompressione ad alto rapporto (LZMA).**

- **Comando:** `xz [opzioni] [file]`
- **Esempio:** `xz -z -9 file.tar`
- **Distribuzioni:** Tutte (xz-utils)

### `zip` / `unzip`
**Pacchettizzazione e compressione formato ZIP.**

- **Comando:** `zip [opzioni] archivio.zip [file]`
- **Esempio:** `zip -r archivio.zip directory/`
- **Distribuzioni:** Tutte (zip / unzip)

### `chmod`
**Modifica i permessi di file e directory.**

- **Comando:** `chmod [opzioni] permessi file`
- **Esempio:** `chmod 755 script.sh`
- **Distribuzioni:** Tutte (coreutils)

### `chown`
**Modifica proprietario e gruppo di file e directory.**

- **Comando:** `chown [opzioni] utente:gruppo file`
- **Esempio:** `chown -R www-data:www-data /var/www`
- **Distribuzioni:** Tutte (coreutils)

### `ln`
**Crea collegamenti fisici (hard link) o simbolici (symlink).**

- **Comando:** `ln [opzioni] target link_name`
- **Esempio:** `ln -s /usr/bin/python3 /usr/local/bin/python`
- **Distribuzioni:** Tutte (coreutils)

---

## 5. Process Management

### `ps`
**Fornisce un'istantanea dei processi correnti.**

- **Comando:** `ps [opzioni]`
- **Esempio:** `ps aux --sort=-%mem`
- **Distribuzioni:** Tutte (procps-ng)

### `kill`
**Invia un segnale a un processo tramite PID.**

- **Comando:** `kill [opzioni] PID`
- **Esempio:** `kill -9 1234`
- **Distribuzioni:** Tutte (util-linux / coreutils)

### `pkill`
**Invia un segnale a uno o più processi per nome.**

- **Comando:** `pkill [opzioni] pattern`
- **Esempio:** `pkill -f "python server.py"`
- **Distribuzioni:** Tutte (procps-ng)

### `pgrep`
**Cerca processi per nome o pattern e ne restituisce i PID.**

- **Comando:** `pgrep [opzioni] pattern`
- **Esempio:** `pgrep -u www-data nginx`
- **Distribuzioni:** Tutte (procps-ng)

### `nice` / `renice`
**Avvia o modifica la priorità di scheduling di un processo.**

- **Comando:** `nice -n [valore] comando`
- **Esempio:** `nice -n 19 backup.sh`
- **Distribuzioni:** Tutte (util-linux / coreutils)

### `nohup`
**Esegue un comando ignorando il segnale SIGHUP (persiste dopo logout).**

- **Comando:** `nohup comando [arg] &`
- **Esempio:** `nohup python server.py > output.log 2>&1 &`
- **Distribuzioni:** Tutte (coreutils)

### `systemctl`
**Controlla il sistema di init systemd e lo stato dei servizi.**

- **Comando:** `systemctl [comando] [unità]`
- **Esempio:** `systemctl status nginx`
- **Distribuzioni:** Debian (≥8), Ubuntu (≥15.04), Fedora, Arch

### `journalctl`
**Interroga i log di systemd-journald.**

- **Comando:** `journalctl [opzioni]`
- **Esempio:** `journalctl -u nginx --since "1 hour ago"`
- **Distribuzioni:** Debian (≥8), Ubuntu (≥15.04), Fedora, Arch (systemd)

---

## 6. Disk & Storage

### `fdisk`
**Manipola la tabella delle partizioni su un disco (MBR e GPT).**

- **Comando:** `fdisk [opzioni] /dev/sdX`
- **Esempio:** `fdisk -l`
- **Distribuzioni:** Tutte (util-linux)

### `parted`
**Tool avanzato per la gestione delle partizioni (supporta GPT nativamente).**

- **Comando:** `parted [opzioni] /dev/sdX [comando]`
- **Esempio:** `parted /dev/sda mklabel gpt`
- **Distribuzioni:** Tutte (parted)

### `lsblk`
**Elenca i dispositivi a blocchi in output ad albero.**

- **Comando:** `lsblk [opzioni]`
- **Esempio:** `lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT`
- **Distribuzioni:** Tutte (util-linux)

### `blkid`
**Mostra attributi (UUID, LABEL, tipo FS) dei dispositivi a blocchi.**

- **Comando:** `blkid [opzioni] [dispositivo]`
- **Esempio:** `blkid /dev/sda1`
- **Distribuzioni:** Tutte (util-linux)

### `mount`
**Monta un filesystem su una directory specificata.**

- **Comando:** `mount [opzioni] dispositivo directory`
- **Esempio:** `mount /dev/sdb1 /mnt/usb`
- **Distribuzioni:** Tutte (util-linux)

### `umount`
**Smonta un filesystem montato.**

- **Comando:** `umount [opzioni] dispositivo|directory`
- **Esempio:** `umount /mnt/usb`
- **Distribuzioni:** Tutte (util-linux)

### `mkfs`
**Crea un filesystem su un dispositivo (wrapper per mkfs.\*).**

- **Comando:** `mkfs -t [tipo] [opzioni] dispositivo`
- **Esempio:** `mkfs -t ext4 /dev/sdb1`
- **Distribuzioni:** Tutte (util-linux / e2fsprogs)

### `fsck`
**Controlla e ripara un filesystem.**

- **Comando:** `fsck [opzioni] dispositivo`
- **Esempio:** `fsck -f /dev/sda1`
- **Distribuzioni:** Tutte (util-linux / e2fsprogs)

### `dd`
**Copia/converti file a basso livello (clonazione dischi, backup raw).**

- **Comando:** `dd [opzioni] if=origine of=destinazione`
- **Esempio:** `dd if=/dev/sda of=/backup.img bs=4M status=progress`
- **Distribuzioni:** Tutte (coreutils)

### `smartctl`
**Controlla e monitora lo stato S.M.A.R.T. dei dischi.**

- **Comando:** `smartctl [opzioni] dispositivo`
- **Esempio:** `smartctl -H /dev/sda`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (smartmontools)

### `pvcreate` / `vgcreate` / `lvcreate`
**Gestione volumi logici LVM (Physical Volume, Volume Group, Logical Volume).**

- **Comando:** `pvcreate /dev/sdb1 && vgcreate vg_data /dev/sdb1 && lvcreate -L 10G -n lv_data vg_data`
- **Esempio:** `lvcreate -L 50G -n lv_home vg_system`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (lvm2)

---

## 7. System Information

### `uname`
**Mostra informazioni sul kernel e sull'architettura del sistema.**

- **Comando:** `uname [opzioni]`
- **Esempio:** `uname -a`
- **Distribuzioni:** Tutte (coreutils)

### `lscpu`
**Mostra dettagli sull'architettura della CPU.**

- **Comando:** `lscpu`
- **Esempio:** `lscpu | grep "Model name"`
- **Distribuzioni:** Tutte (util-linux)

### `lsusb`
**Elenca i dispositivi USB collegati al sistema.**

- **Comando:** `lsusb [opzioni]`
- **Esempio:** `lsusb -v`
- **Distribuzioni:** Tutte (usbutils)

### `lspci`
**Elenca i dispositivi PCI collegati al sistema.**

- **Comando:** `lspci [opzioni]`
- **Esempio:** `lspci -nnk | grep -A2 "VGA"`
- **Distribuzioni:** Tutte (pciutils)

### `lsmod`
**Mostra i moduli del kernel attualmente caricati.**

- **Comando:** `lsmod`
- **Esempio:** `lsmod | grep nvidia`
- **Distribuzioni:** Tutte (kmod)

### `dmesg`
**Stampa o controlla il ring buffer dei messaggi del kernel.**

- **Comando:** `dmesg [opzioni]`
- **Esempio:** `dmesg -T | tail -20`
- **Distribuzioni:** Tutte (util-linux)

### `dmidecode`
**Legge le tabelle DMI/SMBIOS per informazioni hardware (BIOS, chassis, RAM, CPU).**

- **Comando:** `dmidecode [opzioni]`
- **Esempio:** `dmidecode -t memory`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `lshw`
**Estrae informazioni hardware dettagliate (CPU, RAM, rete, storage).**

- **Comando:** `lshw [opzioni]`
- **Esempio:** `lshw -class network -short`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `hostnamectl`
**Gestisce il nome host del sistema (systemd).**

- **Comando:** `hostnamectl [comando] [nome]`
- **Esempio:** `hostnamectl set-hostname server01`
- **Distribuzioni:** Debian (≥8), Ubuntu (≥15.04), Fedora, Arch (systemd)

### `timedatectl`
**Gestisce data, ora e fuso orario del sistema (systemd).**

- **Comando:** `timedatectl [comando]`
- **Esempio:** `timedatectl set-timezone Europe/Rome`
- **Distribuzioni:** Debian (≥8), Ubuntu (≥15.04), Fedora, Arch (systemd)

---

## 8. Security

### `openssl`
**Toolkit per crittografia, certificati SSL/TLS, hashing e generazione chiavi.**

- **Comando:** `openssl [comando] [opzioni]`
- **Esempio:** `openssl req -new -x509 -days 365 -nodes -out cert.pem -keyout key.pem`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `gpg` (GnuPG)
**Cifratura, decifratura e firma di file/email con crittografia asimmetrica.**

- **Comando:** `gpg [opzioni] [file]`
- **Esempio:** `gpg --encrypt --recipient user@example.com file.txt`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `ssh`
**Client e server per connessioni remote sicure (Secure Shell).**

- **Comando:** `ssh [opzioni] user@host`
- **Esempio:** `ssh -i ~/.ssh/id_rsa admin@192.168.1.100`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (openssh-client)

### `scp`
**Copia file tra host locali e remoti tramite SSH.**

- **Comando:** `scp [opzioni] sorgente destinazione`
- **Esempio:** `scp file.txt user@host:/home/user/`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (openssh-client)

### `ssh-keygen`
**Genera, gestisce e converte chiavi di autenticazione SSH.**

- **Comando:** `ssh-keygen [opzioni]`
- **Esempio:** `ssh-keygen -t ed25519 -C "commento"`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (openssh-client)

### `fail2ban`
**Ban IP malevoli basandosi su log di applicazioni (SSH, Apache, ecc.).**

- **Comando:** `fail2ban-client [comando]`
- **Esempio:** `fail2ban-client status sshd`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `rkhunter`
**Scanner per rootkit, backdoor e vulnerabilità locali.**

- **Comando:** `rkhunter [opzioni]`
- **Esempio:** `rkhunter --check --skip-keypress`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `lynis`
**Strumento di audit di sicurezza per sistemi Unix/Linux.**

- **Comando:** `lynis [comando] [opzioni]`
- **Esempio:** `lynis audit system`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `apparmor`
**Sistema MAC (Mandatory Access Control) basato su profili.**

- **Comando:** `aa-status / aa-enforce / aa-complain`
- **Esempio:** `aa-status`
- **Distribuzioni:** Debian, Ubuntu, SUSE

### `selinux`
**Sistema MAC con policy flessibili e contesti di sicurezza (labels).**

- **Comando:** `sestatus / getenforce / setenforce`
- **Esempio:** `sestatus`
- **Distribuzioni:** Fedora, CentOS, RHEL, Debian/Ubuntu (parziale)

---

## 9. User & Group Management

### `useradd`
**Crea un nuovo utente con parametri base.**

- **Comando:** `useradd [opzioni] nome_utente`
- **Esempio:** `useradd -m -s /bin/bash -G sudo mario`
- **Distribuzioni:** Tutte (shadow-utils / adduser)

### `usermod`
**Modifica i parametri di un account utente esistente.**

- **Comando:** `usermod [opzioni] nome_utente`
- **Esempio:** `usermod -aG docker mario`
- **Distribuzioni:** Tutte (shadow-utils)

### `userdel`
**Rimuove un account utente dal sistema.**

- **Comando:** `userdel [opzioni] nome_utente`
- **Esempio:** `userdel -r mario`
- **Distribuzioni:** Tutte (shadow-utils)

### `groupadd`
**Crea un nuovo gruppo.**

- **Comando:** `groupadd [opzioni] nome_gruppo`
- **Esempio:** `groupadd developers`
- **Distribuzioni:** Tutte (shadow-utils)

### `groupmod`
**Modifica il nome o il GID di un gruppo esistente.**

- **Comando:** `groupmod [opzioni] nome_gruppo`
- **Esempio:** `groupmod -n devs developers`
- **Distribuzioni:** Tutte (shadow-utils)

### `groupdel`
**Rimuove un gruppo dal sistema.**

- **Comando:** `groupdel nome_gruppo`
- **Esempio:** `groupdel oldteam`
- **Distribuzioni:** Tutte (shadow-utils)

### `passwd`
**Imposta o modifica la password di un utente.**

- **Comando:** `passwd [opzioni] [utente]`
- **Esempio:** `passwd mario`
- **Distribuzioni:** Tutte (shadow-utils)

### `chage`
**Gestisce le informazioni di scadenza della password per un utente.**

- **Comando:** `chage [opzioni] utente`
- **Esempio:** `chage -M 90 mario`
- **Distribuzioni:** Tutte (shadow-utils)

### `id`
**Mostra l'UID, il GID e i gruppi aggiuntivi di un utente.**

- **Comando:** `id [opzioni] [utente]`
- **Esempio:** `id www-data`
- **Distribuzioni:** Tutte (coreutils)

### `who` / `w`
**Mostra gli utenti attualmente connessi al sistema.**

- **Comando:** `who [opzioni]` / `w [opzioni]`
- **Esempio:** `who -a`
- **Distribuzioni:** Tutte (coreutils / procps-ng)

### `last`
**Mostra la cronologia degli ultimi accessi (log di `wtmp`).**

- **Comando:** `last [opzioni] [utente]`
- **Esempio:** `last -10 mario`
- **Distribuzioni:** Tutte (util-linux)

### `loginctl`
**Controlla le sessioni utente tramite systemd-logind.**

- **Comando:** `loginctl [comando]`
- **Esempio:** `loginctl list-sessions`
- **Distribuzioni:** Debian (≥8), Ubuntu (≥15.04), Fedora, Arch (systemd)

---

## 10. Task Scheduling & Automation

### `cron` / `crontab`
**Esecuzione periodica di comandi a intervalli definiti.**

- **Comando:** `crontab [opzioni]`
- **Esempio:** `crontab -e` (aggiungere `0 3 * * * /usr/bin/backup.sh`)
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch (cronie / cron)

### `at`
**Esecuzione di un comando una tantum in un momento specifico.**

- **Comando:** `at [orario]`
- **Esempio:** `echo "shutdown -h now" | at 23:00`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `systemd-timer`
**Alternativa moderna a cron basata su unità systemd (timer + service).**

- **Comando:** `systemctl [comando] timer_name.timer`
- **Esempio:** `systemctl enable --now backup.timer`
- **Distribuzioni:** Debian (≥8), Ubuntu (≥15.04), Fedora, Arch (systemd)

### `anacron`
**Esecuzione di comandi periodici per sistemi non sempre accesi.**

- **Comando:** `anacron [opzioni] [comando]`
- **Esempio:** `anacron -u`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

---

## 11. Shell & Terminal Utilities

### `bash`
**Bourne Again SHell — shell predefinita sulla maggior parte dei sistemi Linux.**

- **Comando:** `bash [opzioni] [script]`
- **Esempio:** `bash -x script.sh`
- **Distribuzioni:** Tutte

### `zsh`
**Shell interattiva avanzata con potenti funzionalità di completamento e theming.**

- **Comando:** `zsh [opzioni]`
- **Esempio:** `zsh -c "echo test"`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `tmux`
**Terminal multiplexer — gestisce più sessioni, finestre e riquadri.**

- **Comando:** `tmux [comando] [opzioni]`
- **Esempio:** `tmux new -s mysession`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `screen`
**Terminal multiplexer classico — mantiene sessioni attive anche dopo disconnessione.**

- **Comando:** `screen [opzioni] [comando]`
- **Esempio:** `screen -S mysession`
- **Distribuzioni:** Debian, Ubuntu, Fedora, Arch

### `alias`
**Crea abbreviazioni personalizzate per comandi lunghi (shell builtin).**

- **Comando:** `alias nome='comando'`
- **Esempio:** `alias ll='ls -lah'`
- **Distribuzioni:** Tutte (shell builtin)

### `history`
**Mostra la cronologia dei comandi eseguiti nella shell corrente.**

- **Comando:** `history [opzioni]`
- **Esempio:** `history | grep ssh`
- **Distribuzioni:** Tutte (shell builtin)

### `which`
**Mostra il percorso assoluto di un eseguibile.**

- **Comando:** `which [opzioni] comando`
- **Esempio:** `which python3`
- **Distribuzioni:** Tutte (debianutils / which)

### `whereis`
**Cerca il binario, i sorgenti e le pagine man di un comando.**

- **Comando:** `whereis [opzioni] comando`
- **Esempio:** `whereis ls`
- **Distribuzioni:** Tutte (util-linux)

---

## Licenza

Questo documento è distribuito con licenza MIT. Libero da usare, modificare e distribuire.
