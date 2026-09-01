# Terraform Tools — Base, Intermedio ed Esperto

Ogni voce segue il formato **Comando**, **Spiegazione** ed **Esempio**.
Sostituire i valori tra parentesi angolari con quelli dell'ambiente reale.

## Base

### `terraform version`
**Comando:** `terraform version`
**Spiegazione:** Mostra la versione di Terraform e del provider in uso.
**Esempio:** `terraform version`

### `terraform help`
**Comando:** `terraform -help` oppure `terraform COMANDO -help`
**Spiegazione:** Mostra l'aiuto generale o le opzioni di un comando.
**Esempio:** `terraform plan -help`

### `terraform init`
**Comando:** `terraform init`
**Spiegazione:** Inizializza la directory, scarica provider e prepara il backend.
**Esempio:** `terraform init`

### `terraform init -upgrade`
**Comando:** `terraform init -upgrade`
**Spiegazione:** Aggiorna provider e moduli rispettando i vincoli configurati.
**Esempio:** `terraform init -upgrade`

### `terraform fmt`
**Comando:** `terraform fmt [-recursive]`
**Spiegazione:** Applica la formattazione standard ai file `.tf`.
**Esempio:** `terraform fmt -recursive`

### `terraform validate`
**Comando:** `terraform validate`
**Spiegazione:** Controlla sintassi e coerenza interna della configurazione.
**Esempio:** `terraform validate`

### `terraform providers`
**Comando:** `terraform providers`
**Spiegazione:** Mostra provider richiesti dalla configurazione e dallo stato.
**Esempio:** `terraform providers`

### `terraform plan`
**Comando:** `terraform plan [--out=FILE]`
**Spiegazione:** Calcola le modifiche senza applicarle; è il controllo principale prima di un deploy.
**Esempio:** `terraform plan -out=tfplan`

### `terraform show`
**Comando:** `terraform show [FILE]`
**Spiegazione:** Mostra in forma leggibile un piano o lo stato Terraform.
**Esempio:** `terraform show tfplan`

### `terraform apply`
**Comando:** `terraform apply [FILE]`
**Spiegazione:** Applica le modifiche approvate per creare, aggiornare o eliminare risorse.
**Esempio:** `terraform apply tfplan`

### `terraform output`
**Comando:** `terraform output [-raw] [NOME]`
**Spiegazione:** Mostra i valori dichiarati nel blocco `output`.
**Esempio:** `terraform output -raw load_balancer_dns`

### `terraform console`
**Comando:** `terraform console`
**Spiegazione:** Apre una console interattiva per provare espressioni HCL e variabili.
**Esempio:** `terraform console`

### `terraform get`
**Comando:** `terraform get [-update]`
**Spiegazione:** Scarica i moduli dichiarati; `-update` richiede versioni aggiornate.
**Esempio:** `terraform get -update`

### `terraform workspace list`
**Comando:** `terraform workspace list`
**Spiegazione:** Elenca gli workspace disponibili nella configurazione corrente.
**Esempio:** `terraform workspace list`

### `terraform workspace show`
**Comando:** `terraform workspace show`
**Spiegazione:** Mostra lo workspace attualmente selezionato.
**Esempio:** `terraform workspace show`

### `terraform workspace new`
**Comando:** `terraform workspace new NOME`
**Spiegazione:** Crea e seleziona un nuovo workspace per uno stato separato.
**Esempio:** `terraform workspace new staging`

### `terraform workspace select`
**Comando:** `terraform workspace select NOME`
**Spiegazione:** Seleziona uno workspace già esistente.
**Esempio:** `terraform workspace select production`

### `terraform workspace delete`
**Comando:** `terraform workspace delete NOME`
**Spiegazione:** Elimina uno workspace; verificare prima che il suo stato non serva più.
**Esempio:** `terraform workspace delete test`

## Intermedio

### `terraform plan` con variabili
**Comando:** `terraform plan -var='CHIAVE=VALORE' -var-file=FILE.tfvars`
**Spiegazione:** Genera un piano usando valori di input diversi dall'impostazione predefinita.
**Esempio:** `terraform plan -var-file=production.tfvars`

