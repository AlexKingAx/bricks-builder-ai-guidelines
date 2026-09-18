# 04 · BREAKPOINT E RESPONSIVE

> ⚠️ **L'errore più costoso e più facile da fare.**
> Bricks può essere configurato **desktop-first** o **mobile-first**. Se scrivi gli stili con la logica sbagliata, il sito si rompe su tutti i dispositivi tranne uno — e spesso non te ne accorgi subito.

---

## Regola assoluta: verifica prima, sempre

**A inizio di ogni sessione, prima di scrivere anche un solo stile responsive:**

```
bricks-list-breakpoints
```

Guarda questi campi:

```jsonc
{
  "isMobileFirst": false,   // ← IL CAMPO CHE CONTA
  "baseKey": "desktop",     // ← da quale breakpoint si parte
  "baseWidth": 1920,
  "customEnabled": true
}
```

**Non dedurlo. Non ricordarlo dalla sessione precedente. Non assumerlo dall'overlay.** Leggilo.

---

## Le due logiche

### Desktop-first (`isMobileFirst: false`)

Il breakpoint base è il più largo. Le regole scendono verso il basso.

```
Base (es. 1920px)  ← scrivi qui lo stile principale
    ↓ max-width
1279px             ← correzioni
    ↓
991px              ← correzioni
    ↓
478px              ← correzioni
```

In CSS: `@media (max-width: …)`

### Mobile-first (`isMobileFirst: true`)

Il breakpoint base è il più stretto. Le regole salgono.

```
Base (es. 320px)   ← scrivi qui lo stile principale
    ↑ min-width
768px              ← arricchimenti
    ↑
1280px             ← arricchimenti
```

In CSS: `@media (min-width: …)`

---

## Cosa cambia in pratica

| | Desktop-first | Mobile-first |
|---|---|---|
| Stile principale | sul breakpoint **più largo** | sul breakpoint **più stretto** |
| Media query | `max-width` | `min-width` |
| Ordine di lavoro | dal grande al piccolo | dal piccolo al grande |
| Errore tipico | scrivere il layout su mobile e non vederlo su desktop | l'opposto |

---

## Lo standard di Alex

Su **tutti** i suoi siti Alex usa questa configurazione:

```
DESKTOP-FIRST · base 1920px · laptop 1279px · tablet 991 · mobile 767 · mobile 478
```

Fa parte del kit, al pari di classi e variabili. Vedi `10-DESIGN-SYSTEM.md`.

**Questo non esonera dal verificare.** `bricks-list-breakpoints` a inizio sessione
serve a leggere la configurazione reale di quel sito, non a darla per scontata.

---

## 🔴 Non toccare mai i breakpoint

`bricks/set-breakpoints` è **rosso assoluto**.

Cambiare un breakpoint — anche solo la larghezza — significa che **ogni stile responsive del sito cambia punto di attivazione**. Non c'è revisione, non c'è annulla, e il danno si vede solo aprendo le pagine una per una.

I breakpoint presenti su un sito sono già quelli giusti: arrivano con il kit.
Non vanno impostati, non vanno "corretti", non vanno allineati.

Se in un caso eccezionale servisse davvero: si propone, si fa backup del database,
e lo fa Alex consapevolmente.

---

## Regole di scrittura responsive

1. **Lo stile principale va sul breakpoint base.** Gli altri contengono solo le differenze.
2. **Non ripetere** su ogni breakpoint ciò che è già ereditato.
3. **Non saltare livelli** senza motivo: se correggi 478 ma non 767, controlla che 767 stia bene davvero.
4. Per spaziature e tipografia preferisci **scale fluide** (`clamp()` via `generate-scale-variables`) invece di quattro valori fissi.
5. Verifica il risultato **almeno** su base, un intermedio e il più stretto.

---

## Novità Bricks 2.4 utili qui

**Sposta gli stili tra breakpoint.** Prima, se avevi stilizzato tutto sul breakpoint sbagliato, dovevi rifare a mano. Ora:

- copia **tutti** gli stili o **solo il breakpoint corrente**
- incolla mantenendo il breakpoint d'origine **oppure** forzando su quello corrente
- **sposta** stili da un breakpoint a un altro (merge o sovrascrittura)
- reset totale o del solo breakpoint corrente

Funziona su elementi, classi attive e selezioni multiple.

> Utile per rimediare a un lavoro impostato sul breakpoint sbagliato. Resta comunque una modifica: se coinvolge classi globali → 🟠.

**Offcanvas con direzione responsive:** lato di apertura diverso per breakpoint.

**Gap unificati:** flex e grid condividono gli stessi controlli row/column gap.

---

## Verifica finale

Prima di dire che un lavoro responsive è finito:

```
1. bricks-list-breakpoints        → conferma la logica
2. bricks-get-page-elements       → controlla dove sono finiti gli stili
3. browser + screenshot           → base, intermedio, mobile
```

**Un lavoro responsive non verificato visivamente non è finito.**
