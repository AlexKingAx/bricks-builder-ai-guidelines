# AVVIO — Prompt pronti per iniziare una sessione

---

## 🚀 Apertura di sessione (incolla sempre questo per primo)

```
Sto lavorando su webagencyalba.it tramite MCP.

Prima di qualsiasi cosa:
1. leggi bricks-guidelines/00-SAFETY.md e CLAUDE.md
2. leggi bricks-guidelines/siti/webagencyalba-it.md (overlay del sito)
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

## 📌 Specifico di webagencyalba.it

```
Ambiente:     STAGING — regole normali, ma le conferme sul design system restano
Breakpoint:   DESKTOP-FIRST, base 1920px, con "laptop" a 1279px
Classi:       64, tutte da mantenere — non proporre pulizie
Palette:      ancora quella stock di Bricks, da sostituire con i colori di brand
Componenti:   bullet, Back To Top Button, Whatsapp float
```

⚠️ Il JS del "Back To Top Button" sta nel **child theme**, non nel componente: se tocchi quel componente, controlla anche lì.
