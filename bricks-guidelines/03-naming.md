# 03 · NAMING — Convenzioni

Un design system si sfalda per il naming, non per la tecnica. Due persone (o due sessioni) che chiamano la stessa cosa in modo diverso creano due cose.

---

## Regola zero: adattati al sito, non imporre

**Prima di creare qualsiasi nome, leggi come sono chiamate le cose esistenti.**

```
bricks-list-global-classes
bricks-list-global-variables
```

Se il sito ha già una convenzione → **seguila**, anche se non è quella preferita.
Se il sito non ne ha una → applica lo standard qui sotto.
Se il sito ne ha **più di una** → fermati e chiedi quale adottare. Non sceglierne una di testa tua: creeresti la convenzione numero tre.

---

## Lo standard di riferimento

### Prefissi per famiglia

| Prefisso | Famiglia | Esempi |
|---|---|---|
| `lt-` | layout e utility | `lt-section-container`, `lt-padding`, `lt-radius` |
| `body-` | tipografia | `body-intro-text`, `body-fake-h1`, `body-pre-title` |
| `an-` | animazioni | `an-fadein`, `an-fadeinup` |
| *(nessuno)* | blocchi funzionali | `faq`, `slider`, `footer` |

### Blocchi funzionali: BEM — **obbligatorio**

> 🔒 **Ogni classe nuova che descrive un blocco deve rispettare BEM.**
> Non è una preferenza stilistica: è la regola che impedisce al sistema di sfaldarsi.
> Una classe nuova che non è BEM non si crea — si rinomina prima.

```
blocco__elemento--modificatore
  │        │          └── variante di stato o di aspetto   (due trattini)
  │        └───────────── parte interna del blocco          (due underscore)
  └────────────────────── il blocco, autonomo
```

#### Le tre parti

| Parte | Cos'è | Esempio |
|---|---|---|
| **Blocco** | entità autonoma, ha senso da sola | `card`, `faq`, `nav`, `hero` |
| **Elemento** | parte che **non** esiste fuori dal blocco | `card__title`, `faq__question` |
| **Modificatore** | variante di aspetto o stato | `card--featured`, `nav__item--active` |

#### Le regole, senza eccezioni

**1. Un solo livello di elemento.** BEM è piatto, non rispecchia il DOM.

```
card__title            ✅
card__body__text       ❌ due livelli
card__text             ✅ (anche se nel DOM sta dentro al body)
```

**2. Il modificatore non vive da solo.** Va sempre insieme alla classe base.

```html
<div class="card card--featured">   ✅
<div class="card--featured">        ❌ senza `card` non ha stili
```

**3. Il modificatore descrive lo stato, non l'aspetto.**

```
card--featured      ✅ dice perché
card--in-evidenza   ✅ va bene se il sito è in italiano
card--bordo-rosso   ❌ descrive come, invecchia male
```

**4. Dentro una parte si usa il trattino singolo.**

```
product-card__price-label--on-sale
└─ blocco ──┘└─ elemento ──┘└─ mod ─┘
```

**5. Il blocco non contiene il nome del genitore.**

```
page__card__title   ❌ card è un blocco, non un elemento di page
card__title         ✅
```

**6. Niente nomi di tag o di posizione.**

```
faq__div            ❌   faq__answer      ✅
hero__left-column   ❌   hero__media      ✅
```

#### Quando BEM **non** si applica

Due sole famiglie restano fuori, perché non sono blocchi:

| Famiglia | Convenzione | Perché |
|---|---|---|
| Utility `lt-` | `lt-flex`, `lt-text-center` | fanno una cosa sola, non hanno parti |
| Tipografia `body-` | `body-fake-h1`, `body-intro-text-B1` | scala tipografica, non blocchi |

**Tutto il resto è BEM.** Se stai per creare una classe che non rientra in queste due
famiglie e non è BEM, la stai sbagliando.

#### Esempi completi

```
✅ CORRETTO
hero                      blocco
hero__title               elemento
hero__subtitle            elemento
hero__cta                 elemento
hero__cta--secondary      modificatore su elemento
hero--compact             modificatore su blocco

❌ SBAGLIATO                        → ✅ CORRETTO
titolo-hero                         hero__title
hero-title                          hero__title          (un underscore doppio, non uno singolo)
hero__content__title                hero__title          (un solo livello)
hero__titolo-grande                 hero__title--large   (variante = modificatore)
hero-2                              hero--compact        (mai numeri progressivi)
```

#### Prima di creare: verifica

```
1. È un blocco, un elemento o un modificatore?
2. Se elemento → il blocco esiste già?
3. Un solo livello di `__`?
4. Il modificatore dice il perché, non il come?
5. Rispetto la lingua del sito?
6. Ho cercato se esiste già?  →  bricks-list-global-classes
```

Una sola risposta negativa → **non creare. Chiedi.**

> ⚠️ Il kit contiene già classi che non rispettano BEM (`sottotitolo`, `colonne-contatti`,
> `slide-foto`). Sono storiche e **restano**. Non sono un precedente: le classi nuove
> seguono BEM.

### Variabili: famiglia + ruolo + scala

```
--lt-spacing-nano       .3rem
--lt-spacing-micro      8px
--lt-spacing-small      12px
--lt-spacing-md         18px
--lt-spacing-large      32px

--lt-radius             8px
--lt-radius-medium      12px
```

Scala coerente e prevedibile. **Mai** `--spacing-1`, `--spacing-2`: fra sei mesi nessuno sa cosa sono.

---

## Regole di scrittura

| Regola | ✅ | ❌ |
|---|---|---|
| Solo minuscole e trattini | `card-title` | `cardTitle`, `Card_Title` |
| Una sola lingua per sito | tutto IT o tutto EN | `titolo-card` + `card-subtitle` |
| Ruolo, non aspetto | `alert-text` | `testo-rosso-16px` |
| Niente numeri progressivi | `card--featured` | `card-2`, `card-3` |
| Niente abbreviazioni oscure | `section-container` | `sct-ctr` |

---

## Sulla lingua

Italiano e inglese **entrambi vanno bene**. Quello che non va bene è mescolarli nello stesso sito.

L'inglese ha un vantaggio pratico: combacia con le proprietà CSS e con i nomi degli elementi Bricks, quindi si legge meglio in mezzo al codice.

**Scegli una lingua per sito, scrivila nell'overlay, non deviare.**

---

## Categorie

Bricks permette di raggruppare classi e variabili in categorie. **Usale.** Con 100+ classi senza categorie non trovi più niente.

Categorie minime consigliate:

```
Layout · Tipografia · Componenti · Animazioni · Utility
```

---

## Prima di creare un nome: la checklist

1. Ho letto l'elenco completo delle classi esistenti? *(non `get-design-context`, che tronca a 100 — usa `list-global-classes`)*
2. Esiste già con un nome diverso? *(cerca sinonimi)*
3. Il nome segue la convenzione del sito?
4. Dice il **ruolo** o solo l'**aspetto**?
5. Reggerà quando serviranno le varianti?

Se una risposta è no → **non creare. Chiedi.**
