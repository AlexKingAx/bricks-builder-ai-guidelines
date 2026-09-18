# AVVIO — Prompt pronti per iniziare una sessione

---

## 🆕 Primo avvio su un sito NUOVO (non ancora documentato)

Usa questo quando in `bricks-guidelines/siti/` **non esiste ancora** il file di questo sito.

```
Lavoreremo su <NOME SITO>. È un ambiente di <produzione / staging / locale>.

In bricks-guidelines/siti/ non esiste ancora il suo overlay: va creato prima di
qualsiasi altra cosa.

1. leggi bricks-guidelines/siti/README.md e segui il processo che descrive
2. leggi bricks-guidelines/00-SAFETY.md e CLAUDE.md
3. copia 90-overlay-TEMPLATE.md in siti/<nomesito>.md
4. compilalo leggendo il sito vero, SOLA LETTURA, senza modificare niente
5. riportami: incoerenze trovate, cose che sembrano sbagliate ma potrebbero
   essere volute, e le domande a cui le abilities non rispondono

Non iniziare nessun lavoro finché non ti confermo l'overlay.
```

L'ultima riga è quella che conta: separa la documentazione dal lavoro.

---

## 🚀 Apertura di sessione (incolla sempre questo per primo)

```
Sto lavorando su <NOME SITO> tramite MCP.

Prima di qualsiasi cosa:
1. leggi bricks-guidelines/00-SAFETY.md e CLAUDE.md
2. leggi bricks-guidelines/siti/<nomesito>.md (overlay del sito)
   — se non esiste, fermati: va creato prima, vedi siti/README.md
3. chiama bricks-list-breakpoints e bricks-get-design-context

Poi dimmi in tre righe cosa hai capito del design system, e aspetta istruzioni.
Non modificare niente finché non te lo chiedo.
```

L'ultima riga è quella che conta: impedisce di partire a scrivere prima di aver letto.

---

## 🛠 Come chiedere un lavoro

**Vago → risultati imprevedibili:**
```
fammi una sezione hero
```

**Specifico → risultati usabili:**
```
Aggiungi una sezione hero alla pagina Servizi.
Usa le classi e le variabili che già esistono, non crearne di nuove.
Mostrami il piano prima di scrivere.
```

Le tre frasi da tenere a portata di mano:

| Frase | Cosa ottiene |
|---|---|
| `Usa il design system esistente, non creare classi nuove` | evita la frammentazione |
| `Mostrami il piano prima di scrivere` | vedi cosa succede prima che succeda |
| `Verifica il risultato e mostrami cosa hai toccato` | niente "fatto" non verificati |

---

## 🔍 Prompt di sola lettura (sempre sicuri)

**Capire una pagina:**
```
Mostrami la struttura della pagina <nome> e quali classi globali usa.
Sola lettura.
```

**Controllare lo stato del design system:**
```
Chiama bricks-get-design-context e bricks-audit-design-system.
Riportami solo le cose che sono davvero rotte, non le classi inutilizzate:
quelle fanno parte dello starter kit e restano.
```

**Prima di toccare qualcosa:**
```
Dimmi dove è usata la classe <nome> prima che decida se modificarla.
```

---

## ⚠️ Cosa aspettarsi durante il lavoro

| Tipo di azione | Cosa succede |
|---|---|
| 🟢 letture, modifiche a pagine | procede e ti riporta cosa ha fatto |
| 🟠 design system, impostazioni | si ferma, descrive, aspetta il tuo "sì" |
| 🔴 cancellazioni, PHP, breakpoint | si ferma sempre, anche se hai già detto sì poco prima |

Se qualcosa procede senza chiedere quando doveva chiedere → **le guidelines non sono state lette.** Riapri con il prompt di apertura.

---

## 📌 Le informazioni specifiche del sito

Non stanno qui: stanno nell'**overlay del sito**, in `siti/<nomesito>.md`.

Ambiente, breakpoint, classi disponibili, palette, componenti, cose da non toccare e
debito noto sono dati che cambiano da progetto a progetto e vanno **letti dal sito**,
mai copiati da un altro.

Se l'overlay non esiste → `siti/README.md`, processo obbligatorio.
