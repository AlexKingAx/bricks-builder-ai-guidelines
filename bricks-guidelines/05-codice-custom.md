# 05 · CODICE CUSTOM — CSS, JS, PHP

---

## L'ordine delle preferenze

```
1. CONTROLLI BRICKS         sempre, quando bastano — anche per breakpoint e pseudo-classi
2. CLASSE IN PIÙ            un modificatore BEM che imposta i controlli (vedi sotto)
3. CONDIZIONI / STRUTTURA   condizioni Bricks o elementi separati al posto di regole sui figli
4. CSS RAW (_cssCustom)     🛑 solo con permesso chiesto PRIMA — vedi guardrail
5. CODE ELEMENT (JS)        quando serve comportamento
6. PHP                      ultima risorsa, con conferma
```

**Non scrivere CSS per fare ciò che un controllo fa già.** Diventa invisibile nel builder e nessuno lo trova più.

---

## 🛑 CSS raw (`_cssCustom`) — guardrail

> **Niente CSS raw senza permesso esplicito, chiesto PRIMA di scriverlo.**
> Vale ovunque si scriva CSS a mano: `_cssCustom` su classe globale, `_cssCustom` su elemento,
> `css.stylesheet` del theme style, Code element, Custom code di sito.
> Un permesso vale per **quella** regola, non per le successive.

### Perché esiste questa regola

Sulla pagina Confronto di un sito ho scritto regole raw dentro classi globali che agivano
sui figli: `:nth-child(even)` per la zebra, `:not(:first-child)` per nascondere un titolo,
selettori discendenti con `.brxe-text-basic` ripetuto per vincere sulla specificità.
Nel builder erano invisibili: aprendo l'elemento non c'è nessun campo che spieghi quel
comportamento, e un controllo "non fa niente" perché una regola altrove lo sovrascrive.
Tutto era ottenibile con i campi di Bricks e **una classe in più sull'elemento**. Il costo
è stato un audit e la riscrittura di quattro classi.

### Quando è ammesso (devono valere tutte)

1. La proprietà o funzione **non ha nessun controllo** in Bricks. Verificalo davvero:
   `get-element-schema` con `controlKeys`, e prova anche la variante per breakpoint
   (`:mobile_landscape`) e per pseudo-classe (`:hover`). Esempi tipici che *non* hanno
   controllo: `color-mix()`, `line-clamp`, `@container`, `::before`/`::after` con contenuto,
   `:has()`, `@keyframes`, `text-wrap: balance`.
2. Non si risolve con la checklist qui sotto.
3. Il caso lo richiede necessariamente. Non basta che sia più veloce o più comodo.

### Non sono motivi validi

Layout, flex/grid, colori, sfondi, bordi, ombre, tipografia, spaziature, posizione,
dimensioni, visibilità per breakpoint, hover semplice: **hanno tutti un controllo**.
Se ti sembra che manchi, hai cercato il campo sbagliato o il breakpoint sbagliato.

### Vietato in ogni caso

- **Selettori che agiscono sui figli** dentro `_cssCustom` di una classe (`.a .b`, `>`,
  `:nth-child`, `:first-child`, `:not()`): l'effetto compare su elementi che non hanno
  traccia della regola. Se un figlio deve avere uno stile, gli si dà una classe.
- **Specificità forzata** (`.brxe-text-basic` aggiunto solo per vincere) e `!important`
  senza motivo scritto: sono il sintomo che le classi sono sbagliate.
- **Stile statico scritto in JS.** Se un elemento serve a JS (una barra sticky, un
  pannello), la struttura e lo stile vivono nel builder come elemento con le sue classi;
  il JS imposta solo i valori dinamici (`top`, `transform`, larghezze, `display`).

### Checklist prima di chiedere il permesso (in quest'ordine)

| # | Prova | Al posto di |
|---|---|---|
| 1 | Campo nativo, anche per breakpoint e pseudo-classe | qualsiasi CSS |
| 2 | Classe già esistente (`bg-soft`, `lt-*`…) | classe nuova + CSS |
| 3 | **Modificatore BEM sull'elemento stesso** (`riga--alt`, `foto--contain`, `tag--static`): una classe in più con campi nativi | `:nth-child`, `:first-child`, `:not()`, selettori sui figli |
| 4 | **Condizioni Bricks** (es. indice del loop) | CSS per mostrare/nascondere in base alla posizione |
| 5 | **Struttura**: elemento separato, o due elementi con classi diverse | regole discendenti |

Se dopo i cinque punti serve ancora CSS raw → chiedi.

### Come si chiede

```
🛑 Mi serve CSS raw
Dove:        classe `xxx` / elemento `yyy`
Cosa:        <proprietà o funzione senza controllo>
Perché:      <cosa manca in Bricks, e cosa ho già provato nei punti 1–5>
Testo:       <il CSS esatto che scriverei>
Senza CSS:   <cosa si perde con l'alternativa migliore>
Approvi?
```

### Se viene approvato

- una regola per volta, con il selettore **solo sulla classe o sull'elemento stesso**;
- solo `var(--token)`, mai valori fissi (vedi `02-design-system.md`);
- un commento che dice **perché** serve, non cosa fa;
- lo riporti nel report finale, in una riga "CSS raw autorizzati";
- lo annoti nell'overlay del sito (`siti/<sito>.md`, sezione debito tecnico: dove sta e perché),
  così chi lavora dopo lo trova.

### Audit di chiusura