### `terraform apply` con approvazione automatica
**Comando:** `terraform apply -auto-approve`
**Spiegazione:** Applica senza chiedere conferma interattiva; usarlo solo in pipeline controllate.
**Esempio:** `terraform apply -auto-approve tfplan`

### `terraform destroy`
**Comando:** `terraform destroy [--target=ADDRESS]`
**Spiegazione:** Elimina le risorse gestite dallo stato; è un comando distruttivo.
**Esempio:** `terraform plan -destroy`

### `terraform taint` e `untaint`
**Comando:** `terraform taint ADDRESS`; `terraform untaint ADDRESS`
**Spiegazione:** Segnalano o rimuovono la segnalazione che una risorsa deve essere ricreata; oggi preferire `-replace`.
**Esempio:** `terraform plan -replace='aws_instance.web'`

### `terraform apply -replace`
**Comando:** `terraform apply -replace='ADDRESS'`
**Spiegazione:** Forza la sostituzione controllata di una risorsa durante l'apply.
**Esempio:** `terraform apply -replace='aws_instance.web'`

### `terraform import`
**Comando:** `terraform import ADDRESS ID`
**Spiegazione:** Collega una risorsa cloud già esistente a una risorsa Terraform.
**Esempio:** `terraform import 'aws_instance.web' i-0123456789abcdef0`

### `terraform plan -refresh-only`
**Comando:** `terraform plan -refresh-only`
**Spiegazione:** Aggiorna la fotografia dello stato senza proporre modifiche infrastrutturali.
**Esempio:** `terraform plan -refresh-only`

### `terraform refresh`
**Comando:** `terraform refresh`
**Spiegazione:** Aggiorna lo stato leggendo l'infrastruttura; nelle versioni moderne preferire `plan -refresh-only`.
**Esempio:** `terraform plan -refresh-only -out=refresh.tfplan`

### `terraform graph`
**Comando:** `terraform graph [-type=plan]`
**Spiegazione:** Produce il grafo delle dipendenze tra risorse.
**Esempio:** `terraform graph -type=plan | dot -Tpng > graph.png`

### `terraform state list`
**Comando:** `terraform state list [PATTERN]`
**Spiegazione:** Elenca gli indirizzi delle risorse presenti nello stato.
**Esempio:** `terraform state list 'module.network.*'`

### `terraform state show`
**Comando:** `terraform state show ADDRESS`
**Spiegazione:** Mostra attributi e metadati di una risorsa nello stato.
**Esempio:** `terraform state show 'aws_instance.web'`

### `terraform state mv`
**Comando:** `terraform state mv SOURCE DESTINATION`
**Spiegazione:** Sposta un indirizzo nello stato senza ricreare la risorsa reale.
**Esempio:** `terraform state mv 'aws_instance.old' 'aws_instance.web'`

### `terraform state rm`
**Comando:** `terraform state rm ADDRESS`
**Spiegazione:** Rimuove una risorsa dallo stato senza eliminarla dal cloud.
**Esempio:** `terraform state rm 'aws_instance.legacy'`

### `terraform state pull`
**Comando:** `terraform state pull > state-backup.json`
**Spiegazione:** Scarica lo stato corrente; proteggerlo perché può contenere dati sensibili.
**Esempio:** `terraform state pull > state-backup.json`

### `terraform state push`
**Comando:** `terraform state push STATEFILE`
**Spiegazione:** Carica uno stato locale nel backend; usare solo con procedura approvata.
**Esempio:** `terraform state push recovered-state.json`

### `terraform providers lock`
**Comando:** `terraform providers lock`
**Spiegazione:** Aggiorna i checksum dei provider nel file `.terraform.lock.hcl`.
**Esempio:** `terraform providers lock -platform=linux_amd64 -platform=windows_amd64`

### `terraform fmt` e `validate` in CI
**Comando:** `terraform fmt -check -diff; terraform validate -no-color`
**Spiegazione:** Falliscono la pipeline se il codice non è formattato o valido.
**Esempio:** `terraform fmt -check -recursive; terraform validate -no-color`

### `terraform force-unlock`
**Comando:** `terraform force-unlock LOCK_ID`
**Spiegazione:** Rimuove un lock rimasto bloccato dopo un crash; non usarlo se un altro apply è attivo.
**Esempio:** `terraform force-unlock <lock-id>`

