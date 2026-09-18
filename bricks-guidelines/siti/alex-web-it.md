# alex-web.it — ⚠️ SITO LEGACY, NON È IL RIFERIMENTO

> **Leggi questo prima di usare alex-web.it come esempio di qualcosa.**

---

## Stato

```
Sito:        alex-web.it (sito personale di Alex)
Stato:       LEGACY — costruito circa un anno fa
Ruolo:       ❌ NON è il design system di partenza per i siti clienti
```

**Alex ha dichiarato esplicitamente** che questo sito non è costruito bene e non riflette il suo modo di lavorare attuale. Il design system di riferimento è un altro e verrà documentato a parte.

---

## Regole per questo sito

1. **Non usarlo come modello.** Le sue convenzioni non vanno imitate né estese ad altri progetti.
2. **Non proporre di sistemarlo** se non è quello che è stato chiesto.
3. Se ci si lavora sopra: **si segue ciò che c'è**, senza importare convenzioni da fuori.
4. Non generare l'overlay `90-…` da questo sito.

---

## Cosa c'è dentro (rilevato, non approvato)

```
108 classi globali · 13 variabili · 2 theme styles
2 palette (26 colori) · 0 componenti
Breakpoint: DESKTOP-FIRST, base 1920px
```

Breakpoint: `desktop 1920` (base) → `pc 1279` (custom) → `tablet_portrait 991` → `mobile_landscape 767` → `mobile_portrait 478`

### Problemi rilevati

- **Almeno 5 convenzioni di naming** convivono: `lt-*`, `body-*`, `an-*`, BEM `ft-6__col`, italiano descrittivo (`sottotitolo`, `immagine-chisiamo`, `colonne-contatti`)
- `title-anim` esiste **due volte** con ID diversi (`nhjqll`, `szqdlc`), entrambe senza impostazioni
- **~12 classi senza alcuna impostazione**: `glass-card`, `gradient-bg`, `article`, `step`, `fq-title`, `fq-content`, `faq-answer`, `lt-anim-pineapple`, `left-slider-button`, `step-funziona-text`
- Typo mai corretto: **`footeer-space`**
- Variabili quasi tutte `lt-*`, tranne `heading-line-height`, `body-line-height`, `section-standard-padding`
- Categorie quasi tutte vuote: solo `zozwnv` (tipografia) e `brohvi` (alcune utility)
- **Zero componenti** — tutto a classi

> Questo elenco serve a **non ripetere questi errori altrove**, non come lista di cose da correggere qui.

---

## Se un giorno si rifà

Ricostruire dal design system nuovo seguendo `02-design-system.md` e `03-naming.md`, non migrare l'esistente.
