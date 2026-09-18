# 10 · DESIGN SYSTEM CANONICO — Starter kit di Alex

> **Questo è il design system di riferimento per ogni sito nuovo.**
> Vale come default su tutti i progetti, salvo indicazione contraria scritta
> nell'overlay del singolo sito.
>
> Estratto il 2026-09-17 dal progetto che fa da sorgente del kit, via MCP in sola lettura.

---

## Come si propaga a un sito nuovo

> 📦 **Pacchetto pronto:** [`../../starter-kit/bricks-starter-kit.zip`](../../starter-kit/)
> contiene già tutto quanto descritto qui. Vedi `starter-kit/README.md`.

**Non clonare il sito.** In Bricks 2.4 il modo corretto è il pacchetto di trasferimento:

```
Sul sito sorgente     bricks/list-transfer-items  →  bricks/export-transfer-package
Sul sito nuovo        bricks/inspect-transfer-package  →  bricks/import-transfer-package
```

L'import va ispezionato prima (restituisce conflitti e `zipHash`, da passare come
`expectedZipHash`). In caso di conflitto **mantiene l'esistente** salvo `allowOverwrite: true`.

Alternativa dal builder: **Global Import & Export** (novità 2.4), che trasferisce
theme styles, classi, variabili, palette, breakpoint, componenti, template e font.

---

## ⚠️ Cosa NON fa parte del kit: la palette

La palette di brand **non è inclusa di proposito**. Il flusso di Alex è:

1. si crea la palette del nuovo sito all'inizio del progetto
2. le classi si raccordano alla palette nuova **la prima volta che ciascuna serve**

Di conseguenza, su un sito appena seminato è **normale e atteso** che:

- la palette risulti quella stock di Bricks
- l'audit segnali variabili orfane (`--color-1…8`, `--nero`, `--white`)
- molte classi risultino "non referenziate"

**Non sono difetti. Non proporre pulizie.**

`--vh` è un caso a parte: viene impostata da JavaScript a runtime e ha il fallback `1vh`.
Corretta così, non toccarla.

---

## Breakpoint — standard di Alex

```
Logica:  DESKTOP-FIRST  (isMobileFirst: false)
Base:    desktop — 1920px
```

**È la configurazione standard su tutti i siti di Alex.** Fa parte del kit al pari
di classi e variabili, e arriva insieme a quello.

| Key | Etichetta | Larghezza | Note |
|---|---|---|---|
| `desktop` | Desktop | **1920px** | base |
| `laptop` | Laptop | 1279px | |
| `tablet_portrait` | Tablet verticale | 991px | |
| `mobile_landscape` | Mobile orizzontale | 767px | |
| `mobile_portrait` | Mobile verticale | 478px | builder a 380px |

> ⚠️ Chiamare comunque `bricks-list-breakpoints` a inizio sessione: serve a leggere la
> configurazione reale del sito su cui si lavora, non a darla per scontata.
> Vedi `04-breakpoints.md`.

### Come arrivano su un sito nuovo

Dentro il pacchetto di trasferimento e dentro Global Import & Export: **importando il kit
sono già configurati.**

🔴 `bricks/set-breakpoints` resta vietato. I breakpoint di un sito non si impostano e non si
correggono: se non sono quelli giusti, è il kit che non è stato importato — e la soluzione
è importarlo, non modificarli a mano.

---
## Variabili globali (23)

### Generate da scala tipografica

**heading** — prefisso `heading-size-`, min 16px ratio 1.25, max 20px ratio 1.333

**Text** — prefisso `text-size-`, min 16px ratio 1.25, max 20px ratio 1.333

| Variabile | Valore |
|---|---|
| `--heading-size-H1` | `clamp(3.2rem, calc(0.022222222222222216 * (100vw - 36rem) + 3.2rem), 5.6rem)` |
| `--heading-size-H2` | `clamp(2.6rem, calc(0.014814814814814815 * (100vw - 36rem) + 2.6rem), 4.2rem)` |
| `--heading-size-H3` | `clamp(2.2rem, calc(0.009259259259259259 * (100vw - 36rem) + 2.2rem), 3.2rem)` |
| `--heading-size-H4` | `clamp(1.8rem, calc(0.005555555555555554 * (100vw - 36rem) + 1.8rem), 2.4rem)` |
| `--heading-size-H5` | `clamp(1.4rem, calc(0.0027777777777777783 * (100vw - 36rem) + 1.4rem), 1.7rem)` |
| `--heading-size-H6` | `clamp(1.3rem, calc(0.0027777777777777783 * (100vw - 36rem) + 1.3rem), 1.6rem)` |
| `--text-size-B1` | `clamp(1.8rem, calc(0.0018518518518518515 * (100vw - 36rem) + 1.8rem), 2rem)` |
| `--text-size-B2` | `clamp(1.6rem, calc(0.0009259259259259247 * (100vw - 36rem) + 1.6rem), 1.7rem)` |
| `--text-size-B3` | `clamp(1.5rem, calc(0.0009259259259259267 * (100vw - 36rem) + 1.5rem), 1.6rem)` |
| `--text-size-B4` | `clamp(1.3rem, calc(0.0009259259259259247 * (100vw - 36rem) + 1.3rem), 1.4rem)` |

