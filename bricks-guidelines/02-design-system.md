# 02 · DESIGN SYSTEM — Regole di costruzione

Valgono su qualsiasi sito Bricks. I valori concreti (nomi, token, scale) stanno nell'**overlay del sito** — vedi `90-overlay-TEMPLATE.md`.

---

## La gerarchia. In quest'ordine, sempre.

```
1. VARIABILE GLOBALE      il valore vive in un posto solo
2. CLASSE GLOBALE         un pacchetto di stili riutilizzabile
3. COMPONENTE             struttura + stile + contenuto, riusabili
4. STILE SULL'ELEMENTO    ultima risorsa, solo per l'irripetibile
```

**Si scende di livello solo quando il livello sopra non basta.** Se ti ritrovi a mettere stili sull'elemento, fermati e chiediti perché la classe non esiste.

> 🔒 **Soglia: due.** Se lo stesso stile serve a **2 o più elementi**, si crea una classe.
> Non si aspetta la terza ripetizione. Vedi `91-pattern-sezioni-blocchi.md` §7.

---

## Regola 1 — Mai valori hardcoded

```css
padding: 24px;                    ❌
padding: var(--lt-padding);       ✅

color: #ff6600;                   ❌
color: var(--primary);            ✅

border-radius: 8px;               ❌
border-radius: var(--lt-radius);  ✅
```

**Le uniche eccezioni ammesse:**
- `0`, `100%`, `auto`, `none`
- valori strutturali unici e non riutilizzabili, **documentati con un commento**

Se un valore serve due volte → **è una variabile**.

---

## Regola 2 — Cerca prima di creare

Prima di ogni `create-global-class`, `create-color` o `set-global-variables`:

1. `list-global-classes` / `list-global-variables` — elenco completo, non troncato
2. cerca per **nome** e per **sinonimo** (`card` / `box` / `scheda` / `tile`)
3. cerca per **valore**: esiste già una variabile con quel colore o quella misura?

Se trovi qualcosa di simile ma non identico → **chiedi**. Le due risposte possibili sono "estendi quella" o "creane una nuova", e la scelta è dell'utente.

> Un design system muore per accumulo di quasi-duplicati, non per una cancellazione sbagliata.

---

## Regola 3 — Le scale si generano, non si scrivono a mano

Per spaziature, tipografia e sfumature di colore usa i generatori:

| Ability | Genera |
|---|---|
| `bricks/generate-scale-variables` | scale fluide di spacing e tipografia |
| `bricks/generate-color-shades` | sfumature coerenti di un colore |

Una scala generata è matematicamente coerente. Dodici valori scritti a mano in sei mesi non lo sono mai.

---

## Regola 4 — Il colore passa dalla palette

```
Colore nella palette  →  variabile  →  classe/elemento
```

Mai un esadecimale in un controllo o nel CSS. Se serve un colore nuovo, entra nella palette prima di essere usato.

Serve una variante più chiara/scura? `generate-color-shades`, non un esadecimale a occhio.

### Le variabili colore non si creano a mano

`bricks/create-color` e `bricks/create-color-palette` **generano già la variabile CSS** di
ogni colore. Per i colori **non serve `set-global-variables`**: è una chiamata in più, ed è
per giunta la più pericolosa del design system, perché può azzerare le variabili non
incluse nel payload.

**Il nome però va passato esplicitamente nel campo `raw`:**

```
raw: "var(--color-action)"    ✅ nome leggibile, scelto da te
raw omesso                    ❌ Bricks genera var(--bricks-color-a7f3k2)
```

Un `raw` omesso produce un nome opaco che viola `03-naming.md` e non si può più leggere
fra sei mesi.

`create-color-palette` accetta i colori inline nell'array `colors`: **una palette completa
si crea con una sola chiamata**, non con palette + N colori + N variabili.

> Verificato su webagencyalba.it il 2026-09-17 leggendo lo schema delle ability.

---

## Regola 5 — Classe o componente?

