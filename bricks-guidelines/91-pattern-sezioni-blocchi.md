# 91 · PATTERN DI SVILUPPO SEZIONI E BLOCCHI

> **Stato: UFFICIALE.** Validato da Alex il 2026-09-18. Vale su tutti i suoi siti.
>
> Ricavato leggendo il DOM di **deltatraslochi.com** e confrontandolo con il kit in
> `10-DESIGN-SYSTEM.md`. Descrive **come si costruisce una sezione**: scheletro,
> larghezze, spaziature, quando creare una classe.

---

## 1. Lo scheletro di una sezione

La struttura ricorrente, identica in 8 sezioni su 10:

```
section.brxe-section.lt-padding-t-b
└── div.brxe-container.lt-section-container
    └── contenuto
```

**Tre livelli, tre responsabilità separate:**

| Livello | Classe | Cosa fa | Cosa NON fa |
|---|---|---|---|
| `section` | `lt-padding-t-b` | respiro verticale (60px sopra e sotto) | non limita la larghezza |
| `container` | `lt-section-container` | larghezza e centratura (1400px, max 95%) | non mette padding verticale |
| contenuto | `lt-col-spacing-*` | distanza fra i figli, via `gap` | non usa margini |

> La sezione non conosce la larghezza. Il container non conosce il respiro.
> Tenerli separati è il motivo per cui il pattern regge su tutte le pagine.

### Quando NON si mette `lt-padding-t-b`

La classe è il respiro **standard** (60px, da `--section-standard-padding`).
Quando una sezione ne vuole meno, o non ne vuole affatto — tipicamente quelle con
fondo pieno — **la classe non si mette**: si scrive il padding direttamente
sull'elemento, con il valore che serve.

```
respiro standard   →  classe lt-padding-t-b
respiro diverso    →  niente classe, padding sull'elemento
nessun respiro     →  niente classe
```

**Non si mette la classe per poi sovrascriverla.** O la si usa, o non la si usa.

---

## 2. Le larghezze

| Classe | Larghezza | Quando |
|---|---|---|
| `lt-section-container` | 1400px · max 95% | default, quasi sempre |
| `lt-section-container-text` | 1000px | blocchi di solo testo, per la leggibilità |
| `lt-section-container-full` | 100% | caroselli, immagini a tutta larghezza |
| `lt-section-container-95` / `-90` | 95% / 90% | varianti disponibili, poco usate |

> ⚠️ **La direzione flex di `lt-section-container` cambia da sito a sito.**
> Su deltatraslochi.com è `row`; su webagencyalba.it è `column`. Non darla per scontata:
> se la sezione ha due colonne affiancate, **imposta `_direction` esplicitamente
> sull'elemento**, altrimenti su un sito funziona e sull'altro le colonne si impilano.

---

## 3. Le spaziature: sempre `gap`, mai margini

La distanza fra elementi si dichiara **sul genitore**, con una classe di spacing:

| Classe | Valore | Uso osservato |
|---|---|---|
| `lt-col-spacing-nano` | .3rem | — |
| `lt-col-spacing-micro` | 8px | dettagli |
| `lt-col-spacing-small` | 12px | 9 usi — elementi stretti |
| `lt-col-spacing-md` | 18px | 6 usi — testo + pulsante |
| `lt-col-spacing-md2` | **24px** | **18 usi — il default di fatto** |
| `lt-col-spacing-large` | 32px | 2 usi — separazioni ampie |

⚠️ **`lt-col-spacing-md2` non è un default.** È semplicemente la più frequente, perché
capita spesso che sia quella giusta. **La spaziatura si sceglie in base al layout**, non
per ripiego: un elenco fitto vuole `small`, due blocchi distanti vogliono `large`.

Il conteggio qui sopra descrive cosa è successo su un sito, non cosa fare sul prossimo.

---

## 4. Più container nella stessa sezione

Quando una sezione ha un'intestazione e poi una griglia, sono **due container fratelli**,
non un container con dentro tutto:

```
section.lt-padding-t-b
├── container.lt-section-container        ← intestazione (occhiello + titolo + testo + CTA)
└── container.lt-section-container        ← griglia delle card
```

---

## 5. L'anatomia dell'intestazione