### Variabili piatte

| Variabile | Valore | Ruolo |
|---|---|---|
| `--body-line-height` | `1.5` | line-height testo |
| `--heading-line-height` | `1.2` | line-height titoli |

> ⚠️ **Correzione del 2026-09-17: erano `150%` e `120%`, ora senza unità.**
> Una percentuale di `line-height` si calcola sul font-size dell'elemento che la
> dichiara e **si eredita come valore fisso in px**, non come rapporto. Con il body a
> 15px, `150%` diventava `22.5px` ereditati da tutti i figli: su un testo da 20px
> l'interlinea reale scendeva a **1.13**, stretta e scomoda.
> I titoli non ne soffrivano perché ognuno ridichiara la propria interlinea.
> Senza unità, `1.5` si eredita come rapporto e si ricalcola su ogni corpo.
> **Verificato in pagina: B1, B2, B3 e B4 ora rendono tutti 1.50.**
| `--lt-border-color` | `rgb(221, 221, 221)` | colore bordi |
| `--lt-radius` | `8px` | raggio standard |
| `--lt-radius-medium` | `12px` | raggio medio |
| `--lt-real-vh` | `var(--vh, 1vh)` | altezza viewport reale (JS) |
| `--lt-spacing-large` | `32px` | scala spaziature |
| `--lt-spacing-md` | `18px` | scala spaziature |
| `--lt-spacing-md2` | `24px` | scala spaziature |
| `--lt-spacing-micro` | `8px` | scala spaziature |
| `--lt-spacing-nano` | `.3rem` | scala spaziature |
| `--lt-spacing-small` | `12px` | scala spaziature |
| `--section-standard-padding` | `60px` | padding sezioni |

> ⚠️ **Incoerenza nota:** heading e text usano il generatore di scale con categoria
> dedicata; le 6 spaziature (`nano`→`large`) sono variabili piatte senza categoria,
> pur essendo chiaramente una scala. Da uniformare se un giorno si rifà il kit.

---
## Classi globali (64)

Categorie in uso: **Layout** e **Font & Text**. Il resto è senza categoria.


### Layout e utility (22)

| Classe | ID | Note |
|---|---|---|
| `lt-bg-image` | `fmieia` | — |
| `lt-black-text` | `tlrrhx` | — |
| `lt-border-image` | `cbnpna` | non ancora usata |
| `lt-container-full` | `xoanys` | non ancora usata |
| `lt-flex` | `jswtfm` | non ancora usata |
| `lt-flex-col` | `vhqmle` | non ancora usata |
| `lt-flex-row` | `nzysrk` | non ancora usata |
| `lt-items-center` | `sbfusj` | non ancora usata |
| `lt-justify-center` | `vnxhsp` | non ancora usata |
| `lt-overlay-entrata` | `iyzfpy` | — |
| `lt-padding-contenitori` | `zlpini` | — |
| `lt-padding-t-b` | `utaufy` | — |
| `lt-pineapple-radius` | `htyism` | non ancora usata |
| `lt-section-container` | `yvgjqs` | — |
| `lt-section-container-90` | `gxjtaa` | non ancora usata |
| `lt-section-container-95` | `efwhte` | non ancora usata |
| `lt-section-container-full` | `dduajq` | non ancora usata |
| `lt-section-container-no-anim` | `pqldaj` | non ancora usata |
| `lt-section-container-text` | `byzwic` | non ancora usata |
| `lt-text-center` | `radvlf` | non ancora usata |
| `lt-white-text` | `dwlsmk` | — |
| `lt-width-full-important` | `rxkcwx` | — |

### Tipografia (11)

