# 90 · OVERLAY SITO — webagencyalba.it

> Compilato leggendo il sito vero via MCP (`bricks/list-breakpoints`, `get-design-context`,
> `list-global-variables`, `list-color-palettes`, `list-components`, `list-templates`,
> `audit-design-system`). Sola lettura — nessuna scrittura effettuata.

---

## Identità

```
Sito:            webagencyalba.it
URL:             https://webagencyalba.it
Ambiente:        SITO DI PROVA per sperimentare con l'AI (Alex, 2026-09-17)
Backup:          ⚠️ NON CHIEDERLO. Alex ha dichiarato il 2026-09-17 che questo è un
                 sito di prova: non c'è nulla da proteggere. Le conferme sulle azioni
                 🟠/🔴 restano, la domanda sul backup no — non riproporla a ogni passo.
Stato:           attivo
```

✅ **STAGING** — valgono le regole normali di `00-SAFETY.md`, senza i rinforzi previsti per la produzione.
Le conferme sulle azioni 🟠/🔴 al design system restano comunque obbligatorie: staging non significa senza revisioni.

> **Natura del sito:** è uno **starter kit** portato da progetti precedenti. La palette di brand
> non esiste ancora: viene creata all'inizio di ogni nuovo sito, e le classi vengono raccordate
> alla palette nuova man mano che servono. Le variabili "orfane" segnalate dall'audit sono
> quindi **attese**, non un difetto. Vedi `webagencyalba-it-classi.md`.

---

## 1. Breakpoint ⚠️

```
isMobileFirst:   false
Logica:          DESKTOP-FIRST (max-width)
Base:            desktop — 1920px (standard di Alex, vedi 10-DESIGN-SYSTEM.md)
```

| Key | Etichetta | Larghezza | Custom |
|---|---|---|---|
| desktop (base) | Desktop | 1920px | sì, modificato dal default |
| laptop | Laptop | 1279px | sì, breakpoint aggiunto ex-novo |
| tablet_portrait | Tablet verticale | 991px | no |
| mobile_landscape | Mobile orizzontale | 767px | no |
| mobile_portrait | Mobile verticale | 478px (widthBuilder 380px) | modificato dal default |

Nota: configurazione **standard di Alex**, identica su tutti i suoi siti. Desktop-first: si parte da 1920 e si scende. 🔴 Non modificarla.

---

## 2. Lingua e convenzione

```
Lingua dei nomi:     mista italiano/inglese, incoerente
Convenzione:         prefissi multipli non unificati: lt- (opooqy), body- (ltlcjx),
                     ft-6__ (BEM, footer), icon-/icona- (mescolati)
Categorie in uso:    2 categorie di classi con ID opachi (opooqy, ltlcjx) — nomi
                     leggibili delle categorie non risolvibili dalle ability lette
```

Deviazioni note dalla convenzione (da non imitare):
```
- "icon-footer" vs "icona-navbar-menu" vs "icon-image-menu" — inglese e italiano mescolati
  per lo stesso concetto (icona)
- due classi diverse con lo STESSO nome "title-anim" (id nhjqll e szqdlc) — collisione di
  naming, probabilmente un duplicato mai ripulito
- "ft-6__col" / "ft-6__heading" / "ft-6__menu" usano BEM con un numero (6) che non ha
  riscontro altrove nel sistema — verosimilmente residuo di un footer alternativo mai usato
  (infatti risultano "unused" nell'audit)
```

---

## 3. Variabili

**Scale disponibili (via scale generator, categoria tipografica):**
```
Heading (H1-H6):  clamp() fluido, min 16px/1.25 - max 20px/1.333, baseline H6
Text (B1-B4):     clamp() fluido, stessa curva, baseline B3
```

**Variabili "piatte" (fuori scala, nessuna categoria):**
| Variabile | Valore | Uso |
|---|---|---|
| heading-line-height | 120% | line-height titoli |
| body-line-height | 150% | line-height testo |
| section-standard-padding | 60px | padding sezioni |
| lt-radius | 8px | ⚠️ non referenziata da nessuna parte (audit: unused) |
| lt-radius-medium | 12px | ⚠️ non referenziata da nessuna parte (audit: unused) |
| lt-border-color | rgb(221,221,221) | ⚠️ non referenziata (audit: unused) |
| lt-spacing-nano | .3rem | spacing |
| lt-spacing-micro | 8px | spacing |
| lt-spacing-small | 12px | spacing |
| lt-spacing-md | 18px | spacing |
| lt-spacing-md2 | 24px | spacing |
| lt-spacing-large | 32px | spacing |
| lt-real-vh | var(--vh, 1vh) | ⚠️ non referenziata (audit: unused), e occhio: il fallback
  `--vh` stesso è un orfano CSS (vedi §9 Debito tecnico) |

