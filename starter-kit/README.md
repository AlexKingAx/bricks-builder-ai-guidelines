# Starter kit — pacchetto importabile

`bricks-starter-kit.zip` è il design system descritto in
[`../bricks-guidelines/10-DESIGN-SYSTEM.md`](../bricks-guidelines/10-DESIGN-SYSTEM.md),
esportato da Bricks e pronto da importare su un'installazione nuova.

```
65  classi globali        layout, tipografia, animazioni, spaziature, blocchi
24  variabili             scale tipografiche fluide, spaziature, raggi, bordi
 2  theme styles          stile del sito e stile per gli articoli
 5  breakpoint            desktop-first, base 1920px
```

Richiede **Bricks 2.4 o superiore**. Peso: ~11 KB.

---

## ⚠️ Cosa NON contiene, di proposito

| Escluso | Perché |
|---|---|
| **Palette colori** | si crea all'inizio di ogni progetto, con i colori del brand |
| Componenti | legati al progetto o a plugin di terze parti |
| Template | header, footer e archivi sono specifici del sito |
| Impostazioni | chiavi API, codice personalizzato e altre impostazioni del sito |

Dopo l'import la palette va creata, e le classi si raccordano ai colori nuovi **man mano che
servono**. Finché non lo fai è **normale** che l'audit segnali variabili orfane
(`--color-1…8`, `--nero`, `--white`) e molte classi non referenziate: **non sono difetti.**

---

## Come si importa

### Dal builder — Global Import & Export

`Bricks → Settings → Import & Export` → carica lo ZIP → scegli cosa importare.

### Via MCP, con un assistente AI

```
1. bricks/inspect-transfer-package     legge lo ZIP, restituisce conflitti e zipHash
2. bricks/import-transfer-package      importa, passando lo zipHash come expectedZipHash
```

In caso di conflitto **mantiene l'esistente**, salvo `allowOverwrite: true`.

Prompt pronto:

```
Importa starter-kit/bricks-starter-kit.zip su questo sito.
Prima ispezionalo e mostrami conflitti e avvisi.
Non sovrascrivere niente senza chiedermelo.
```

---

## 🔴 Attenzione ai breakpoint

Il pacchetto contiene i breakpoint: **desktop-first, base 1920px**, poi 1279 · 991 · 767 · 478.

Importarli su un sito **già stilizzato** cambia il punto di attivazione di ogni stile
responsive esistente, e **non è reversibile**: i breakpoint non hanno revisioni.

```
sito nuovo e vuoto      →  importa tutto, breakpoint compresi
sito già stilizzato     →  importa classi e variabili, LASCIA FUORI i breakpoint
```

In caso di dubbio: backup del database prima di importare.

---

## Verifica dopo l'import

```
bricks-list-breakpoints          i breakpoint sono quelli attesi?
bricks-list-global-classes       elenco completo — get-design-context tronca a 100
bricks-list-global-variables     le variabili sono arrivate con i valori giusti
```

---

## Rigenerare il pacchetto

Da un sito che ha il kit aggiornato:

```
1. bricks/list-transfer-items        ottieni gli ID di classi, variabili, theme styles
2. bricks/export-transfer-package    types: classes, variables, theme-styles, breakpoints
```

> Il `manifest.json` dell'export contiene **nome e URL del sito sorgente**. Prima di
> pubblicare il pacchetto, sostituisci il blocco `site` — altrimenti pubblichi il dominio
> da cui è stato estratto.
