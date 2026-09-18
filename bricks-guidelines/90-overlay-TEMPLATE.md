# 90 · OVERLAY SITO — Template

> **Come si usa:** copia questo file in `siti/<nome-sito>.md` e compilalo leggendo il sito vero.
> I file `00`–`05` sono regole generali valide ovunque. **Questo file contiene i valori concreti di UN sito.**
> In caso di conflitto, l'overlay vince sui file generali — tranne che su `00-SAFETY.md`, che vince sempre.

---

## Identità

```
Sito:
URL:
Ambiente:        locale / staging / PRODUZIONE
Backup:          dove, con che frequenza, chi lo fa
Stato:           attivo / legacy / da rifare
```

⚠️ Se **PRODUZIONE**: valgono le regole rinforzate di `00-SAFETY.md`.

---

## Come si compila (da eseguire sul sito)

```
bricks-list-breakpoints          → logica responsive
bricks-get-design-context        → panoramica
bricks-list-global-classes       → elenco COMPLETO (get-design-context tronca a 100)
bricks-list-global-variables     → elenco completo
bricks-list-color-palettes       → palette e colori
bricks-list-components           → componenti
bricks-list-templates            → header, footer, archivi
bricks-audit-design-system       → derive e duplicati
```

---

## 1. Breakpoint ⚠️

> Compilare leggendo `bricks-list-breakpoints`. **Non copiare da un altro sito.**

```
isMobileFirst:   true / false
Logica:          DESKTOP-FIRST (max-width) / MOBILE-FIRST (min-width)
Base:            <key> — <larghezza>px
```

| Key | Etichetta | Larghezza | Custom |
|---|---|---|---|
| | | | |

---

## 2. Lingua e convenzione

```
Lingua dei nomi:     italiano / inglese
Convenzione:         <es. lt- / body- / an- + BEM>
Categorie in uso:    <elenco>
```

Deviazioni note dalla convenzione (da non imitare):
```
```

---

## 3. Variabili

| Variabile | Valore | Uso |
|---|---|---|
| | | |

**Scale disponibili:**
```
Spacing:
Tipografia:
Radius:
```

---

## 4. Colori

| Palette | Colori | Ruolo |
|---|---|---|
| | | |

Colori primari da usare:
```
```

---

## 5. Classi globali per famiglia

**Layout / container**
```
```

**Tipografia**
```
```

**Bottoni**
```
```

**Animazioni**
```
```

**Blocchi funzionali**
```
```

---

## 6. Componenti

| Componente | Cosa fa | Proprietà | Usato in |
|---|---|---|---|
| | | | |

---

## 7. Theme styles

| ID | Etichetta | Condizioni |
|---|---|---|

---

## 8. Template

| Tipo | Nome | Condizioni |
|---|---|---|
| Header | | |
| Footer | | |

---

## 9. Regole specifiche di questo sito

Cose che valgono **solo qui** e che vanno rispettate:

```
-
-
```

---

## 10. Da non toccare

Elementi fragili, personalizzazioni delicate, cose che sembrano sbagliate ma sono volute:

```
-
-
```

---

## 11. Debito tecnico noto

Problemi già identificati, **da non "sistemare" di iniziativa**:

| Problema | Dove | Note |
|---|---|---|
| | | |

---

## 12. Stack

```
Plugin rilevanti:
Slider:              <es. Splide / Swiper nativo>
Custom fields:       <es. ACF / Meta Box>
SEO:
WooCommerce:         sì / no
```
