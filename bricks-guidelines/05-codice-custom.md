# 05 · CODICE CUSTOM — CSS, JS, PHP

---

## L'ordine delle preferenze

```
1. CONTROLLI BRICKS       sempre, quando bastano
2. CSS su classe globale  quando serve CSS vero, riutilizzabile
3. CSS sull'elemento      solo per il caso unico
4. CODE ELEMENT (JS)      quando serve comportamento
5. PHP                    ultima risorsa, con conferma
```

**Non scrivere CSS per fare ciò che un controllo fa già.** Diventa invisibile nel builder e nessuno lo trova più.

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

## CSS

**Dove metterlo, in ordine:**

| Dove | Quando |
|---|---|
| Classe globale (Class Manager) | stile riutilizzabile — **la scelta giusta quasi sempre** |
| Theme style | comportamento di default del sito |
| Elemento | caso unico e irripetibile |
| Settings > Custom code | solo globale vero (reset, font-face) |

> In 2.4 il Custom CSS di una classe globale si modifica direttamente dal Class Manager.

**Regole:**
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
