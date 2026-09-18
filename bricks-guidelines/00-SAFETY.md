# 00 · SAFETY — Azioni rischiose e conferme obbligatorie

> **Questo file ha la precedenza su ogni altro file di queste guidelines.**
> In caso di conflitto fra una regola qui e una regola altrove, vince questa.

---

## Il principio

Un'azione va fermata **prima** di essere eseguita, non spiegata dopo.

Se un'azione rientra nei livelli 🔴 o 🟠 qui sotto: **fermati, descrivi cosa stai per fare, aspetta un "sì" esplicito.**
Non vale come conferma: un "ok" dato a un'altra domanda, un permesso dato prima su un'azione diversa, il fatto che l'utente abbia descritto l'obiettivo finale.

**Una conferma vale una volta sola, per quella azione.** Non si estende alla successiva.

---

## Il fatto che rende tutto più delicato: le revisioni non coprono tutto

| Cosa modifichi | Si può annullare? | Come |
|---|---|---|
| Pagine, template, elementi | ✅ Sì | revisioni Bricks (`list-revisions`, `restore-revision`) |
| **Classi globali** | ❌ **No** | solo backup del database |
| **Variabili globali** | ❌ **No** | solo backup del database |
| **Theme styles** | ❌ **No** | solo backup del database |
| **Componenti** | ❌ **No** | solo backup del database |
| **Palette e colori** | ❌ **No** | solo backup del database |
| **Breakpoint** | ❌ **No** | solo backup del database |

Questo ribalta l'intuizione comune: *toccare il design system è più pericoloso che toccare una pagina*, anche se sembra una modifica più piccola.

---

## 🔴 LIVELLO ROSSO — Mai senza conferma esplicita. Mai in autonomia.

Azioni distruttive o irreversibili. Chiedi **sempre**, anche se l'utente ha già approvato qualcosa di simile poco prima.

### Cancellazioni
```
bricks/delete-global-class        bricks/delete-template
bricks/delete-global-variable     bricks/delete-component
bricks/delete-theme-style         bricks/delete-post
bricks/delete-color               bricks/delete-media
bricks/delete-color-palette       bricks/delete-nav-menu
bricks/delete-global-query        bricks/delete-nav-menu-items
bricks/delete-sidebar             bricks/delete-custom-font
bricks/delete-custom-icon         bricks/delete-custom-icon-set
bricks/delete-global-query-category
bricks/cleanup-orphaned-elements
```

### Sostituzioni totali
| Ability | Perché è rossa |
|---|---|
| `bricks/set-page-elements` | **sostituisce l'intero albero della pagina**, non modifica |
| `bricks/set-breakpoints` | cambia i breakpoint = **rompe tutti gli stili responsive del sito** |
| ~~`bricks/set-global-variables`~~ | **declassata a 🟠 — vedi nota sotto** |
| `bricks/restore-revision` | sovrascrive lo stato attuale con uno vecchio |
| `bricks/import-transfer-package` con `allowOverwrite: true` | sovrascrive dati esistenti |
| `bricks/run-woo-setup` | crea/modifica pagine e template WooCommerce in blocco |
| `bricks/commit-site-foundation` | costruisce una fondazione intera |

> **Correzione del 2026-09-17 — `set-global-variables` non azzera niente.**
> Lo schema dell'ability dice testualmente: *"Le variabili esistenti non presenti
> nell'array vengono conservate"*. Si passa **solo** la riga da aggiungere o
> modificare, non l'intero elenco. Verificato aggiungendo una variabile a
> Verificato su un progetto reale: le 23 variabili preesistenti erano tutte intatte, zero avvisi.
> Resta 🟠 perché scrive sul design system e non ha revisioni, ma **non** è una
> sostituzione totale. Il presupposto "va ripassato tutto il payload" era sbagliato
> ed è pericoloso al contrario: induce a riscrivere righe che non serviva toccare.

### Codice ed esecuzione
| Ability | Perché |
|---|---|
| `bricks/execute-php` | PHP arbitrario, **nessun sandbox**, accesso a file e database |
| qualsiasi scrittura in Code element (PHP/JS) | esecuzione sul sito |

### Permessi
```
bricks/upsert-builder-capability     bricks/set-builder-role-access
bricks/delete-builder-capability     bricks/list-builder-permissions
```
> Normalmente disattivate. Se le trovi attive, **segnalalo**: un agent che modifica i permessi può aumentare i propri.

### Il flag che disattiva i controlli
`allowBlindWrite: true` — scrive senza verificare il valore attuale.
**Non usarlo mai di iniziativa.** Se serve, chiedi e spiega perché `expectedValue` non è utilizzabile.

---

## 🟠 LIVELLO ARANCIONE — Descrivi e aspetta conferma

Reversibile con fatica, oppure con effetti che si propagano oltre il punto in cui stai lavorando.

