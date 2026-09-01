# Git Tools — Base, Intermedio ed Esperto

Ogni voce segue il formato **Comando**, **Spiegazione** ed **Esempio**.

## Base

### `git --version`
**Comando:** `git --version`
**Spiegazione:** Mostra la versione di Git installata.
**Esempio:** `git --version`

### `git help`
**Comando:** `git help COMANDO`
**Spiegazione:** Apre la documentazione di un comando.
**Esempio:** `git help commit`

### `git config`
**Comando:** `git config --global user.name NOME; git config --global user.email EMAIL`
**Spiegazione:** Configura nome ed email usati nei commit.
**Esempio:** `git config --global user.name "Mario Rossi"`

### `git init`
**Comando:** `git init [DIRECTORY]`
**Spiegazione:** Crea una nuova repository Git.
**Esempio:** `git init progetto`

### `git clone`
**Comando:** `git clone URL [DIRECTORY]`
**Spiegazione:** Copia una repository remota e la sua cronologia sul computer.
**Esempio:** `git clone https://github.com/org/progetto.git`

### `git status`
**Comando:** `git status [--short] [--branch]`
**Spiegazione:** Mostra branch corrente, modifiche staged, non staged e file non tracciati.
**Esempio:** `git status --short --branch`

### `git add`
**Comando:** `git add FILE|DIRECTORY`
**Spiegazione:** Mette modifiche nell'area di staging, pronte per il commit.
**Esempio:** `git add src/ README.md`

### `git add --patch`
**Comando:** `git add --patch [FILE]`
**Spiegazione:** Fa scegliere quali singole parti di una modifica mettere nello staging.
**Esempio:** `git add --patch app.py`

### `git commit`
**Comando:** `git commit -m "MESSAGGIO"`
**Spiegazione:** Salva nello storico le modifiche presenti nello staging.
**Esempio:** `git commit -m "feat: add health check"`

### `git commit --amend`
**Comando:** `git commit --amend [--no-edit]`
**Spiegazione:** Corregge l'ultimo commit, modifiche o messaggio.
**Esempio:** `git add README.md; git commit --amend --no-edit`

### `git log`
**Comando:** `git log [--oneline] [--graph] [--all]`
**Spiegazione:** Mostra la cronologia dei commit.
**Esempio:** `git log --oneline --graph --decorate --all`

### `git show`
**Comando:** `git show COMMIT`
**Spiegazione:** Mostra dettagli e modifiche di un commit, tag o altro riferimento.
**Esempio:** `git show --stat HEAD`

### `git diff`
**Comando:** `git diff [COMMIT_A COMMIT_B] [-- FILE]`
**Spiegazione:** Confronta modifiche locali, staged o tra due commit.
**Esempio:** `git diff --cached -- README.md`

### `git restore`
**Comando:** `git restore FILE`
**Spiegazione:** Ripristina un file all'ultimo commit, eliminando le sue modifiche locali.
**Esempio:** `git restore -- README.md`

### `git restore --staged`
**Comando:** `git restore --staged FILE`
**Spiegazione:** Toglie un file dallo staging senza cancellare il lavoro locale.
**Esempio:** `git restore --staged app.py`

### `git rm`
**Comando:** `git rm FILE`
**Spiegazione:** Elimina un file dal disco e prepara la cancellazione per il commit.
**Esempio:** `git rm old-config.yml`

### `git mv`
**Comando:** `git mv VECCHIO NUOVO`
**Spiegazione:** Sposta o rinomina un file registrando l'operazione.
**Esempio:** `git mv README.txt README.md`

### `git clean`
**Comando:** `git clean -n [-d]`; `git clean -f [-d]`
**Spiegazione:** Prima mostra e poi può eliminare file non tracciati; usare sempre `-n` prima.
**Esempio:** `git clean -nd`

### `git branch`
**Comando:** `git branch [NOME]`
**Spiegazione:** Elenca o crea branch locali senza cambiare branch.
**Esempio:** `git branch feature/login`

### `git switch`
**Comando:** `git switch NOME_BRANCH`
**Spiegazione:** Cambia branch e aggiorna i file locali.
**Esempio:** `git switch feature/login`

### `git switch -c`
**Comando:** `git switch -c NOME_BRANCH`
**Spiegazione:** Crea e seleziona subito un nuovo branch.
**Esempio:** `git switch -c feature/login`

### `git remote`
**Comando:** `git remote -v|add|remove|rename`
**Spiegazione:** Visualizza e gestisce gli indirizzi delle repository remote.
**Esempio:** `git remote add origin https://github.com/org/progetto.git`

### `git fetch`
**Comando:** `git fetch REMOTE [BRANCH]`
**Spiegazione:** Scarica aggiornamenti remoti senza modificare il branch corrente.
**Esempio:** `git fetch origin --prune`

### `git pull`
**Comando:** `git pull [--rebase] REMOTE BRANCH`
**Spiegazione:** Scarica aggiornamenti e li integra nel branch corrente.
**Esempio:** `git pull --rebase origin main`