```
container.lt-section-container
├── block.lt-col-spacing-md2
│   ├── div.soprattitolo.lt-col-spacing-md2
│   │   ├── svg                                  ← icona
│   │   └── heading.body-fake-h5                 ← occhiello
│   └── h3                                       ← titolo
└── block.lt-col-spacing-md
    ├── text.body-intro-text-B2
    └── a.bricks-button
```

### La regola che conta: tag e aspetto sono due assi indipendenti

Questo è il motivo per cui il kit ha una famiglia `body-fake-h1…h6` completa, e non
sarebbe altrimenti giustificabile.

```
TAG SEMANTICO   (h1 / h2 / div)        →  lo decide la SEO: cosa deve vedere il crawler
CLASSE body-fake-h*                    →  lo decide il design: che aspetto deve avere
```

**Le due scelte si fanno separatamente.** Non esiste alcun obbligo che un `h2` sembri un H2.

| Serve | Tag | Classe | Effetto |
|---|---|---|---|
| titolo che conta per la SEO ma visivamente è una tagline piccola | `h1` / `h2` | `body-fake-h5` | pesa nella gerarchia, si vede discreto |
| titolo grande per l'utente che **non** deve entrare nella gerarchia | `div` | `body-fake-h1` | domina la pagina, il crawler non lo conta |
| titolo grande che **deve** restare nella gerarchia | `h2` | `body-fake-h1` | pesa e domina |
| occhiello sopra il titolo | `div` | `body-fake-h5` / `-h6` | non ruba gerarchia |

#### Esempio reale — homepage di Web Agency Alba

```
h1.body-fake-h5     "Web agency ad Alba"        ← titolo SEO, reso come tagline
div.body-fake-h1    "Siti che portano clienti"  ← ciò che l'utente legge per primo,
                                                   fuori dalla gerarchia dei titoli
```

Lo stesso blocco può essere costruito al contrario, se il titolo grande deve contare
anche per la SEO: `h2.body-fake-h1`.

**Come si decide, in pratica:**

1. Scrivi cosa deve leggere il **crawler** → sceglie il tag.
2. Scrivi cosa deve vedere l'**utente** → sceglie la classe `body-fake-*`.
3. Se coincidono, il tag nativo basta e la classe non serve.
4. Se non coincidono, scollegali. È voluto, non è un trucco.

> ⚠️ Un solo `h1` per pagina, qualunque aspetto abbia. Lo scollegamento riguarda
> l'aspetto, non la correttezza della gerarchia: `h1 → h2 → h3` senza salti, anche
> quando visivamente l'ordine di grandezza è invertito.

---

## 6. L'anatomia di una card

```
block.{blocco}__card.lt-items-center.lt-col-spacing-md2.lt-pineapple-radius
├── svg
├── h4.lt-text-center
└── text-basic.lt-text-center
```

Le card combinano **una classe BEM propria** (`why__card`) con **le utility del kit**.
La classe BEM porta solo ciò che è specifico di quel blocco: fondo, bordo, padding.
Tutto il resto — allineamento, gap, raggio — arriva dalle utility.

---

## 7. Stile ripetuto = classe. Sempre.

> 🔒 **Se lo stesso CSS serve a più di un elemento, non si scrive sull'elemento: si crea una classe.**

È la regola che distingue un sito costruito da uno assemblato. In un builder visuale la
strada facile è impostare padding e bordo sulla prima card, poi sulla seconda, poi sulla
terza. Funziona finché non devi cambiare il bordo — e a quel punto lo cambi in tre punti,
o in trenta.

### La soglia è due

| Elementi con lo stesso stile | Cosa fare |
|---|---|
| 1 | stile sull'elemento |
| **2 o più** | **classe** |

Non serve aspettare la terza ripetizione. Alla seconda, la classe si crea.

### Esempi

**Tre card con stesso bordo e stesso padding**

```
❌  card 1 → padding + bordo sull'elemento
    card 2 → padding + bordo sull'elemento
    card 3 → padding + bordo sull'elemento

✅  classe  servizi__card  { padding, bordo }
    applicata a tutte e tre
```

**Una tagline resa allo stesso modo in tutto il sito**

```
✅  classe  tagline  { dimensione, colore, spaziatura lettere, margine }
    applicata ovunque compaia
```

Se compare in tutto il sito, la classe è ancora più obbligatoria: senza, per cambiare
la tagline devi passare pagina per pagina.

### Più di una classe, se serve