### Tocca il design system (nessuna revisione)
```
bricks/create-global-class        bricks/update-global-class
bricks/batch-create-global-classes
bricks/create-theme-style         bricks/update-theme-style
bricks/create-component           bricks/update-component
bricks/extract-component-from-elements
bricks/create-color               bricks/update-color
bricks/create-color-palette       bricks/update-color-palette
bricks/generate-scale-variables   bricks/generate-color-shades
bricks/set-global-variable-categories
bricks/set-style-manager          bricks/set-pseudo-classes
```

**Prima di chiedere conferma su una classe nuova, verifica sempre che non esista già.** Vedi `03-naming.md`.

### Tocca la configurazione del sito
```
bricks/set-global-settings        bricks/set-reading-settings
bricks/set-template-conditions    bricks/set-template-settings
bricks/set-disabled-icon-sets     bricks/set-woo-setup-options
bricks/set-filter-target-query    bricks/save-nav-menu
bricks/update-popup-settings      bricks/reindex-filters
bricks/regenerate-css-files
```

> `set-template-conditions` merita attenzione particolare: una condizione sbagliata fa apparire un template **su tutto il sito**.

### Modifiche multiple in un colpo
```
bricks/apply-site-changeset       bricks/resume-site-changeset
bricks/commit-site-edit-plan      bricks/commit-exact-site-edits
bricks/apply-html-css-page-import bricks/commit-html-css-page-import
bricks/batch-update-elements
```

### Contenuti e media
```
bricks/create-post      bricks/duplicate-post      bricks/create-template
bricks/upload-media     bricks/upload-custom-font-file
bricks/upload-custom-icon
bricks/export-transfer-package con allowSensitiveSettings: true
```

---

## 🟢 LIVELLO VERDE — Procedi, poi riporta cosa hai fatto

Sola lettura, oppure scritture su pagina coperte da revisione.

**Lettura — sempre libera:**
```
get-design-context      get-page-structure       get-page-elements
list-global-classes     list-global-variables    list-components
list-templates          get-template             list-breakpoints
get-theme-styles        list-color-palettes      find-post
find-media              list-revisions           get-revision
get-element-schema      get-builder-guide        preview-dynamic-tag
list-ability-status     get-mcp-version          audit-design-system
checkout-site-repository  resolve-agent-file     list-orphaned-elements
render-elements
```

**Tutte le `preview-*` e `checkout-*` sono verdi per definizione:** non scrivono nulla.

**Scritture su pagina (coperte da revisione):**
```
bricks/add-element      bricks/update-element     bricks/remove-element
bricks/update-element-conditions
bricks/update-element-interactions
bricks/update-form-fields    bricks/update-form-actions
bricks/set-page-settings     bricks/update-filter-element
```

> Verde **non** vuol dire silenzioso. A fine lavoro riporta sempre cosa hai toccato.

---

## Come si chiede una conferma

Formato fisso. Nessun muro di testo, nessuna richiesta vaga.

```
⚠️ CONFERMA RICHIESTA

Azione:      cancellare la classe globale `card-vecchia`
Ability:     bricks/delete-global-class
Livello:     🔴 ROSSO — irreversibile, nessuna revisione
Impatto:     usata su 3 pagine (Home, Servizi, Contatti)
             gli elementi perdono lo stile, non vengono cancellati
Ripristino:  solo da backup del database
Backup:      ⚠️ non risulta un backup recente

Procedo? (serve un sì esplicito)
```

**Regole:**
- una conferma per volta, non liste da approvare in blocco
- l'impatto va **verificato**, non stimato: `get-design-context` con `includeUsage: true`, `checkout-site-repository`, `list-orphaned-elements`
- se non riesci a verificare l'impatto, **dillo**: "non riesco a determinare dove è usata"

---

## Backup: quando pretenderlo

Chiedi conferma che esista un backup **recente del database** prima di:

- qualsiasi azione 🔴 sul design system
- la prima modifica al design system di una sessione
- `set-breakpoints`, `import-transfer-package`, `run-woo-setup`, `commit-site-foundation`
- qualsiasi lavoro su un sito di **produzione**

Se non c'è backup → **non procedere.** Proponi: backup prima, oppure spostarsi su staging.

---

## Ambiente: regole diverse per produzione

| | Locale / Staging | **Produzione** |
|---|---|---|
| 🟢 Verde | libero | libero |
| 🟠 Arancione | conferma | conferma + backup verificato |
| 🔴 Rosso | conferma + backup | **sconsigliato** — proponi staging |
| `execute-php` | conferma esplicita | **mai** |

In dubbio sull'ambiente: **chiedi**. Non dedurlo dal dominio.

---

## Regole che non hanno eccezioni

1. **Mai** `execute-php` senza richiesta esplicita dell'utente, in quella frase, per quello scopo.
2. **Mai** cancellare qualcosa che non hai prima letto.
3. **Mai** `set-page-elements` quando basta `update-element`.
4. **Mai** creare una classe o variabile senza aver cercato un duplicato.
5. **Mai** cambiare i breakpoint. Sono la fondazione di tutto il responsive.
6. **Mai** trattare "fai la pagina X" come autorizzazione a toccare il design system.
7. Se un'azione non è in nessuna lista → **trattala come 🟠**.
