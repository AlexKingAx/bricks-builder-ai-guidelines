# Bricks AI Guidelines

Sistema di regole e documentazione per far lavorare un assistente AI su siti **Bricks Builder**
senza che combini danni e senza che frammenti il design system.

Nato per i progetti di **Alex Somale — [alex-web.it](https://alex-web.it)**, ma la parte di
regole è generica e riusabile su qualsiasi installazione Bricks.

---

## Indice

1. [Il problema che risolve](#1-il-problema-che-risolve)
2. [Come funziona](#2-come-funziona)
3. [Mappa della cartella](#3-mappa-della-cartella)
4. [Prerequisiti](#4-prerequisiti)
5. [Setup: collegare Bricks all'AI](#5-setup-collegare-bricks-allai)
6. [Problemi noti in fase di collegamento](#6-problemi-noti-in-fase-di-collegamento)
7. [Installare le skill Bricks](#7-installare-le-skill-bricks)
8. [Replicare su un altro sito](#8-replicare-su-un-altro-sito)
9. [Replicare su un altro computer o ambiente](#9-replicare-su-un-altro-computer-o-ambiente)
10. [Anatomia dei file: cosa fa ognuno e perché esiste](#10-anatomia-dei-file-cosa-fa-ognuno-e-perché-esiste)
11. [Le regole non negoziabili](#11-le-regole-non-negoziabili)
12. [Documentazione Bricks](#12-documentazione-bricks)
13. [Appendice: lo starter kit](#13-appendice-lo-starter-kit)

---

## 1. Il problema che risolve

Bricks 2.4 (rilasciato il **16 settembre 2026**) ha introdotto le **AI Abilities**: circa 170
azioni che un assistente AI può eseguire sul sito attraverso il protocollo MCP. Può creare
pagine, modificare elementi, gestire classi globali, variabili, componenti, form, query,
WooCommerce, import/export.

È potente e pericoloso nella stessa misura, per tre motivi che non sono evidenti:

**a) Il design system non ha l'annulla.**

| Cosa modifica l'AI | Si recupera? |
|---|---|
| Pagine e template | ✅ sì, revisioni Bricks |
| Classi globali, variabili, theme styles, componenti, palette, breakpoint | ❌ **no, solo backup del database** |

**b) Un agente lasciato libero frammenta il design system.** Non trova una classe, ne crea una
nuova quasi identica. Ripete lo stesso padding su dieci elementi invece di fare una classe.
In tre sessioni il sistema è irriconoscibile.

**c) I dati di Bricks sull'uso delle risorse sono indicativi, non probatori.** La documentazione
ufficiale lo dice: le informazioni sulle dipendenze *non dimostrano* che una risorsa sia
inutilizzata o eliminabile in sicurezza. Un agente che ci si fida cancella cose che servono.

Questo repository è la risposta: **regole scritte, conferme obbligatorie, un design system
documentato, e un orchestratore che dice all'AI cosa leggere e quando.**

---

## 2. Come funziona

```
CLAUDE.md                 ← l'orchestratore: viene letto a ogni sessione
    │                       contiene SOLO il routing, non le regole
    ├── bricks-guidelines/
    │     ├── 00-SAFETY.md          cosa richiede conferma (precedenza assoluta)
    │     ├── 01 … 05               come si lavora
    │     ├── 10-DESIGN-SYSTEM.md   il kit: classi, variabili, breakpoint
    │     ├── 90-overlay-TEMPLATE   da copiare per ogni sito
    │     ├── 91-pattern-sezioni    come si costruisce una sezione
    │     └── siti/                 un file per ogni progetto
    └── (memoria di Claude)         preferenze che sopravvivono alle sessioni
```

**I numeri raggruppano per tipo**, non sono una sequenza continua:
`00-05` regole generali · `10` il design system · `90-91` template e pattern.

### Il principio

`CLAUDE.md` non contiene regole, contiene **rimandi**. È leggero, viene caricato sempre, e
dice: *"stai per creare una classe? leggi `03-naming.md`"*. Le regole vere stanno nei file
dedicati e si caricano solo quando servono.

### Precedenza in caso di conflitto

```
00-SAFETY.md  >  overlay del sito  >  10-DESIGN-SYSTEM.md  >  91-pattern  >  regole generali
```

La sicurezza vince sempre. Un singolo progetto può discostarsi dal kit, ma deve essere
scritto nel suo overlay.

### I tre livelli di rischio

| | Esempi | Comportamento dell'AI |
|---|---|---|
| 🔴 | cancellazioni, `set-page-elements`, `set-breakpoints`, PHP, permessi | si ferma sempre, conferma obbligatoria |
| 🟠 | classi globali, variabili, impostazioni sito, upload | descrive e aspetta un sì |
| 🟢 | letture, anteprime, modifiche a pagine | procede e riporta |

Una conferma vale **una volta sola**, per quell'azione. Azione non classificata → si tratta
come 🟠.

---

## 3. Mappa della cartella

I numeri **raggruppano per tipo** e indicano l'ordine di lettura dentro ogni gruppo. Non sono
una sequenza continua: i buchi sono voluti, servono ad aggiungere file senza rinumerare tutto.

```
📁 bricks-ai-guidelines/
│
├── 📄 README.md          ← sei qui: setup, replica, spiegazione di ogni file
├── 📄 CLAUDE.md          ← l'orchestratore, caricato a ogni sessione
│
└── 📁 bricks-guidelines/
    │
    │   ┄┄ 00-09 · REGOLE GENERALI ┄┄ valgono su qualsiasi sito Bricks
    ├── 📕 00-SAFETY.md              conferme e azioni rischiose      🔴 precedenza assoluta
    ├── 📗 01-workflow.md            leggi → pianifica → scrivi → verifica
    ├── 📗 02-design-system.md       gerarchia token, regole di costruzione
    ├── 📗 03-naming.md              convenzioni di nomenclatura + BEM obbligatorio
    ├── 📗 04-breakpoints.md         desktop-first, responsive
    ├── 📗 05-codice-custom.md       CSS, SVG, JS, PHP, import HTML/CSS
    │
    │   ┄┄ 10-19 · IL DESIGN SYSTEM ┄┄ i dati concreti del kit
    ├── ⭐ 10-DESIGN-SYSTEM.md       64 classi, 23 variabili, breakpoint
    │
    │   ┄┄ 90-99 · TEMPLATE E PATTERN ┄┄
    ├── 📘 90-overlay-TEMPLATE.md    scheletro da copiare per ogni sito
    ├── 📘 91-pattern-sezioni-blocchi.md   come si costruisce una sezione
    │
    │   ┄┄ SENZA NUMERO · STRUMENTI ┄┄
    ├── 🚀 AVVIO.md                  prompt pronti per iniziare una sessione
    │
    └── 📁 siti/                     un file per progetto  🔒 NON versionata
        ├── README.md                    come si genera un overlay
        └── …                            gli overlay restano in locale
```

### Quale file leggere, in base a cosa stai facendo

| Devi… | Apri |
|---|---|
| **iniziare una sessione** | [`AVVIO.md`](bricks-guidelines/AVVIO.md) → copia il prompt di apertura |
| **eseguire un'azione che scrive** | [`00-SAFETY.md`](bricks-guidelines/00-SAFETY.md) |
| **capire come si lavora** | [`01-workflow.md`](bricks-guidelines/01-workflow.md) |
| **costruire una sezione o un blocco** | [`91-pattern-sezioni-blocchi.md`](bricks-guidelines/91-pattern-sezioni-blocchi.md) |
| **creare una classe o una variabile** | [`02-design-system.md`](bricks-guidelines/02-design-system.md) + [`03-naming.md`](bricks-guidelines/03-naming.md) |
| **dare un nome a qualcosa** | [`03-naming.md`](bricks-guidelines/03-naming.md) |
| **scrivere stili responsive** | [`04-breakpoints.md`](bricks-guidelines/04-breakpoints.md) |
| **scrivere CSS, JS, SVG o PHP** | [`05-codice-custom.md`](bricks-guidelines/05-codice-custom.md) |
| **sapere cosa esiste già nel kit** | [`10-DESIGN-SYSTEM.md`](bricks-guidelines/10-DESIGN-SYSTEM.md) |
| **documentare un sito nuovo** | [`90-overlay-TEMPLATE.md`](bricks-guidelines/90-overlay-TEMPLATE.md) |
| **documentare o rileggere un sito** | [`siti/README.md`](bricks-guidelines/siti/README.md) |

### Ordine di lettura consigliato la prima volta

```
1.  README.md              questo file, per capire l'impianto
2.  CLAUDE.md              come l'AI viene orchestrata
3.  00-SAFETY.md           cosa può andare storto e come si previene
4.  10-DESIGN-SYSTEM.md    cosa c'è già, per non ricrearlo
5.  91-pattern-sezioni     come si costruisce davvero
6.  il resto               al bisogno, seguendo la tabella qui sopra
```

### Legenda

| | Tipo di file |
|---|---|
| 📕 | precedenza assoluta, vince su tutto |
| 📗 | regola generale, vale su ogni sito |
| ⭐ | dati del design system |
| 📘 | template o pattern da applicare |
| 🚀 | strumento pratico |

---

## 4. Prerequisiti

| | Requisito |
|---|---|
| WordPress | **6.9 o superiore** (serve la WordPress Abilities API) |
| Bricks | **2.4 o superiore** |
| Plugin | **WordPress MCP Adapter** — installabile dal pannello Bricks |
| Sito | **HTTPS**, oppure `WP_ENVIRONMENT_TYPE` impostato a `local` |
| Client AI | Claude Code, Codex, Cursor, Copilot o altro client MCP |
| Locale | Node.js, per eseguire `npx` |

---

## 5. Setup: collegare Bricks all'AI

Tutto avviene in **Bricks → AI**, diviso in tre schede: *Configuration*, *Abilities*, *Skills*.

### 5.1 Attivare le abilities

`Bricks → AI → Configuration` → interruttore **Enable Bricks abilities** → salva.

Se l'interruttore è disattivato: manca l'Abilities API (serve WordPress 6.9+) oppure esiste la
costante `BRICKS_DISABLE_MCP`. Quest'ultima blocca la registrazione delle abilities **anche per
WP-CLI**: è il vero interruttore generale.

### 5.2 Installare l'MCP Adapter

Sempre in *Configuration*, sezione **Connect AI client to MCP** → **Set up MCP server**.

| Stato | Significato | Cosa fare |
|---|---|---|
| Not installed | plugin assente | *Install plugin* |
| Inactive | presente ma spento | *Activate plugin* |
| Connected | attivo, route registrata | procedi |
| **Route missing** | adapter caricato, route REST non registrata | permalink, conflitti plugin, log PHP |

L'endpoint risulterà:

```
https://tuosito.it/wp-json/mcp/mcp-adapter-default-server
```

### 5.3 Creare la credenziale

**Connect AI client to MCP → Create a credential**: scegli l'utente WordPress, dai un nome
riconoscibile (`Claude locale`, `Codex staging`), genera.

> ⚠️ **La password viene mostrata una sola volta.** Copiala subito.

**Non usare il tuo amministratore sui siti dei clienti.** L'AI agisce *come quell'utente*: ha
esattamente i suoi permessi, né più né meno. Per un accesso ristretto:

1. `Bricks → Settings → Builder access` → crea una capability custom
2. attiva solo ciò che serve (es. contenuti sì, componenti no)
3. assegnala a un utente dedicato
4. genera la password applicazione **per quell'utente**

> Il tab *Abilities* controlla la disponibilità **a livello di sito**. Non concede permessi
> all'utente. Un'ability può essere attiva e comunque non chiamabile.

### 5.4 Collegare il client

**Claude Code:**

```bash
claude mcp add 'nome-sito' \
  --env WP_API_URL='https://tuosito.it/wp-json/mcp/mcp-adapter-default-server' \
  --env WP_API_USERNAME='utente' \
  --env WP_API_PASSWORD='xxxx xxxx xxxx xxxx' \
  -- npx -y @automattic/mcp-wordpress-remote@latest
```

**Formato JSON generico:**

```json
{
  "mcpServers": {
    "nome-sito": {
      "command": "npx",
      "args": ["-y", "@automattic/mcp-wordpress-remote@latest"],
      "env": {
        "WP_API_URL": "https://tuosito.it/wp-json/mcp/mcp-adapter-default-server",
        "WP_API_USERNAME": "utente",
        "WP_API_PASSWORD": "xxxx xxxx xxxx xxxx"
      }
    }
  }
}
```

> 🔐 La password va **solo** nell'ambiente dell'MCP server. Mai nell'URL, mai negli argomenti
> da riga di comando, mai nella configurazione globale della shell. E mai incollata in chat:
> finisce nella cronologia della conversazione.

**Serve una chat nuova** perché il client carichi il server.

### 5.5 Verificare

```
Chiama bricks-start-here, poi bricks-get-mcp-version, poi bricks-list-ability-status.
Dimmi se le abilities sono attive e quale versione di Bricks è collegata.
```

Se `bricks-start-here` non compare come strumento diretto, si passa dal dispatcher:

```json
{ "ability_name": "bricks/start-here", "parameters": {} }
```

**Risultato atteso su un'installazione standard:** 165 abilities attive su 170. Le 5 spente
sono disattivate di default ed è giusto lasciarle così:

```
bricks/list-builder-permissions      un agente che modifica i permessi
bricks/upsert-builder-capability     può aumentare i propri
bricks/set-builder-role-access
bricks/delete-builder-capability
bricks/execute-php                   PHP arbitrario, nessun sandbox
```

---

## 6. Problemi noti in fase di collegamento

### 6.1 Hostinger Tools blocca le Application Password

**Capitato davvero, settembre 2026.** Su hosting Hostinger il plugin **Hostinger Tools**
impedisce la creazione delle password applicazione. Il sintomo è insidioso:

> La sezione **Password d'applicazione** sparisce dal profilo utente **senza alcun messaggio**.
> Bricks mostra solo *"Le password dell'applicazione non sono disponibili per questo utente o sito"*.

Il caso reale era **doppio**: prima bloccava **All-In-One WP Security (AIOS)**, che almeno
mostrava un avviso esplicito. Disattivata quell'opzione, la sezione è sparita del tutto —
perché sotto c'era il blocco di Hostinger Tools, silenzioso.

**Ordine di controllo su un sito Hostinger:**

```
1. Hostinger Tools          ← controllalo per primo, non dice niente
2. AIOS / Wordfence / Solid Security
3. HTTPS e siteurl
4. mu-plugins e filtri nel codice
```

### 6.2 La sezione sparisce e non sei su Hostinger

WordPress nasconde le password applicazione se **l'opzione `siteurl` nel database non inizia
per `https`**. Guarda il valore salvato, non la barra degli indirizzi: con un proxy o
Cloudflare in modalità *Flexible* il browser mostra il lucchetto mentre il server riceve HTTP.

| Controllo | Dove |
|---|---|
| I due URL sono `https://`? | Impostazioni → Generali |
| `WP_HOME` / `WP_SITEURL` forzati a `http`? | `wp-config.php` |
| Cloudflare in *Flexible*? | SSL/TLS → Overview, metti *Full (strict)* |
| Sito in locale? | `define( 'WP_ENVIRONMENT_TYPE', 'local' );` |

### 6.3 Altri blocchi

Cerca queste stringhe in `functions.php`, nei plugin custom e in `wp-content/mu-plugins/`
(quest'ultima cartella gira **anche a plugin disattivati**):

```
wp_is_application_passwords_available
wp_is_application_passwords_available_for_user
```

### 6.4 Scorciatoia con WP-CLI

Se hai accesso SSH, **WP-CLI crea la credenziale anche quando l'interfaccia la nasconde**:

```bash
wp user application-password create UTENTE "Claude Code" --porcelain
```

Sblocca subito, ma il problema di fondo resta: vale la pena sistemarlo comunque.

---

## 7. Installare le skill Bricks

Le **skill** sono istruzioni di workflow per l'agente. **Non concedono nessun permesso** e non
abilitano nessuna azione: insegnano solo a lavorare meglio (ispezionare prima di scrivere,
non duplicare classi, fare anteprima prima di salvare).

Repository: **[codeerhq/bricks-skills](https://github.com/codeerhq/bricks-skills)** — 45 skill.

**Claude Code, via marketplace:**

```bash
claude plugin marketplace add codeerhq/bricks-skills
claude plugin install bricks@bricks-skills
```

**Aggiornare** — importante, perché una versione vecchia descrive un Bricks che non usi più:

```bash
claude plugin marketplace update bricks-skills
claude plugin update bricks@bricks-skills
```

Dopo l'aggiornamento serve **riavviare il client**. Verifica con:

```bash
claude plugin list
```

> La release `0.1.0` è del 16 settembre 2026, **lo stesso giorno di Bricks 2.4**. Una copia
> precedente non conosce elemento File, trigger carrello, condizioni sulle azioni form,
> permessi CSS/JS/PHP e import parziali.

---

## 8. Replicare su un altro sito

### 8.1 Portare le guidelines

```bash
cp -R CLAUDE.md bricks-guidelines/ /percorso/nuovo-progetto/
```

I file `00`–`05`, `91` e `10-DESIGN-SYSTEM.md` restano **identici**: sono regole generali e il
design system canonico.

### 8.2 Creare l'overlay del sito

```bash
cp bricks-guidelines/90-overlay-TEMPLATE.md bricks-guidelines/siti/nuovo-sito.md
```

> La cartella `siti/` **non è versionata**: gli overlay restano sulla tua macchina. Vedi
> [`siti/README.md`](bricks-guidelines/siti/README.md).

Poi compilalo **leggendo il sito vero**, non a memoria. Le chiamate sono elencate dentro il
template:

```
bricks-list-breakpoints          bricks-list-components
bricks-get-design-context        bricks-list-templates
bricks-list-global-classes       bricks-list-color-palettes
bricks-list-global-variables     bricks-audit-design-system
```

Prompt pronto in `bricks-guidelines/AVVIO.md`.

### 8.3 Seminare il design system

**Non clonare il sito.** Clonare propaga anche i difetti e non è versionabile. In Bricks 2.4:

```
Sito sorgente   bricks/list-transfer-items  →  bricks/export-transfer-package
Sito nuovo      bricks/inspect-transfer-package  →  bricks/import-transfer-package
```

L'ispezione va fatta prima: restituisce conflitti e uno `zipHash` da passare come
`expectedZipHash`. In caso di conflitto **mantiene l'esistente**, salvo `allowOverwrite: true`.

Dal builder l'equivalente è **Global Import & Export**, che trasferisce theme styles, classi,
variabili, palette, breakpoint, componenti, template, font e icone.

I **breakpoint viaggiano dentro il pacchetto**: importando il kit arrivano già configurati.

### 8.4 Creare la palette

La palette di brand **non fa parte del kit**, di proposito. Si crea all'inizio di ogni
progetto, e le classi si raccordano alla palette nuova man mano che servono.

Finché non l'hai creata è **normale** che l'audit segnali variabili orfane (`--color-1…8`,
`--nero`, `--white`) e molte classi non referenziate. Non sono difetti.

---

## 9. Replicare su un altro computer o ambiente

Le guidelines sono file di testo: si copiano e basta. **Quello che non si copia è tutto il
resto.** Su una macchina nuova va rifatto:

| Cosa | Dove vive | Si copia? |
|---|---|---|
| Guidelines | questa cartella | ✅ sì, sono file |
| Overlay dei siti (`siti/`) | locale, non versionata | ❌ si rigenerano leggendo il sito |
| **Configurazione MCP** | `~/.claude.json` | ❌ **contiene le password: da rifare** |
| Plugin skill Bricks | `~/.claude/plugins/` | ❌ da reinstallare |
| Memoria di Claude | `~/.claude/projects/…/memory/` | ⚠️ opzionale, è una preferenza personale |
| Application password | WordPress, per sito e per utente | ❌ da rigenerare |

### Procedura su una macchina nuova

```bash
# 1. Node.js installato (serve per npx)
node --version

# 2. Guidelines
cp -R /sorgente/CLAUDE.md /sorgente/bricks-guidelines/ ~/progetti/sito/

# 3. Skill Bricks
claude plugin marketplace add codeerhq/bricks-skills
claude plugin install bricks@bricks-skills

# 4. Nuova credenziale da Bricks → AI → Configuration, poi:
claude mcp add 'nome-sito' \
  --env WP_API_URL='https://sito.it/wp-json/mcp/mcp-adapter-default-server' \
  --env WP_API_USERNAME='utente' \
  --env WP_API_PASSWORD='...' \
  -- npx -y @automattic/mcp-wordpress-remote@latest

# 5. Verifica
claude mcp list
```

### Su un ambiente cloud o condiviso

- **una credenziale per ambiente**, mai la stessa riusata: si revocano singolarmente
- **utente WordPress dedicato** con permessi ridotti, mai l'amministratore
- credenziali in variabili d'ambiente o in un gestore di segreti, **mai nei file del progetto**
- su domini locali (`.local`, `.test`, `.ddev.site`) Bricks può aggiungere
  `NODE_TLS_REJECT_UNAUTHORIZED=0`: va tenuto **limitato a quel server MCP**, mai globale

### Se metti questa cartella sotto Git

Le guidelines si versionano volentieri. **Non deve finirci dentro nessuna credenziale.**
La configurazione MCP sta in `~/.claude.json`, fuori dal progetto: verifica che resti fuori.

Se un giorno una password finisce in un commit, **va considerata compromessa**: si revoca dal
profilo utente WordPress e se ne genera una nuova. Toglierla dal file non basta, resta nella
storia del repository.

---

## 10. Anatomia dei file: cosa fa ognuno e perché esiste

### `CLAUDE.md` — l'orchestratore

**Cosa fa:** viene caricato automaticamente a ogni sessione. Contiene la sintesi dei tre livelli
di rischio, l'ordine di lettura iniziale, la tabella di routing, le regole senza eccezioni.

**Perché esiste:** un file solo con tutte le regole sarebbe troppo lungo da caricare sempre e
verrebbe letto male. Questo resta leggero e **rimanda**. Dentro non ci vanno regole, solo
riferimenti: se una regola è qui *e* in un altro file, prima o poi divergono.

---

### `bricks-guidelines/00-SAFETY.md` — conferme e azioni rischiose

**Cosa fa:** classifica ogni ability nei tre livelli 🔴🟠🟢, elencandole per nome. Definisce il
formato della richiesta di conferma e quando pretendere un backup.

**Perché esiste:** è il file che Alex ha chiesto per primo. Senza una lista esplicita, un
agente decide da sé cosa è rischioso — e sbaglia, perché l'intuizione dice che modificare una
pagina è più grave che modificare una classe, mentre è **l'opposto**: la pagina ha le revisioni,
la classe no.

**Le tre regole che chiudono le scappatoie:**
- una conferma vale una volta sola, non si estende all'azione successiva
- azione non classificata → si tratta come 🟠
- *"fai la pagina X"* non autorizza a toccare il design system

**Ha precedenza assoluta** su ogni altro file.

---

### `01-workflow.md` — come si opera

**Cosa fa:** il ciclo leggi → pianifica → scrivi → verifica. Spiega il meccanismo
`checkout → preview → apply` delle abilities di scrittura, quale percorso scegliere in base al
peso della modifica, e come comportarsi davanti a un commit parziale.

**Perché esiste:** il fallimento più comune non è la cancellazione sbagliata, è **scrivere
senza aver letto**. Si creano doppioni, si ignorano le variabili esistenti, si sbaglia la
logica responsive. Qui è scritto che la fase di lettura non è saltabile.

Contiene anche il dettaglio che i changeset multi-risorsa **non sono atomici**: un commit
parziale è uno stato previsto, e in quel caso ci si ferma e si segnala.

---

### `02-design-system.md` — regole di costruzione

**Cosa fa:** la gerarchia variabile → classe → componente → stile sull'elemento. Il divieto di
valori hardcoded. L'obbligo di cercare prima di creare. L'uso dei generatori di scale. La
soglia del due.

**Perché esiste:** sono le regole che impediscono la frammentazione. La più importante è
*cerca prima di creare*: un design system muore per accumulo di quasi-duplicati, non per una
cancellazione sbagliata.

---

### `03-naming.md` — convenzioni e BEM

**Cosa fa:** le convenzioni di nomenclatura e le **regole BEM obbligatorie** per ogni classe
nuova: `blocco__elemento--modificatore`, un solo livello di `__`, modificatore mai da solo,
nomi che dicono il ruolo e non l'aspetto. Con la tabella sbagliato → corretto.

**Perché esiste:** il design system di partenza aveva **cinque convenzioni diverse** che
convivevano. BEM è la regola scelta per fermare la deriva sulle classi future senza toccare
lo storico.

**Il dettaglio che tiene:** le classi storiche che non sono BEM restano come sono ma **non
fanno precedente**. Senza questa frase, al primo dubbio l'agente guarda il kit, vede
`sottotitolo`, e crea `sottotitolo-servizi`.

Due sole famiglie restano fuori da BEM: le utility `lt-` e la tipografia `body-`, perché non
sono blocchi.

---

### `04-breakpoints.md` — responsive

**Cosa fa:** obbliga a chiamare `bricks-list-breakpoints` a ogni sessione, spiega la differenza
fra desktop-first e mobile-first, documenta lo standard di Alex.

**Perché esiste:** è **l'errore più costoso e più facile**. Se scrivi gli stili con la logica
sbagliata il sito si rompe ovunque tranne su un dispositivo, e spesso non te ne accorgi subito.

🔴 **`bricks/set-breakpoints` è vietato senza eccezioni.** I breakpoint presenti su un sito non
si impostano, non si correggono, non si allineano: arrivano con il kit. Se non sono quelli
giusti significa che il kit non è stato importato.

---

### `05-codice-custom.md` — CSS, SVG, JS, PHP

**Cosa fa:** l'ordine delle preferenze (controlli → classe → elemento → codice), dove mettere
il CSS, il funzionamento di **CSS Sync**, i requisiti per JavaScript e PHP, la pipeline di
import HTML/CSS.

**Perché esiste:** raccoglie le trappole. Due in particolare:

- **CSS Sync** (novità 2.4, opt-in): sincronizza Custom CSS e controlli in entrambe le
  direzioni, ma **se una proprietà è definita in tutti e due i posti vince il Custom CSS**. È
  la spiegazione di *"il controllo non fa niente"*.
- **Instant Navigation** (attiva di default in 2.4): la pagina cambia senza ricaricare, quindi
  il codice legato a `DOMContentLoaded` non rigira.

---

### `10-DESIGN-SYSTEM.md` — il kit canonico ⭐

**Cosa fa:** documenta il design system di riferimento: 64 classi globali raggruppate per
famiglia, 23 variabili con i valori veri, i breakpoint. Più la procedura per portarlo su un
sito nuovo.

**Perché esiste:** prima il design system veniva propagato **clonando interi siti**. Ogni copia
ereditava i fossili della precedente — è così che un blocco `ft-6__*` mai adottato è arrivato
identico su due progetti diversi, con gli stessi ID.

Averlo scritto lo rende un **artefatto versionabile** invece di una consuetudine.

**Cosa non contiene, di proposito: la palette.** Si crea per ogni progetto.

---

### `90-overlay-TEMPLATE.md` — il template per ogni sito

**Cosa fa:** lo scheletro da compilare per documentare un sito: ambiente, breakpoint, lingua,
variabili, colori, classi, componenti, theme styles, template, regole specifiche, **cose da non
toccare**, debito tecnico noto, stack.

**Perché esiste:** le regole generali valgono ovunque, ma ogni progetto ha le sue particolarità.
La sezione *Da non toccare* è quella che evita più danni: raccoglie le cose che sembrano
sbagliate ma sono volute.

---

### `91-pattern-sezioni-blocchi.md` — come si costruisce una sezione

**Cosa fa:** lo scheletro ricorrente (`section` → `container` → contenuto), le larghezze, le
spaziature via `gap`, l'anatomia di intestazioni e card, e la **regola del due**.

**Perché esiste:** le altre regole dicono *cosa* non fare. Questa dice *come si fa*. È stata
ricavata leggendo un sito pubblicato e validata da Alex.

**Due contenuti che valgono da soli:**

- **Tag e aspetto sono assi indipendenti.** Il tag (`h1`, `h2`, `div`) lo decide la SEO; la
  classe `body-fake-*` decide l'aspetto. Un `h1.body-fake-h5` è un titolo che pesa nella
  gerarchia ma si vede piccolo. È il motivo per cui esiste tutta la famiglia `body-fake-*`.
- **La regola del due:** se lo stesso CSS serve a **2 o più elementi**, si crea una classe.
  Non si aspetta la terza ripetizione.

---

### `AVVIO.md` — prompt pronti

**Cosa fa:** il prompt di apertura sessione, come formulare una richiesta di lavoro, i prompt
di sola lettura sempre sicuri, cosa aspettarsi durante il lavoro.

**Perché esiste:** le regole funzionano solo se vengono lette. Il prompt di apertura forza la
lettura di `00-SAFETY.md` e dell'overlay prima di qualsiasi cosa, e chiude con *"non modificare
niente finché non te lo chiedo"*.

> Se l'AI procede senza chiedere quando doveva chiedere, **le guidelines non sono state lette.**
> Riapri con il prompt di apertura.

---

### `siti/` — un file per progetto 🔒

**Cosa fa:** contiene un overlay compilato per ogni sito su cui si lavora: ambiente,
breakpoint, classi, componenti, cose da non toccare, debito noto.

**Perché non è versionata:** questi file sono **generati**, non sorgente. Si producono
copiando `90-overlay-TEMPLATE.md` e compilandolo leggendo il sito vero, e spesso riguardano
progetti di clienti. Versionarli significherebbe pubblicare informazioni che non sono mie da
pubblicare, e tenere in repo dati che invecchiano al primo intervento sul sito.

Nel repository c'è solo [`siti/README.md`](bricks-guidelines/siti/README.md), che spiega
come generarne uno. Tutto il resto è escluso da `.gitignore`.

> L'overlay di un sito **vince sulle regole generali e sul design system canonico**: un
> progetto può avere regole sue. Non vince mai su `00-SAFETY.md`.

---

## 11. Le regole non negoziabili

1. **Leggi prima di scrivere.** Sempre `get-design-context` prima di toccare il design system.
2. **Mai un valore hardcoded.** Si usano le variabili esistenti.
3. **Mai creare senza aver cercato un duplicato.** Con `bricks-list-global-classes`, non con
   `get-design-context` che **tronca l'elenco a 100**.
4. **Ogni classe nuova rispetta BEM.** Eccetto utility `lt-` e tipografia `body-`.
5. **Stile uguale su 2+ elementi → si crea una classe.** Mai ripetuto sull'elemento.
6. **Mai assumere la logica responsive.** `bricks-list-breakpoints`, ogni volta.
7. **Mai cambiare i breakpoint presenti sul sito.**
8. **Mai dire "fatto" senza aver riletto** il risultato.

---

## 12. Documentazione Bricks

| Risorsa | Link |
|---|---|
| **AI Abilities and Skills** — la guida completa: setup MCP, credenziali, permessi, PHP, WP-CLI, sicurezza, troubleshooting | [academy.bricksbuilder.io/builder/features/ai-abilities-and-skills](https://academy.bricksbuilder.io/builder/features/ai-abilities-and-skills/) |
| **Changelog Bricks 2.4** — tutte le novità di questa versione | [bricksbuilder.io/release/bricks-2-4](https://bricksbuilder.io/release/bricks-2-4/) |
| **Bricks Academy** — documentazione generale | [academy.bricksbuilder.io](https://academy.bricksbuilder.io/) |
| **Bricks Skills** — il pacchetto di skill per gli agenti | [github.com/codeerhq/bricks-skills](https://github.com/codeerhq/bricks-skills) |
| **WordPress MCP Adapter** | link *View on GitHub* dentro `Bricks → AI → Configuration` |

### Limiti tecnici da conoscere

| Operazione | Limite |
|---|---|
| Conversione HTML/CSS | **2 MiB** totali |
| Upload font | 8 MiB per file |
| Upload icona SVG | 1 MiB di markup |
| Upload media | limite di WordPress · 30s di timeout sui download remoti |
| Checkout multi-target | 25 target |
| Modifiche in un edit plan | 64 |

---

## 13. Appendice: lo starter kit

Riferimento rapido. Il dettaglio completo con ID, categorie e note sta in
[`bricks-guidelines/10-DESIGN-SYSTEM.md`](bricks-guidelines/10-DESIGN-SYSTEM.md).

### Breakpoint

```
DESKTOP-FIRST  ·  base 1920px
desktop 1920  →  laptop 1279  →  tablet_portrait 991  →  mobile_landscape 767  →  mobile_portrait 478
```

### Variabili (23)

**Scale generate** — `heading-size-H1…H6` e `text-size-B1…B4`, `clamp()` fluido
(min 16px ratio 1.25 · max 20px ratio 1.333).

| Variabile | Valore | Ruolo |
|---|---|---|
| `--heading-line-height` | `1.2` | line-height titoli |
| `--body-line-height` | `1.5` | line-height testo |
| `--section-standard-padding` | `60px` | respiro verticale sezioni |
| `--lt-radius` | `8px` | raggio standard |
| `--lt-radius-medium` | `12px` | raggio medio |
| `--lt-border-color` | `rgb(221, 221, 221)` | colore bordi |
| `--lt-spacing-nano` | `.3rem` | scala spaziature |
| `--lt-spacing-micro` | `8px` | scala spaziature |
| `--lt-spacing-small` | `12px` | scala spaziature |
| `--lt-spacing-md` | `18px` | scala spaziature |
| `--lt-spacing-md2` | `24px` | scala spaziature |
| `--lt-spacing-large` | `32px` | scala spaziature |
| `--lt-real-vh` | `var(--vh, 1vh)` | altezza viewport reale, `--vh` da JavaScript |

### Classi globali (64)

**Layout e utility (22)**
```
lt-section-container          lt-section-container-text     lt-section-container-full
lt-section-container-95       lt-section-container-90       lt-section-container-no-anim
lt-container-full             lt-width-full-important       lt-padding-t-b
lt-padding-contenitori        lt-flex                       lt-flex-row
lt-flex-col                   lt-items-center               lt-justify-center
lt-text-center                lt-white-text                 lt-black-text
lt-bg-image                   lt-border-image               lt-overlay-entrata
lt-pineapple-radius
```

> `lt-white-text` e `lt-black-text` sono **semantiche, non letterali**: significano *"il colore
> chiaro / scuro della palette di questo progetto"*. Su un sito `lt-white-text` può rendere
> giallo ed essere corretto.

**Tipografia (11)**
```
body-fake-h1   body-fake-h2   body-fake-h3   body-fake-h4   body-fake-h5   body-fake-h6
body-intro-text-B1            body-intro-text-B2
body-medium-text-B3           body-small-text-B4            header-small
```

**Spaziature colonne (6)** — distanza fra i figli, via `gap`
```
lt-col-spacing-nano   lt-col-spacing-micro   lt-col-spacing-small
lt-col-spacing-md     lt-col-spacing-md2     lt-col-spacing-large
```

**Animazioni e transizioni (11)**
```
lt-anim-fadein        lt-anim-fadeinup       lt-anim-scale        lt-anim-pineapple
lt-transition-300ms   lt-transition-450ms    lt-transition-600ms
title-anim (×2)       entrata                scrolling-text
```

**Icone (4)**
```
icn-footer   icona-navbar-menu   icon-image-menu   icons-contacts
```

**Footer alternativo (3)** — mai adottato, viaggia con ogni copia del kit
```
ft-6__col   ft-6__heading   ft-6__menu
```

**Blocchi funzionali (7)**
```
bullet   colonne-contatti   div-navbar-menu   highlight-container   hyphens   rail   text-item
```

### Debito noto del kit — accettato, non da correggere

| Cosa | Nota |
|---|---|
| `title-anim` esiste due volte | ID diversi, entrambe senza impostazioni |
| 4 classi vuote | definite ma senza impostazioni |
| Naming misto IT/EN | `icn-footer` vs `icona-navbar-menu` vs `icon-image-menu` |
| `ft-6__*` mai adottato | footer alternativo che viaggia con ogni copia |
| Nessuna classe per i bottoni | i bottoni non hanno una classe globale dedicata |
| Spacing senza categoria | heading e text usano il generatore di scale, lo spacing no |

**Tutto questo resta.** L'elenco serve a non ripetere gli stessi schemi nelle classi **nuove**,
non come lista di cose da sistemare.

---

*Documentazione aggiornata al 18 settembre 2026 · Bricks 2.4 · Bricks Skills 0.1.0*