| Classe | ID | Note |
|---|---|---|
| `body-fake-h1` | `rmmygz` | — |
| `body-fake-h2` | `smcykn` | — |
| `body-fake-h3` | `nkuuui` | — |
| `body-fake-h4` | `dsztdt` | — |
| `body-fake-h5` | `aqpnxy` | — |
| `body-fake-h6` | `mhbkqt` | — |
| `body-intro-text-B1` | `dpzkru` | — |
| `body-intro-text-B2` | `npqeov` | — |
| `body-medium-text-B3` | `ukohyz` | — |
| `body-small-text-B4` | `razpfu` | — |
| `header-small` | `kopqjz` | non ancora usata |

### Animazioni e transizioni (11)

| Classe | ID | Note |
|---|---|---|
| `entrata` | `wkbwju` | — |
| `lt-anim-fadein` | `rfhfdz` | non ancora usata |
| `lt-anim-fadeinup` | `oevvvc` | non ancora usata |
| `lt-anim-pineapple` | `vwrsip` | vuota; non ancora usata |
| `lt-anim-scale` | `olxkuw` | non ancora usata |
| `lt-transition-300ms` | `ppgieq` | vuota |
| `lt-transition-450ms` | `wjlbip` | non ancora usata |
| `lt-transition-600ms` | `osazwm` | non ancora usata |
| `scrolling-text` | `ucpmyy` | non ancora usata |
| `title-anim` | `nhjqll` | vuota; **nome duplicato**; non ancora usata |
| `title-anim` | `szqdlc` | vuota; **nome duplicato**; non ancora usata |

### Spaziature colonne (6)

| Classe | ID | Note |
|---|---|---|
| `lt-col-spacing-large` | `sydyto` | non ancora usata |
| `lt-col-spacing-md` | `rirjgj` | — |
| `lt-col-spacing-md2` | `hqmmva` | non ancora usata |
| `lt-col-spacing-micro` | `okigns` | non ancora usata |
| `lt-col-spacing-nano` | `blbopw` | non ancora usata |
| `lt-col-spacing-small` | `ejbdmd` | non ancora usata |

### Icone (4)

| Classe | ID | Note |
|---|---|---|
| `icn-footer` | `oqlrzy` | non ancora usata |
| `icon-image-menu` | `kgdvkf` | non ancora usata |
| `icona-navbar-menu` | `aaqokq` | — |
| `icons-contacts` | `puulux` | non ancora usata |

### Footer alternativo (ft-6) (3)

| Classe | ID | Note |
|---|---|---|
| `ft-6__col` | `tvauks` | non ancora usata |
| `ft-6__heading` | `bmkwfh` | non ancora usata |
| `ft-6__menu` | `webmmk` | non ancora usata |

### Blocchi funzionali (7)

| Classe | ID | Note |
|---|---|---|
| `bullet` | `yokqnp` | CSS custom |
| `colonne-contatti` | `njtgmb` | non ancora usata |
| `div-navbar-menu` | `bovagf` | — |
| `highlight-container` | `ergzxy` | non ancora usata |
| `hyphens` | `yrxpyq` | non ancora usata; CSS custom |
| `rail` | `tjzczl` | non ancora usata |
| `text-item` | `hamegy` | non ancora usata; CSS custom |

---

## Debito noto del kit (accettato, non da correggere)

| Cosa | Nota |
|---|---|
| `title-anim` esiste due volte | ID `nhjqll` e `szqdlc`, entrambe senza impostazioni |
| 4 classi vuote | definite ma senza impostazioni, non producono CSS |
| Naming misto IT/EN | `icn-footer` vs `icona-navbar-menu` vs `icon-image-menu` |
| `ft-6__*` mai adottato | footer alternativo che viaggia con ogni copia |
| Nessuna classe per i bottoni | i bottoni non hanno una classe globale dedicata |

**Alex ha deciso di mantenere tutto.** Questo elenco serve a non ripetere gli stessi
schemi in classi nuove, non come lista di cose da sistemare.

---

## Regole per chi ci lavora sopra

1. **Prima cerca, poi crea.** `bricks-list-global-classes` — l'elenco completo, non
   `get-design-context` che tronca a 100.
2. **Usa le variabili del kit**, mai valori fissi. Vedi `02-design-system.md`.
3. **Ogni classe nuova rispetta BEM** — `blocco__elemento--modificatore`, un solo livello
   di `__`, modificatore mai da solo. Uniche famiglie escluse: utility `lt-` e tipografia
   `body-`. Regole complete in `03-naming.md`.
   Le classi storiche del kit che non sono BEM restano come sono, ma **non fanno precedente**.
4. **Non cancellare niente del kit** senza richiesta esplicita di Alex.
5. **Crea la palette all'inizio** del progetto, prima di costruire.