### `git push`
**Comando:** `git push [-u] REMOTE BRANCH`
**Spiegazione:** Pubblica commit locali sul remoto; `-u` imposta il branch upstream.
**Esempio:** `git push -u origin main`

## Intermedio

### `git branch -d` e `-D`
**Comando:** `git branch -d BRANCH`; `git branch -D BRANCH`
**Spiegazione:** Eliminano un branch locale; `-D` forza anche se non integrato.
**Esempio:** `git branch -d feature/login`

### `git branch -r` e `-a`
**Comando:** `git branch -r`; `git branch -a`
**Spiegazione:** Mostrano branch remoti oppure locali e remoti.
**Esempio:** `git branch -a`

### `git branch -m`
**Comando:** `git branch -m VECCHIO NUOVO`
**Spiegazione:** Rinomina un branch locale.
**Esempio:** `git branch -m master main`

### `git push --delete`
**Comando:** `git push REMOTE --delete BRANCH`
**Spiegazione:** Elimina un branch dal repository remoto.
**Esempio:** `git push origin --delete feature/obsolete`

### `git merge`
**Comando:** `git merge [--no-ff] BRANCH`
**Spiegazione:** Integra nella branch corrente la storia del branch indicato.
**Esempio:** `git switch main; git merge --no-ff feature/login`

### `git merge --abort`
**Comando:** `git merge --abort`
**Spiegazione:** Annulla un merge in conflitto e torna allo stato precedente.
**Esempio:** `git merge feature/login; git merge --abort`

### `git rebase`
**Comando:** `git rebase BASE`
**Spiegazione:** Sposta i commit sopra una nuova base, riscrivendo la storia locale.
**Esempio:** `git rebase origin/main`

### `git rebase -i`
**Comando:** `git rebase -i HEAD~N`
**Spiegazione:** Permette di riordinare, unire, modificare o eliminare commit recenti.
**Esempio:** `git rebase -i HEAD~3`

### `git rebase --continue` e `--abort`
**Comando:** `git rebase --continue`; `git rebase --abort`
**Spiegazione:** Continuano il rebase dopo i conflitti oppure lo annullano.
**Esempio:** `git add app.py; git rebase --continue`

### `git cherry-pick`
**Comando:** `git cherry-pick COMMIT`
**Spiegazione:** Applica su un branch le modifiche di uno specifico commit.
**Esempio:** `git cherry-pick a1b2c3d`

### `git revert`
**Comando:** `git revert COMMIT`
**Spiegazione:** Crea un nuovo commit che annulla un commit precedente senza riscrivere la storia.
**Esempio:** `git revert a1b2c3d`

### `git reset`
**Comando:** `git reset --soft|--mixed|--hard COMMIT`
**Spiegazione:** Sposta HEAD; `soft` conserva staging, `mixed` conserva file modificati, `hard` li elimina.
**Esempio:** `git reset --soft HEAD~1`

### `git stash`
**Comando:** `git stash push -u -m "MESSAGGIO"`
**Spiegazione:** Mette temporaneamente da parte modifiche, anche file non tracciati con `-u`.
**Esempio:** `git stash push -u -m "lavoro temporaneo"`

### `git stash list/show/apply/pop/drop`
**Comando:** `git stash list|show|apply|pop|drop STASH`
**Spiegazione:** Elenca, analizza, recupera o elimina modifiche messe da parte.
**Esempio:** `git stash list; git stash pop stash@{0}`

### `git tag`
**Comando:** `git tag [-a] TAG [-m "MESSAGGIO"]`
**Spiegazione:** Crea o elenca riferimenti stabili, spesso usati per release.
**Esempio:** `git tag -a v1.0.0 -m "Release 1.0.0"`

### `git push --tags`
**Comando:** `git push REMOTE TAG|--tags`
**Spiegazione:** Pubblica uno o tutti i tag locali sul remoto.
**Esempio:** `git push origin v1.0.0`

### `git log --grep`
**Comando:** `git log --oneline --grep='TESTO' -i`
**Spiegazione:** Cerca commit il cui messaggio contiene il testo indicato.
**Esempio:** `git log --grep='security' -i`

### `git log -S` e `-G`
**Comando:** `git log -S'TESTO'`; `git log -G'REGEX'`
**Spiegazione:** Trovano commit che hanno aggiunto/rimosso testo o corrispondono a una regex.
**Esempio:** `git log -S'ENABLE_FEATURE' -- config.yml`

### `git blame`
**Comando:** `git blame [-L INIZIO,FINE] FILE`
**Spiegazione:** Mostra commit e autore responsabili di ogni riga.
**Esempio:** `git blame -L 20,40 app.py`

### `git shortlog`
**Comando:** `git shortlog -sn --all`
**Spiegazione:** Riassume il numero di commit per autore.
**Esempio:** `git shortlog -sn --all`

