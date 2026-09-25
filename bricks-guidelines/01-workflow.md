# 01 · WORKFLOW — Come si opera su Bricks

Regola madre: **leggere prima, proporre poi, scrivere alla fine, verificare sempre.**

---

## Le quattro fasi

```
1. LEGGI      capisci com'è fatto adesso
2. PIANIFICA  proponi cosa cambi e con quali token esistenti
3. SCRIVI     preview → conferma → apply
4. VERIFICA   rileggi il risultato, non fidarti dell'"ok" della scrittura
```

Saltare la fase 1 è la causa numero uno dei disastri: si creano classi duplicate, si ignorano variabili esistenti, si scrive mobile-first su un sito desktop-first.

---

## Fase 1 — Leggi (obbligatoria, sempre)

**A inizio sessione, sempre:**

| Chiamata | Cosa ti dice |
|---|---|
| `bricks-get-design-context` | palette, classi, variabili, theme styles, componenti, breakpoint |
| `bricks-list-breakpoints` | **desktop-first o mobile-first** — vedi `04-breakpoints.md` |

**Prima di toccare una pagina:**

| Chiamata | Quando |
|---|---|
| `bricks-get-page-structure` | per capire l'alberatura senza scaricare tutto |
| `bricks-get-page-elements` | quando ti servono le impostazioni reali |
| `bricks-find-post` | per trovare l'ID giusto partendo dal nome |

**Prima di creare qualsiasi cosa nel design system:**

`bricks-get-design-context` con `includeUsage: true` → ti dice anche dove sono usati i componenti.

> ⚠️ `get-design-context` tronca l'elenco classi a 100. Se il sito ne ha di più, usa `list-global-classes` per l'elenco completo prima di dire "questa classe non esiste".

---

## Fase 2 — Pianifica e proponi

Prima di scrivere, l'utente deve poter dire "no, non così".

Proponi in questo formato:

```
PIANO

Obiettivo:    sezione hero sulla pagina Servizi
Riuso:        lt-section-container, body-fake-h1, button-primary
              var(--lt-padding), var(--lt-spacing-large)
Da creare:    nessuna classe nuova
Scrive su:    pagina Servizi (ID 142) — coperta da revisione
Livello:      🟢 verde
```

Se il piano richiede classi o variabili nuove, **quello è già un 🟠** — vedi `00-SAFETY.md`.

---

## Fase 3 — Scrivi

Bricks 2.4 non scrive "al volo". Le abilities di scrittura seguono uno schema a tre tempi:

```
checkout   →   preview   →   apply
(leggi)        (valida, non salva)   (scrive il candidato bloccato)
```

**Cosa devi sapere in pratica:**

- `preview-*` **non modifica niente**. Usala liberamente, anche solo per capire se una modifica è valida.
- `apply-*` accetta **solo il token del preview**, non i dati. Non puoi "saltare" il preview.
- Se il sito è cambiato fra preview e apply (hai salvato dal builder, un altro processo ha scritto), **apply viene rifiutata**. È corretto: rifai il checkout.

### Scegli il percorso più leggero che risolve

| Serve | Usa |
|---|---|
| cambiare un testo, una variabile, una singola impostazione | `commit-exact-site-edits` (1 chiamata) |
| modificare una pagina/risorsa identificata chiaramente | `resolve-agent-file` → `commit-agent-file` (2 chiamate) |
| più modifiche su più target (max 25 target / 64 modifiche) | `checkout-site-edit-map` → `commit-site-edit-plan` |
| lavoro strutturale su una pagina | `checkout-page-workspace` → `preview` → `apply` |
| aggiornare una singola risorsa di design | `checkout-design-resource-workspace` → `preview` → `apply` |

> Il percorso design-resource-workspace è **solo update**: non crea, non cancella, non rinomina.

### expectedValue vs allowBlindWrite

Le abilities di modifica puntuale chiedono di dichiarare cosa ti aspetti di trovare:

```
expectedValue: "Il vecchio titolo"     ✅ se non combacia, si ferma
allowBlindWrite: true                  🔴 scrive senza controllare
```

**Usa sempre `expectedValue`.** `allowBlindWrite` richiede conferma esplicita dell'utente.

---

## Fase 4 — Verifica

**Mai dichiarare "fatto" senza aver riletto.** La risposta di una scrittura dice che l'operazione è passata, non che il risultato è quello voluto.