Prima di scrivere "fatto": rileggi classi ed elementi toccati e cerca `_cssCustom`.
Devono esserci **solo** quelli autorizzati. Ricorda che le classi globali **non validano**
le chiavi: una proprietà inventata (es. `_gap` su un blocco) viene salvata senza errore e
poi non fa niente. Controlla le chiavi con `get-element-schema` prima di scriverle.

---

## CSS Sync — novità 2.4, da capire bene

Funzione **opt-in e sperimentale**: `Bricks > Settings > Builder > CSS Sync`.

Tiene sincronizzato il Custom CSS con i controlli di stile, **in entrambe le direzioni**:

- scrivi CSS → i controlli si aggiornano
- cambi un controllo → il valore viene riscritto nel CSS

Rispetta breakpoint attivo e pseudo-classe. Preserva shorthand scritti a mano, variabili composte e punti e virgola finali.

### La regola di precedenza

> **Se una proprietà è definita sia nel Custom CSS che nei controlli, vince il Custom CSS.**

Da ricordare quando un controllo "non fa niente": probabilmente è sovrascritto dal CSS.

### Prima di usarlo

Verifica se è attivo (`get-global-settings`). **Attivarlo è 🟠**: cambia il comportamento del builder su tutto il sito.

---

## CSS — dove va, quando è autorizzato

Vale **solo dopo** il permesso descritto nel guardrail qui sopra. Gli stili normali non sono
CSS: si impostano con i controlli, su una classe globale.

| Dove | Quando |
|---|---|
| Controlli Bricks su classe globale | **sempre**, per ogni stile riutilizzabile |
| Theme style (controlli) | comportamento di default del sito |
| `_cssCustom` di una classe | solo se autorizzato, per una proprietà senza controllo |
| `_cssCustom` di un elemento | solo se autorizzato, caso unico e irripetibile |
| Settings > Custom code | solo globale vero (reset, font-face), autorizzato |

> In 2.4 il Custom CSS di una classe globale si modifica direttamente dal Class Manager.
> Proprio per questo è facile scriverlo senza accorgersene: è il motivo del guardrail.

**Regole (per il CSS autorizzato):**
- sempre `var(--token)`, mai valori fissi — vedi `02-design-system.md`
- `!important` solo se documentato con il motivo nel commento
- niente selettori che escono dall'elemento (`body > div > .cosa`)
- rispetta la logica dei breakpoint del sito — vedi `04-breakpoints.md`

---

## SVG

- colore via `fill` / `stroke`, mai colori inline dentro il markup
- usa `currentColor` quando l'icona deve seguire il testo
- `viewBox` sempre presente, `width`/`height` fissi mai
- upload icona: **massimo 1 MiB** di markup
- l'upload SVG dipende dai permessi del sito: se fallisce, è permessi, non markup

---

## JavaScript

**Requisito:** capability `unfiltered_html`. Senza, la scrittura viene rifiutata — non è un bug.

**Regole:**
- niente librerie esterne senza chiedere: Bricks include già Swiper, Splide, PhotoSwipe
- aggancia gli eventi Bricks invece di aspettare a caso con `setTimeout`
- attenzione a **Instant Navigation** (attiva di default in 2.4): la pagina cambia **senza ricaricare**, quindi il codice legato a `DOMContentLoaded` non rigira. Va riattaccato agli eventi di Bricks.
- ogni scrittura in un Code element è 🟠 — vedi `00-SAFETY.md`

---

## PHP

🔴 **Rosso assoluto.**

- serve `BRICKS_ENABLE_PHP_ABILITIES` definita in PHP (non attivabile da wp-admin)
- serve `manage_options` + capability Bricks "Execute code" + esecuzione codice attiva + firme non bloccate
- **nessun sandbox**: può leggere, modificare e cancellare qualsiasi file raggiungibile da PHP

**Mai di iniziativa.** Solo su richiesta esplicita dell'utente, in quella frase, per quello scopo, e solo in locale o staging.

Per lo sviluppo esistono due costanti che firmano il codice automaticamente:

```php
define( 'BRICKS_DANGEROUSLY_AUTO_SIGN_CODE_ON_BUILDER_SAVE', true );
define( 'BRICKS_DANGEROUSLY_AUTO_SIGN_CODE_ON_BUILDER_RERENDER', true );
```

Il prefisso `DANGEROUSLY_` è letterale. Solo ambienti fidati, mai produzione.

---

## Commenti

Lo stile dei commenti e della documentazione del codice segue le preferenze personali definite nella skill **`alex-config`**. Caricala prima di scrivere codice invece di improvvisare uno stile.

Regola minima, sempre valida: **commenta il perché, non il cosa.**

```css
/* ❌ imposta il padding a 24px */
/* ✅ padding allineato al container per far combaciare le colonne su tablet */
```

---

## HTML/CSS → Bricks

Pipeline in tre tempi:

```
convert-html-css-to-bricks-data   (legge, non salva)
preview-html-css-page-import      (render, non salva)
commit / apply                    (scrive)
```

**L'ordine di salvataggio è obbligatorio**, sbagliarlo frammenta il design system:

1. riconcilia `skipped_global_variables` con le variabili esistenti omonime
2. salva le `global_classes` **una volta sola** con `batch-create-global-classes`
3. **poi** salva l'albero degli elementi

**Limite: 2 MiB** di HTML + CSS combinati.

**Se torna un risultato parziale** — contenuto omesso per permessi mancanti — non insistere: riporta all'utente cosa è stato escluso e perché.