Non c'è nessun obbligo di far stare tutto in una classe sola. Se il blocco ha parti
distinte, **si creano più classi seguendo BEM**:

```
servizi__card                 fondo, bordo, padding
servizi__card-title           tipografia del titolo
servizi__card-icon            dimensione e colore dell'icona
servizi__card--evidenziata    variante
```

Regole complete in `03-naming.md`. Promemoria: un solo livello di `__`, modificatore
mai da solo, nomi che dicono il ruolo.

### Prima di crearla, però

Le due regole di sempre, in quest'ordine:

1. **Esiste già una utility del kit che fa questo?** Padding, gap, allineamento, raggio,
   centratura sono già coperti — vedi §8. Non si crea `servizi__card-centrata` se esiste
   `lt-items-center`.
2. **Esiste già una classe con questo stile?** `bricks-list-global-classes`, elenco completo.

La classe nuova porta **solo ciò che le utility non coprono**: tipicamente fondo, bordo,
padding specifico, ombra.

### Cosa resta sull'elemento

Solo l'irripetibile: una posizione assoluta di quel singolo elemento, una larghezza
calcolata per quel caso, un `_direction` che cambia da sito a sito (§2).

Se ti accorgi di star scrivendo lo stesso valore due volte, **hai già sbagliato**:
torna indietro e fai la classe.

---

## 8. Composizione delle utility

Le utility si accumulano invece di creare classi nuove:

```
lt-flex  +  lt-flex-row  +  lt-items-center  +  lt-justify-center
```

Frequenze osservate: `lt-items-center` 20, `lt-flex` 15, `lt-text-center` 13,
`lt-justify-center` 9, `lt-flex-col` 8.

**Prima di creare una classe, verifica che non sia una combinazione di utility esistenti.**
Vedi anche §7: la classe nuova porta solo ciò che le utility non coprono.

### Le classi di colore sono semantiche, non letterali

`lt-white-text` **non vuol dire "bianco"**. Vuol dire *"il testo nel colore chiaro della
palette di questo progetto"*. Su deltatraslochi.com rende `rgb(253, 195, 0)`, giallo, ed
è **corretto così**: quello è il chiaro di quel progetto.

**Conseguenze pratiche:**

1. All'inizio di ogni progetto la classe va **puntata sul colore chiaro della palette nuova**.
2. Non si legge il suo valore su un sito per dedurre com'è fatto il kit.
3. Non si "corregge" perché non è bianca. Non è un errore.

Lo stesso ragionamento vale per `lt-black-text`, che è il suo opposto: il colore scuro
del progetto, non il nero assoluto.

---

## 9. Naming

Le convenzioni stanno in **`03-naming.md`**. Qui non si ripetono.

**Unica aggiunta che riguarda questo pattern:**

> Una classe che **esiste già** e non segue le convenzioni **non si rinomina**.
> Si lascia com'è e si continua a usarla con quel nome.

Rinominarla romperebbe tutti gli elementi che la referenziano, e le classi globali
**non hanno revisioni**. Le convenzioni valgono per le classi **nuove**.

---

## 10. Ordine di costruzione di una sezione

```
1. section      +  lt-padding-t-b
2. container    +  lt-section-container   (o -text / -full)
3. block        +  lt-col-spacing-md2
4. contenuto    +  tag scelto per la SEO, classe body-fake-* scelta per l'aspetto (§5)
5. utility      +  lt-items-center, lt-text-center, ...
6. classe BEM   per ciò che le utility non coprono — obbligatoria se lo stile
                si ripete su 2+ elementi (§7)
```

---

## Storia

| Data | Evento |
|---|---|
| 2026-09-17 | Pattern ricavato dal DOM di deltatraslochi.com |
| 2026-09-17 | Alex risolve i punti aperti: `lt-white-text` semantica (§8), quando omettere `lt-padding-t-b` (§1), `lt-col-spacing-md2` non è un default (§3), `lt-pineapple-radius` fuori dal pattern |
| **2026-09-18** | **Alex valida l'impianto e aggiunge la §7 "stile ripetuto = classe". File ufficiale.** |

---

## Avvertenza che resta

Il pattern è stato ricavato da una **lettura del DOM renderizzato**, non dal progetto
dentro Bricks. Alcune classi possono essere applicate dal builder in modi che il DOM
non distingue. In caso di dubbio su un singolo dettaglio, verificare nel builder.
