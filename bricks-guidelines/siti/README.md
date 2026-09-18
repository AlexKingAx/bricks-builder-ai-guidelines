# siti/ — overlay dei progetti

Questa cartella contiene **un file per ogni sito** su cui si lavora. I file **non sono
versionati**: sono documentazione di lavoro, spesso relativa a progetti di clienti, e vengono
**rigenerati leggendo il sito vero** a inizio progetto.

Nel repository trovi solo questo README. `.gitignore` esclude tutto il resto.

---

## Cosa va qui

```
siti/
├── README.md              ← questo file, l'unico versionato
├── nomesito-it.md         ← overlay del progetto        (ignorato da git)
└── nomesito-it-classi.md  ← inventario classi, se serve (ignorato da git)
```

---

## Come si genera un overlay

**1. Copia il template**

```bash
cp bricks-guidelines/90-overlay-TEMPLATE.md bricks-guidelines/siti/nomesito-it.md
```

**2. Compilalo leggendo il sito vero**, mai a memoria e mai copiando da un altro progetto.
Le chiamate MCP da usare sono elencate dentro il template:

```
bricks-list-breakpoints          bricks-list-components
bricks-get-design-context        bricks-list-templates
bricks-list-global-classes       bricks-list-color-palettes
bricks-list-global-variables     bricks-audit-design-system
```

> ⚠️ Usa `bricks-list-global-classes` per l'elenco completo delle classi.
> `bricks-get-design-context` **tronca a 100** e ti farebbe credere che alcune non esistano.

**3. Oppure fatti aiutare.** In `../AVVIO.md` c'è il prompt pronto: chiede all'AI di leggere
il sito in sola lettura e compilare il template.

---

## Cosa deve contenere, come minimo

| Sezione | Perché serve |
|---|---|
| **Ambiente** | produzione, staging o locale — cambia le regole di `00-SAFETY.md` |
| **Breakpoint** | letti dal sito, mai assunti |
| **Lingua e convenzione** | per non creare la sesta convenzione di naming |
| **Variabili e colori** | cosa esiste già |
| **Classi per famiglia** | cosa esiste già |
| **Da non toccare** | ⚠️ la sezione che evita più danni: le cose che sembrano sbagliate ma sono volute |
| **Debito tecnico noto** | problemi già identificati, **da non "sistemare" di iniziativa** |

---

## Precedenza

L'overlay di un sito **vince sulle regole generali e sul design system canonico**, perché un
progetto può avere regole sue. Non vince mai su `00-SAFETY.md`.

```
00-SAFETY.md  >  overlay del sito  >  10-DESIGN-SYSTEM.md  >  91-pattern  >  regole generali
```

Se un progetto si discosta dal kit, va scritto **qui dentro**, esplicitamente.