| Verifica | Come |
|---|---|
| l'elemento c'è ed è giusto | `bricks-get-page-elements` |
| l'HTML generato è corretto | `bricks-render-elements` |
| il design system non si è sporcato | `bricks-get-design-context` |
| la pagina si vede bene davvero | browser + screenshot |
| esiste il punto di ripristino | `bricks-list-revisions` |

---

## Quando qualcosa va storto

### Commit parziale
I changeset multi-risorsa **non sono atomici**. Un `apply-site-changeset` può restituire:

| Stato | Cosa significa | Cosa fare |
|---|---|---|
| commit completato | tutto ok | verifica e chiudi |
| in corso | time-slice, non ha finito | `resume-site-changeset` |
| **commit parziale** | una parte è passata, una no | **fermati, segnala all'utente** |
| errore prima del commit | non ha scritto nulla | sicuro, si può ritentare |
| **ripristino manuale** | serve un intervento umano | **fermati, segnala all'utente** |

Su "commit parziale" o "ripristino manuale: **non** chiamare `resolve-site-changeset` di iniziativa.** Quella cancella le prove del problema, non lo risolve. Prima si ispeziona.

### Risposta persa
Le abilities usano chiavi di idempotenza: **ripetere la stessa identica chiamata recupera la risposta**, non duplica l'operazione. Non improvvisare una chiamata diversa.

### Import parziale
Un import HTML/CSS può tornare "parziale" con l'elenco di cosa è stato omesso e perché — di solito permessi mancanti. Non insistere: **riporta le omissioni all'utente**.

---

## Limiti tecnici da tenere a mente

| Operazione | Limite |
|---|---|
| conversione HTML/CSS | **2 MiB** totali (HTML + CSS) |
| upload font | 8 MiB per file |
| upload icona SVG | 1 MiB di markup |
| upload media | limite di WordPress · 30s di timeout sui download remoti |
| checkout multi-target | 25 target |
| modifiche in un edit plan | 64 |

---

## Errori già fatti — non ripeterli

Lezioni da lavori reali. Ognuna è costata almeno un giro di correzione.

| Errore | Cosa fare |
|---|---|
| **CSS raw nelle classi che agisce sui figli** (`:nth-child`, `:not()`, discendenti) | Mai senza permesso. Una classe in più sull'elemento, con i campi nativi. Vedi `05-codice-custom.md` |
| **Chiavi inventate salvate in silenzio** nelle classi (`_gap` su un blocco, ecc.): nessun errore, nessun effetto | Le classi globali non validano. Controlla le chiavi con `get-element-schema` + `controlKeys` prima di scriverle |
| **Blocchi Bricks vanno a capo di default** (`flex-wrap: wrap`) e sono in colonna: il contenuto finisce in una seconda colonna quando l'altezza è fissa, o gli elementi si impilano | Dove serve, imposta `_flexWrap: nowrap` e `_direction` esplicitamente, e misura nel browser |
| **Breakpoint desktop-first**: un override mobile si scrive con `:mobile_landscape` (≤767px), non con una regola a parte | Rileggi i breakpoint e non duplicare il valore base nell'override |
| **Il CSS di classe esce solo se la classe è usata** in un elemento della pagina: un nodo creato da JS non ne beneficia | La struttura statica sta nel builder; il JS imposta solo i valori dinamici |
| **`set-page-elements` riscrive tutto**, compresa la firma del codice (es. query PHP firmata) | Usa `add-element`, `update-element`, `remove-element`. Non esiste "sposta elemento": pianifica la struttura prima |
| **Il codice non passa dall'API** (`bricks_code_sensitive_write_forbidden`): PHP nel query editor, `{echo:…}`, JS | Non aggirare. Costruisci il resto, dì all'utente cosa incollare e dove, ricordagli di risalvare per firmare |
| **Il parametro URL con lo stesso nome del post type** (`?auto=`) dà 404: WordPress lo legge come query variable | Scegli un nome diverso dallo slug di ogni CPT e tassonomia |
| **Contenuti di un altro progetto** trovati in header/footer clonati | Non fidarti di "è lo starter kit": leggi header e footer prima di dire cosa c'è |
| **"Fatto" senza misurare** | Verifica nel browser su desktop, tablet e mobile: allineamento, overflow, elementi tagliati |

---

## Cose che non si fanno mai

- dichiarare "fatto" senza rilettura
- creare una classe senza aver cercato se esiste
- scrivere `_cssCustom` senza permesso chiesto prima — vedi `05-codice-custom.md`
- usare `set-page-elements` quando basta `update-element`
- assumere mobile-first senza aver letto i breakpoint
- procedere dopo un commit parziale come se nulla fosse