### `git diff --check`
**Comando:** `git diff --check`
**Spiegazione:** Cerca spazi finali ed errori di whitespace prima del commit.
**Esempio:** `git diff --cached --check`

## Esperto

### `git reflog`
**Comando:** `git reflog [show] [REF]`
**Spiegazione:** Mostra dove puntavano HEAD e branch nel tempo, utile per recuperare commit.
**Esempio:** `git reflog --date=iso`

### `git fsck`
**Comando:** `git fsck --full [--unreachable]`
**Spiegazione:** Controlla integrità del database e trova oggetti non raggiungibili.
**Esempio:** `git fsck --full --no-reflogs`

### `git gc`
**Comando:** `git gc [--aggressive]`
**Spiegazione:** Ottimizza il database Git e pulisce oggetti non più necessari.
**Esempio:** `git gc`

### `git repack`
**Comando:** `git repack -Ad`
**Spiegazione:** Ricompatta gli oggetti per ridurre spazio e migliorare prestazioni.
**Esempio:** `git repack -Ad`

### `git cat-file`
**Comando:** `git cat-file -p OBJECT`
**Spiegazione:** Legge direttamente il contenuto di un oggetto interno Git.
**Esempio:** `git cat-file -p HEAD^{tree}`

### `git rev-parse`
**Comando:** `git rev-parse REF|--show-toplevel`
**Spiegazione:** Risolve riferimenti e mostra informazioni strutturali sulla repository.
**Esempio:** `git rev-parse --show-toplevel`

### `git ls-files`
**Comando:** `git ls-files [--cached|--modified|--others|--stage]`
**Spiegazione:** Elenca file nell'indice, modificati o non tracciati.
**Esempio:** `git ls-files --others --exclude-standard`

### `git ls-tree`
**Comando:** `git ls-tree [-r] REF [PATH]`
**Spiegazione:** Elenca i file contenuti nell'albero di un commit.
**Esempio:** `git ls-tree -r --name-only HEAD`

### `git update-index`
**Comando:** `git update-index --assume-unchanged|--no-assume-unchanged FILE`
**Spiegazione:** Modifica temporaneamente il controllo di un file nell'indice; non è un metodo di sicurezza.
**Esempio:** `git update-index --assume-unchanged local.env`

### `git archive`
**Comando:** `git archive --format=tar.gz -o FILE REF`
**Spiegazione:** Crea un archivio dei file versionati senza la directory `.git`.
**Esempio:** `git archive --format=tar.gz -o release.tar.gz v1.0.0`

### `git bisect`
**Comando:** `git bisect start; git bisect bad; git bisect good REF`
**Spiegazione:** Cerca con metodo binario il commit che ha introdotto un bug.
**Esempio:** `git bisect start HEAD v1.0.0`

### `git bisect run`
**Comando:** `git bisect run SCRIPT`
**Spiegazione:** Automatizza il bisect usando il codice di uscita dello script di test.
**Esempio:** `git bisect run ./test-regression.sh`

### `git worktree`
**Comando:** `git worktree add|list|remove PATH [BRANCH]`
**Spiegazione:** Gestisce più directory di lavoro collegate alla stessa repository.
**Esempio:** `git worktree add ../hotfix hotfix/1.0`

### `git submodule`
**Comando:** `git submodule add|init|update|sync URL PATH`
**Spiegazione:** Gestisce repository incluse come dipendenze di un'altra repository.
**Esempio:** `git submodule update --init --recursive`

### `git sparse-checkout`
**Comando:** `git sparse-checkout set DIRECTORY`
**Spiegazione:** Scarica nella working tree solo alcune directory di repository grandi.
**Esempio:** `git sparse-checkout set services/api docs`

### `git push --force-with-lease`
**Comando:** `git push --force-with-lease REMOTE BRANCH`
**Spiegazione:** Riscrive la storia remota solo se non sono apparsi aggiornamenti di altri collaboratori.
**Esempio:** `git push --force-with-lease origin feature/login`

### `git notes`
**Comando:** `git notes add -m "NOTA" COMMIT`
**Spiegazione:** Aggiunge una nota a un commit senza modificare il commit stesso.
**Esempio:** `git notes add -m "Analizzato in audit" HEAD`

### `git rerere`
**Comando:** `git config --global rerere.enabled true`
**Spiegazione:** Memorizza le soluzioni ai conflitti per riapplicarle automaticamente.
**Esempio:** `git config --global rerere.enabled true`

### `git credential`
**Comando:** `git config --global credential.helper MANAGER`
**Spiegazione:** Configura il gestore delle credenziali per i repository remoti.
**Esempio:** `git config --global credential.helper manager-core`

---

## Regole operative

- Preferire `revert` a `reset` quando i commit sono già condivisi.
- Preferire `--force-with-lease` a `--force` sui branch remoti.
- Eseguire `git diff --check` prima del commit e controllare il branch prima del push.
- Usare `clean`, `reset --hard` e `rebase` solo dopo aver salvato il lavoro necessario.