## Esperto

### `terraform init -backend-config`
**Comando:** `terraform init -backend-config=FILE`
**Spiegazione:** Configura parametri di un backend remoto separatamente dal codice.
**Esempio:** `terraform init -backend-config=backend-production.hcl`

### `terraform init -migrate-state`
**Comando:** `terraform init -migrate-state`
**Spiegazione:** Migra lo stato quando cambia configurazione o tipo di backend.
**Esempio:** `terraform init -migrate-state`

### `terraform init -reconfigure`
**Comando:** `terraform init -reconfigure`
**Spiegazione:** Ignora la configurazione backend salvata localmente e la ricrea.
**Esempio:** `terraform init -reconfigure -backend-config=backend.hcl`

### `terraform plan -detailed-exitcode`
**Comando:** `terraform plan -detailed-exitcode`
**Spiegazione:** Restituisce codice 0 senza modifiche, 2 con modifiche e 1 con errore: utile in CI.
**Esempio:** `terraform plan -out=tfplan -detailed-exitcode`

### `terraform apply` da piano firmato
**Comando:** `terraform apply FILE`
**Spiegazione:** Applica esattamente il piano salvato, evitando di ricalcolarlo interattivamente.
**Esempio:** `terraform plan -out=tfplan; sha256sum tfplan; terraform apply tfplan`

### `terraform state replace-provider`
**Comando:** `terraform state replace-provider OLD NEW`
**Spiegazione:** Sostituisce il source address di un provider nello stato.
**Esempio:** `terraform state replace-provider registry.terraform.io/-/aws registry.terraform.io/hashicorp/aws`

### `terraform workspace`
**Comando:** `terraform workspace new -state=FILE NOME`
**Spiegazione:** Crea uno workspace importando uno stato esistente.
**Esempio:** `terraform workspace new -state=legacy.tfstate migration`

### `terraform test`
**Comando:** `terraform test [DIRECTORY]`
**Spiegazione:** Esegue test Terraform definiti in file `.tftest.hcl`.
**Esempio:** `terraform test tests/`

### `terraform providers schema`
**Comando:** `terraform providers schema -json > schemas.json`
**Spiegazione:** Esporta gli schemi dei provider per analisi e tooling automatico.
**Esempio:** `terraform providers schema -json > schemas.json`

### `terraform metadata functions`
**Comando:** `terraform metadata functions -json`
**Spiegazione:** Mostra metadati delle funzioni disponibili nel linguaggio Terraform.
**Esempio:** `terraform metadata functions -json`

### `terraform version -json`
**Comando:** `terraform version -json`
**Spiegazione:** Restituisce versioni in JSON, utile per script e pipeline.
**Esempio:** `terraform version -json | jq '.terraform_version'`

### Variabili d'ambiente Terraform
**Comando:** `TF_VAR_nome=valore terraform plan`; `TF_CLI_ARGS_plan='-no-color' terraform plan`
**Spiegazione:** Passa variabili e opzioni senza inserirle direttamente nel comando o nei file.
**Esempio:** `TF_VAR_environment=production terraform plan`

### Log di debug
**Comando:** `TF_LOG=INFO terraform plan`; `TF_LOG_PATH=terraform.log terraform apply`
**Spiegazione:** Aumenta il dettaglio dei log e può salvarli in un file; proteggere eventuali dati sensibili.
**Esempio:** `TF_LOG=TRACE TF_LOG_PATH=terraform.log terraform plan`

### `terraform providers mirror`
**Comando:** `terraform providers mirror DIRECTORY`
**Spiegazione:** Crea un mirror locale dei provider per ambienti isolati o senza accesso Internet.
**Esempio:** `terraform providers mirror ./provider-mirror`

---

## Regole operative

- Eseguire sempre `fmt`, `validate`, `plan` e revisione del piano prima di `apply`.
- Usare backend remoti con locking, versioning, cifratura e accesso limitato.
- Non committare file `.tfstate`, credenziali, token o piani contenenti dati sensibili.
- Evitare `-target` come procedura ordinaria: usarlo solo per recuperi o casi documentati.
- Per `destroy`, `state push`, `state rm` e `force-unlock` conservare approvazione e rollback.
