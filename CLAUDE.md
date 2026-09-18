# CLAUDE.md — Orchestratore Bricks

Questo file dice **quando** leggere **cosa**. Le regole vere stanno in `bricks-guidelines/`.
Non duplicare qui il contenuto degli altri file: qui ci va solo il routing.

---

## ⚠️ REGOLA NUMERO UNO — Conferme

**Prima di qualsiasi azione potenzialmente rischiosa: fermati, descrivi, aspetta un "sì" esplicito.**

Sintesi operativa (il dettaglio è in `bricks-guidelines/00-SAFETY.md`):

| | Cosa | Comportamento |
|---|---|---|
| 🔴 | cancellazioni, sostituzioni totali, PHP, breakpoint, permessi | **conferma obbligatoria, sempre** |
| 🟠 | design system, impostazioni sito, modifiche multiple, upload | **descrivi e aspetta ok** |
| 🟢 | letture, preview, modifiche a pagine (coperte da revisione) | procedi, poi riporta |

**Il fatto che cambia tutto:**
> Pagine e template hanno le revisioni. **Classi globali, variabili, theme styles, componenti, palette e breakpoint NO.**
> Toccare il design system è più pericoloso che toccare una pagina.

**Una conferma vale una volta sola, per quella azione.** Non si estende alla successiva.
**Azione non classificata → trattala come 🟠.**

---

## Ordine di lettura a inizio sessione

**Sempre, prima di toccare qualsiasi cosa:**

1. `bricks-guidelines/00-SAFETY.md` — non negoziabile
2. `bricks-guidelines/siti/<sito>.md` — l'overlay del sito su cui stai lavorando
3. `bricks-list-breakpoints` — desktop-first o mobile-first? **Leggilo, non assumerlo**
4. `bricks-get-design-context` — cosa esiste già

Se non sai su quale sito stai lavorando → **chiedi**. Non dedurlo.

---

## 🔴 SITO SENZA OVERLAY → PRIMA SI DOCUMENTA

**Se in `bricks-guidelines/siti/` non esiste il file di questo sito, non si lavora.**
La prima cosa da fare è crearlo. Non dopo, non "se c'è tempo": prima.

```
ls bricks-guidelines/siti/
   │
   ├─ c'è <sito>.md   →  leggilo e procedi
   └─ non c'è         →  🛑 STOP: genera l'overlay
                          processo completo in siti/README.md
                          prompt pronto in AVVIO.md
```

In sintesi: chiedi **quale sito e quale ambiente** (non si deducono) → copia
`90-overlay-TEMPLATE.md` in `siti/<sito>.md` → leggi il sito in **sola lettura** →
compila ogni sezione → riporta incoerenze e domande → **solo allora** si lavora.

Senza overlay non sai se è produzione, che breakpoint ha, cosa esiste già e cosa
non va toccato. Ogni azione successiva sarebbe un'ipotesi.

> La cartella `siti/` è vuota in una installazione pulita: gli overlay non sono
> versionati, si rigenerano su ogni macchina e per ogni progetto.

---

## Routing — quale file leggere per cosa

| Stai per… | Leggi |
|---|---|
| eseguire un'azione che scrive | `00-SAFETY.md` |
| iniziare un lavoro qualsiasi | `01-workflow.md` |
| **costruire una sezione o un blocco** | **`91-pattern-sezioni-blocchi.md`** |
| creare/modificare classi, variabili, colori, componenti | `02-design-system.md` + `03-naming.md` |
| sapere quali classi/variabili esistono nel kit di Alex | **`10-DESIGN-SYSTEM.md`** |
| seminare il design system su un sito nuovo | **`10-DESIGN-SYSTEM.md`** |
| dare un nome a qualcosa | `03-naming.md` |
| scrivere stili responsive | `04-breakpoints.md` |
| scrivere CSS, JS, SVG o PHP | `05-codice-custom.md` |
| importare HTML/CSS | `05-codice-custom.md` |
| **lavorare su un sito non ancora documentato** | 🛑 **`siti/README.md`** — processo obbligatorio |
| documentare un sito nuovo | `90-overlay-TEMPLATE.md` |
| aprire una sessione di lavoro | `AVVIO.md` — prompt pronti |

**Skill Bricks installate** — usale per il dominio specifico invece di improvvisare:

| Argomento | Skill |
|---|---|
| orientamento generale | `bricks-start-here` |
| query loop | `bricks-query-loops` |
| componenti | `bricks-components` |
| form | `bricks-forms` |
| popup | `bricks-popups` |
| interazioni | `bricks-interactions` |
| condizioni elemento | `bricks-element-conditions` |
| template e condizioni | `bricks-templates-conditions` |
| dynamic data | `bricks-dynamic-data` |
| WooCommerce | `bricks-woocommerce` |
| codice custom | `bricks-custom-code` |
| verifica nel browser | `bricks-browser-verify` |
| stile dei commenti, SVG, preferenze personali | `alex-config` |

> Se una skill Bricks copre l'argomento, **caricala**. Contiene dettagli che queste guidelines non ripetono.

---

## Il ciclo di lavoro

```
1. LEGGI      design system + pagina + breakpoint
2. PIANIFICA  proponi cosa riusi e cosa crei
3. CONFERMA   se 🟠 o 🔴, fermati e aspetta
4. SCRIVI     preview → apply
5. VERIFICA   rileggi il risultato, non fidarti dell'ok
```

Dettagli in `01-workflow.md`.

---

## Le regole che non hanno eccezioni

0. **Nessun overlay del sito → non si lavora.** Si genera prima. Vedi `siti/README.md`.
1. **Leggi prima di scrivere.** Sempre `get-design-context` prima di toccare il design system.
2. **Mai un valore hardcoded.** Si usano le variabili esistenti.
3. **Mai creare senza aver cercato un duplicato.** `list-global-classes`, non `get-design-context` (tronca a 100).
3-bis. **Ogni classe nuova rispetta BEM** (`blocco__elemento--modificatore`). Uniche eccezioni: le utility `lt-` e la tipografia `body-`. Dettagli e casi limite in `03-naming.md`.
3-ter. **Stile uguale su 2+ elementi → si crea una classe**, mai ripetuto sull'elemento. Se serve, più classi in BEM. Vedi `91-pattern-sezioni-blocchi.md` §7.
4. **Mai assumere la logica responsive.** `list-breakpoints`, ogni volta.
5. **Mai dire "fatto" senza aver riletto** il risultato.

---

## Struttura

```
CLAUDE.md                        ← questo file: routing
bricks-guidelines/
  AVVIO.md                       ← prompt pronti per iniziare una sessione
  00-SAFETY.md                   ← 🔴 conferme e azioni rischiose (precedenza assoluta)
  01-workflow.md                 ← come si opera: leggi, pianifica, scrivi, verifica
  02-design-system.md            ← gerarchia token, regole di costruzione
  03-naming.md                   ← convenzioni di nomenclatura
  04-breakpoints.md              ← desktop-first vs mobile-first
  05-codice-custom.md            ← CSS, SVG, JS, PHP, import HTML/CSS
  10-DESIGN-SYSTEM.md            ← ⭐ il design system canonico di Alex
  90-overlay-TEMPLATE.md         ← template da compilare per ogni sito
  91-pattern-sezioni-blocchi.md  ← ⭐ come si costruisce una sezione
  siti/
    README.md                    ← 🛑 processo obbligatorio per documentare un sito
    <sito>.md                    ← overlay dei progetti — NON versionati, locali
```

---

## Come si riusa su un altro progetto

1. Copia `CLAUDE.md` + `bricks-guidelines/`
2. Copia `90-overlay-TEMPLATE.md` in `siti/<nuovo-sito>.md`
3. Compila l'overlay **leggendo il sito vero** (le chiamate sono elencate nel template)
4. I file `00`–`05` restano invariati: sono regole generali

**Precedenza in caso di conflitto:**
```
00-SAFETY.md  >  overlay del sito  >  10-DESIGN-SYSTEM.md  >  91-pattern  >  file generali 01–05
```
L'overlay di un sito può discostarsi dal kit (un progetto può avere regole sue),
ma va scritto esplicitamente lì. In assenza di indicazioni, vale il kit.

---

## Stato

```
Design system di riferimento:   ✅ bricks-guidelines/10-DESIGN-SYSTEM.md
Fonte del kit:                  webagencyalba.it
Definito il:                    2026-09-17
```

**Su ogni sito nuovo si parte da `10-DESIGN-SYSTEM.md`**, salvo che Alex dica diversamente per quel progetto specifico.

Il kit comprende 64 classi globali, 23 variabili e i breakpoint (desktop-first, base 1920px). **Non comprende la palette**: quella si crea all'inizio di ogni progetto e le classi le si raccorda man mano.

alex-web.it è **legacy** e non va usato come modello — vedi `bricks-guidelines/siti/alex-web-it.md`.