⚠️ **Incoerenza strutturale**: heading e text usano lo scale-generator con categoria dedicata; lo spacing (nano→large, 6 step, chiaramente una scala) è definito come variabili piatte senza categoria/scala. Non è un errore bloccante ma è un'inconsistenza nel come il design system è costruito.

---

## 4. Colori

| Palette | Colori | Ruolo |
|---|---|---|
| Default (unica palette) | 18 colori | palette stock non ancora sostituita (grey-100…900, yellow, amber, orange, deep-orange, red, purple, blue, light-blue, sky-blue, green, light-green, lime) |

ℹ️ **Nessun colore di brand: è atteso.** Questo sito è la fonte dello starter kit, e la palette
per definizione non ne fa parte — viene creata all'inizio di ogni nuovo progetto. Non è una
dimenticanza e non va "sistemata". Vedi `10-DESIGN-SYSTEM.md`.

Colori primari da usare:
```
Non determinabile: nessun colore è marcato come "primario" nella palette, che è
generica/stock.
```

---

## 5. Classi globali per famiglia

64 classi totali (elenco completo, sotto la soglia di troncamento di 100).

**Layout / container** (categoria `opooqy`, prefisso `lt-`)
```
lt-section-container, lt-section-container-no-anim*, lt-section-container-95*,
lt-section-container-90*, lt-section-container-text*, lt-section-container-full*,
lt-container-full*, lt-width-full-important, lt-padding-t-b, lt-padding-contenitori,
lt-col-spacing-{nano,micro,small,md,md2,large}*, lt-flex*, lt-flex-row*, lt-flex-col*,
lt-items-center*, lt-justify-center*, lt-text-center*, lt-pineapple-radius*,
lt-border-image*, lt-bg-image, lt-overlay-entrata
```
(* = risulta "unused" nell'audit — vedi §11)

**Tipografia** (categoria `ltlcjx`, prefisso `body-`/`header-`)
```
body-fake-h1 … body-fake-h6, body-intro-text-B1, body-intro-text-B2,
body-small-text-B4, body-medium-text-B3, header-small*
```

**Bottoni**
```
Nessuna classe globale dedicata ai bottoni trovata.
```

**Animazioni**
```
lt-anim-fadein*, lt-anim-fadeinup*, lt-anim-pineapple*, lt-anim-scale*,
lt-transition-300ms, lt-transition-450ms*, lt-transition-600ms*,
title-anim (×2 — nomi duplicati, vedi §2), entrata, scrolling-text*
```

**Blocchi funzionali**
```
bullet, icn-footer*, icona-navbar-menu, div-navbar-menu, colonne-contatti*,
ft-6__col*, ft-6__heading*, ft-6__menu*, icons-contacts*, icon-image-menu*,
rail*, text-item*, highlight-container*, hyphens*
```

---

## 6. Componenti

| Componente | Cosa fa | Proprietà | Usato in |
|---|---|---|---|
| bullet | elemento testo+emoji riusabile | Text, Emoji o altro | elementCount: 3 nel catalogo componenti, **ma `audit-design-system` lo segna a zero istanze** — incoerenza tra le due letture, da verificare a mano prima di considerarlo orfano |
| Back To Top Button | pulsante torna-su; il JS è nel child theme (non nel componente) | nessuna | 3 elementi |
| Whatsapp float | pulsante flottante WhatsApp | nessuna | 2 elementi |

---

## 7. Theme styles

| ID | Etichetta | Condizioni |
|---|---|---|
| tema_sito_v1 | tema_sito_v1 | any (si applica sempre) |
| alex_post_theme | Alex post theme | postType = post |

Nota naming: "Alex post theme" — nome con riferimento personale, non descrittivo della funzione. Da non imitare per nuovi theme style.

---

## 8. Template

| Tipo | Nome | Condizioni |
|---|---|---|
| Header | Header (multilingua) — **pubblicato, attivo** | nessuna condizione esplicita |
| Header | Header (VERSIONE CON PULSANTI MOBILE) — draft | nessuna |
| Footer | Footer | nessuna |
| Section | Entrata con immagine ottimizzata velocita | nessuna |

⚠️ **Nessun template Archive/Single/Search trovato.** Solo header, footer e una section. Se il sito ha archivi o pagine singole personalizzate, non passano da template Bricks dedicati — da verificare con l'utente prima di assumere che "non serva".

---

## 9. Regole specifiche di questo sito

```
- Base breakpoint spostata a 1920px con "laptop" (1279px) aggiunto come breakpoint
  custom: non assumere che valga la logica default-Bricks vista su altri siti.
- Il JS del "Back To Top Button" vive nel child theme, non nel componente stesso —
  se si tocca quel componente, il child theme va controllato in parallelo.
```

### Token e classe display — SOLO QUESTO SITO (2026-09-17)

Aggiunti per dare all'hero il salto di scala del riferimento (einar.qodeinteractive.com).
**Non fanno parte del kit** in `10-DESIGN-SYSTEM.md`: se servono altrove, vanno ricreati.

| Cosa | Nome | Valore |
|---|---|---|
| Variabile | `--heading-size-display` (id `c1c120`) | `clamp(4rem, calc(0.0277777778 * (100vw - 36rem) + 4rem), 7rem)` → 40px→70px |
| Classe | `body-fake-display` (id `27a29b`) | font-size display, peso 600, `letter-spacing: -0.05em`, line-height dal kit |

La variabile sta nella **categoria `heading`** (`ygngdi`) per comparire accanto a
`heading-size-H1…H6` nel builder.

> ⚠️ **Non è uno step della scala.** La scala di quella categoria ha `isManual: true` e
> i suoi `manualValues` elencano solo H1…H6. `--heading-size-display` è una riga
> aggiuntiva assegnata alla stessa categoria. **Se un giorno si rigenera la scala
> `heading`, questa variabile può sparire.** Non si poteva fare diversamente: la
> scrittura delle categorie è bloccata di proposito dalle ability
> (`generate-scale-variables` restituisce solo anteprime, `saved: false`).

**Uso:** si applica `body-fake-display` al posto di `body-fake-h1` sul titolo che deve
dominare la pagina. Vale solo per il titolo principale di una pagina, non per i titoli
di sezione.

**Come è stato scelto il massimo (7rem).** Provati tre valori misurando le righe nella
colonna al 50% dell'hero:

| Massimo | Corpo desktop | Righe | Esito |
|---|---|---|---|
| 11rem | 110px | 4 | troppo: riempie la colonna e soffoca il resto |
| 8rem | 80px | 3 | l'ultima riga resta "visite.", orfana |
| **7rem** | **70px** | **2** | ✅ due righe bilanciate, +25% sul vecchio H1 |

Verificato su tutti i breakpoint del sito (1812 / 1279 / 991 / 767 / 478 / 375):
da 70px a 40px, 2–3 righe, **nessun overflow orizzontale**.

---

## 10. Da non toccare

```
- Header "Header (VERSIONE CON PULSANTI MOBILE)" (draft, id 9): sembra una versione
  alternativa tenuta di proposito, non necessariamente da eliminare.
- TUTTE le 64 classi globali: Alex ha deciso il 2026-09-17 di mantenerle integralmente,
  comprese le 4 vuote e le ~40 non referenziate. Fanno parte dello starter kit e vengono
  attivate man mano. NON proporre pulizie, NON segnalarle come debito da smaltire.
- Le variabili "orfane" (--color-1…8, --nero, --white): attese finché non viene creata
  la palette di brand del sito. --vh è impostata da JS, corretta così.
```

---

## 11. Debito tecnico noto

Da `bricks/audit-design-system` (12 errori, 0 warning, 45 info):

| Problema | Dove | Note |
|---|---|---|
| Riferimenti CSS a variabili inesistenti | `--color-1` … `--color-6`, `--color-8`, `--nero`, `--white`, `--vh` | 10 variabili referenziate nel CSS ma mai definite nel design system — probabile refactor a metà (rinominate/cancellate senza aggiornare gli usi) |
| Classi globali orfane referenziate da elementi | id `qynwjd`, `rrwyqc` | elementi puntano a classi che non esistono più |
| 28 classi globali definite ma inutilizzate | vedi elenco §5 (marcate *) | non cancellare di iniziativa — solo segnalare |
| 4 variabili globali inutilizzate | lt-radius, lt-radius-medium, lt-border-color, lt-real-vh | idem |
| Componente "bullet" senza istanze secondo l'audit, ma elementCount:3 nel catalogo componenti | vedi §6 | incoerenza tra due fonti, verificare a mano |
| ~~Palette non personalizzata~~ | — | **non è debito**: la palette è esclusa dal kit di proposito, si crea per ogni sito |
| Nessuna categoria/scala per le variabili di spacing | design system | incoerente con l'approccio usato per heading/text |
| Naming misto IT/EN e un duplicato di nome (`title-anim` ×2) | classi globali | vedi §2 |

**Non "sistemare" nulla di questo di iniziativa — è tutto materiale da riportare e decidere con l'utente.**

---

## 12. Stack

```
Plugin rilevanti:    "Essential Cervido Add-on" (fornisce almeno il componente
                     "Back To Top Button")
Slider:              non determinato dalle ability lette
Custom fields:       non determinato dalle ability lette
SEO:                 non determinato dalle ability lette
WooCommerce:         non verificato in questa sessione (ability get-woo-setup-status
                     non richiamata — non era nell'elenco del template)
```