| Serve | Usa |
|---|---|
| solo stile, applicato a elementi diversi | **classe globale** |
| stile + struttura + contenuto, ripetuti | **componente** |
| una variante di qualcosa che esiste | **variante del componente**, non una classe nuova |

Se stai per ricostruire a mano la stessa struttura per la terza volta → è un componente.

⚠️ I componenti **non hanno revisioni**. Modificarne uno usato in venti punti li cambia tutti e venti, senza annulla.

---

## Regola 6 — Theme style per il default, classe per l'eccezione

```
Theme style   →   come si comporta il sito per default (h1, p, a, button…)
Classe        →   quando serve discostarsi dal default
```

Se stai creando una classe per riportare un elemento al comportamento normale, il problema è nel theme style.

### 🔒 Prima di creare una classe: cerca nel theme style

Vale per **qualsiasi stile di default riutilizzabile**, non solo per i bottoni.

Prima di ogni `create-global-class`, leggi il theme style e verifica se Bricks ha già
un'impostazione nativa per quella cosa: tipografia, titoli, link, bottoni, form, liste,
tabelle, immagini, colori di base.

```
è un comportamento di default del sito?      →  theme style
è una cosa precisa e circoscritta?           →  classe globale
```

Una classe che fa ciò che il theme style fa già nativamente è un **duplicato**: sopravvive
al progetto, va mantenuta, e prima o poi entra in conflitto con l'impostazione nativa che
nessuno ha mai toccato.

> Regola data da Alex il 2026-09-17. Vale su tutti i siti.

### 🔒 Il caso più frequente: i bottoni si stilizzano SOLO nel theme style

**Non si crea nessuna classe globale per i bottoni.** Mai.
Bricks ha una sezione `button` dedicata dentro il theme style: colori, padding, bordi,
raggio, stati e dimensioni (`sm` → `xl`) stanno lì, e da lì valgono su tutto il sito.

```
Stile dei bottoni        →  theme style, sezione button        ✅
Classe globale btn / btn--ghost  →  NO, non si fa              ❌
```

Le varianti (`primary`, `secondary`, `outline`) sono già previste da Bricks tramite
l'impostazione `style` dell'elemento bottone: si configurano nel theme style, non si
reinventano con classi.

Una classe su un bottone si giustifica **solo** per qualcosa che il theme style non
copre e che riguarda quel singolo punto — ad esempio una larghezza al 100% in un
form specifico. Mai per il suo aspetto di base.

> Regola data da Alex il 2026-09-17. Vale su tutti i siti.

> In Bricks 2.4 i theme style si modificano dal Control Panel, con filtro **Modified** per vedere solo ciò che è stato toccato. Comodo per capire cosa è default e cosa no.

---

## Regola 7 — Ordine di creazione

Quando costruisci qualcosa da zero, **le dipendenze vanno create prima di chi le usa**:

```
1. colori nella palette
2. variabili (e scale generate)
3. theme style root
4. classi globali di base
5. componenti
6. template (header, footer)
7. pagine
```

Vale anche per gli import HTML/CSS: prima `batch-create-global-classes` (una volta sola), **poi** l'albero degli elementi.

---

## Anti-pattern ricorrenti

| ❌ | ✅ |
|---|---|
| `card-2`, `card-new`, `card-final` | una classe, con varianti |
| classi vuote, senza impostazioni | cancellarle o compilarle |
| stessa classe con due ID diversi | unificare, aggiornare i riferimenti |
| `!important` per vincere la specificità | sistemare l'ordine delle classi |
| valori uguali sparsi in dieci classi | una variabile |
| classi che descrivono l'aspetto (`testo-rosso-16`) | classi che descrivono il ruolo (`alert-text`) |

---

## Manutenzione periodica

| Ability | Cosa fa |
|---|---|
| `bricks/audit-design-system` | cerca derive, duplicati, incoerenze |
| `bricks/list-orphaned-elements` | elementi rimasti scollegati |
| `bricks/get-design-context` con `includeUsage: true` | dove sono usati i componenti |

**Gli audit sono in sola lettura: eseguili liberamente.**
Le pulizie che ne derivano sono 🔴 rosse: si propongono, non si eseguono.
