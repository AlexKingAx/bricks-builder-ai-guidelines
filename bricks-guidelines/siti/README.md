# siti/ — overlay dei progetti

Questa cartella è **volutamente vuota** in una installazione pulita. Contiene solo questo file.

Qui dentro va **un file per ogni sito** su cui si lavora. Quei file non sono versionati: sono
documentazione di lavoro, spesso relativa a progetti di clienti, e si **rigenerano leggendo il
sito vero** all'inizio di ogni progetto.

---

## 🔴 Regola: senza overlay non si lavora

> **Prima di eseguire qualsiasi azione su un sito Bricks, in questa cartella deve esistere il
> file dell'overlay di quel sito.**
>
> Se non c'è, la **prima cosa da fare è crearlo**. Non "dopo", non "se c'è tempo": prima.

Non è burocrazia. Senza overlay l'assistente non sa:

- se è **produzione o staging** → cambia tutto il livello di rischio in `00-SAFETY.md`
- quali **breakpoint** ha il sito → scrive gli stili nella logica sbagliata
- quali **classi e variabili** esistono già → ne crea di duplicate
- cosa **non va toccato** → rompe cose messe lì di proposito

---

## Il processo, passo per passo

### Passo 0 — Verifica se l'overlay esiste

```bash
ls bricks-guidelines/siti/
```

| Esito | Cosa fare |
|---|---|
| c'è `nomesito.md` | leggilo e procedi col lavoro |
| **non c'è** | **fermati e vai al passo 1** |

### Passo 1 — Chiedi conferma del sito e dell'ambiente

Due cose che **non si deducono**, si chiedono:

```
Su quale sito lavoriamo?
È in produzione, staging o locale?
```

> `00-SAFETY.md` è esplicito: l'ambiente non si deduce dal dominio. Un `.it` può essere uno
> staging, un `.local` può contenere lavoro non salvato altrove.

### Passo 2 — Copia il template

```bash
cp bricks-guidelines/90-overlay-TEMPLATE.md bricks-guidelines/siti/nomesito-it.md
```

Nome file: il dominio con i punti sostituiti da trattini. `webagencyalba.it` → `webagencyalba-it.md`.

### Passo 3 — Leggi il sito vero

**Sola lettura. Nessuna scrittura in questa fase.** Le chiamate:

```
bricks-list-breakpoints          ⚠️ obbligatoria, mai assumere la logica responsive
bricks-get-design-context        panoramica generale
bricks-list-global-classes       ⚠️ elenco COMPLETO (get-design-context tronca a 100)
bricks-list-global-variables     variabili con i valori
bricks-list-color-palettes       palette e colori
bricks-list-components           componenti e dove sono usati
bricks-list-templates            header, footer, archivi, condizioni
bricks-audit-design-system       incoerenze e riferimenti rotti
```

### Passo 4 — Compila il template

Ogni sezione va riempita con quello che hai **letto**, non con quello che ti aspetti.
Se un dato non è ricavabile dalle abilities, scrivi *"non determinato"* invece di indovinare.

Sezioni che non possono restare vuote:

| Sezione | Perché |
|---|---|
| **Ambiente** | determina il livello di rischio di ogni azione successiva |
| **Breakpoint** | letti dal sito, mai copiati da un altro progetto |
| **Lingua e convenzione** | per non introdurre una convenzione in più |
| **Da non toccare** | ⚠️ la sezione che evita più danni |
| **Debito tecnico noto** | problemi già visti, **da non "sistemare" di iniziativa** |

### Passo 5 — Riporta e fai validare

Prima di considerare l'overlay buono, elenca all'utente:

- le **incoerenze trovate** dall'audit
- le cose che **sembrano sbagliate ma potrebbero essere volute**
- le **domande aperte** a cui le abilities non rispondono

> Molte segnalazioni dell'audit sono **falsi positivi**. Variabili "orfane" e classi "non
> referenziate" sono normali su uno starter kit non ancora raccordato alla palette del
> progetto. **Non sono difetti e non vanno corretti.** Vedi `../10-DESIGN-SYSTEM.md`.

### Passo 6 — Solo adesso si lavora

Con l'overlay compilato e validato, il lavoro può cominciare.

---

## Riepilogo

```
sito nuovo
   │
   ├─ 0. l'overlay esiste?  ──── sì ──→ leggilo e lavora
   │           │
   │           no
   │           ↓
   ├─ 1. chiedi quale sito e quale ambiente
   ├─ 2. copia 90-overlay-TEMPLATE.md
   ├─ 3. leggi il sito (sola lettura)
   ├─ 4. compila ogni sezione
   ├─ 5. riporta incoerenze e domande, fai validare
   └─ 6. ora si lavora
```

Prompt pronto in [`../AVVIO.md`](../AVVIO.md), sezione *Primo avvio su un sito nuovo*.

---

## Cosa c'è in questa cartella

```
siti/
├── README.md              ← questo file, l'unico versionato
├── nomesito-it.md         ← overlay del progetto        (ignorato da git)
└── nomesito-it-classi.md  ← inventario classi, opzionale (ignorato da git)
```

In `.gitignore`:

```gitignore
bricks-guidelines/siti/*
!bricks-guidelines/siti/README.md
```

Così ogni installazione parte pulita, e gli overlay restano sulla macchina di chi lavora.

---

## Precedenza

L'overlay di un sito **vince sulle regole generali e sul design system canonico**, perché un
progetto può avere regole sue. Non vince mai su `00-SAFETY.md`.

```
00-SAFETY.md  >  overlay del sito  >  10-DESIGN-SYSTEM.md  >  91-pattern  >  regole generali
```

Se un progetto si discosta dal kit, va scritto **qui dentro**, esplicitamente.
