# Guida Workflow AI

> Questa guida spiega come usare i workflow disponibili per ottimizzare lo sviluppo con l'AI.

---

## 🎯 Panoramica

I workflow sono comandi slash (`/comando`) che attivano procedure strutturate. Ogni workflow ha uno scopo specifico e produce output predefiniti.

---

## 📋 Workflow Disponibili

### Pianificazione

| Comando | Quando Usarlo | Output |
|---------|---------------|--------|
| `/plan_new_feature` | Nuova feature da PRD | Task list Backend (B1, B2...) + Frontend (F1, F2...) |
| `/refactor_actual_code` | Modifiche a codice esistente | Mappa modifiche file-per-file con linee e funzioni |

### Sviluppo

| Comando | Quando Usarlo | Output |
|---------|---------------|--------|
| `/debug` | Bug da investigare | Root cause + fix |

### Verifica

| Comando | Quando Usarlo | Output |
|---------|---------------|--------|
| `/review` | Prima di merge | Checklist qualità |
| `/optimize` | Pagine lente, bundle grande | Report performance |

### Rilascio

| Comando | Quando Usarlo | Output |
|---------|---------------|--------|
| `/deploy` | Prima di deploy | Checklist pre-produzione |
| `/commit` | Fine lavoro | Lint + commit + push |
| `/document` | Feature completata | Documentazione aggiornata |

---

## 🔄 Ciclo di Sviluppo Tipico

```
1. /plan_new_feature    → Pianifica la feature
2. [Implementazione]    → Scrivi il codice
3. /debug               → Se trovi bug
4. /review              → Verifica qualità
5. /optimize            → Se serve ottimizzare
6. /document            → Documenta la feature
7. /commit              → Committa e pusha
8. /deploy              → Prima del rilascio
```

---

## 💡 Best Practices

### Usali nell'ordine giusto

```
❌ Sbagliato: /commit → /review
✅ Corretto:  /review → /commit
```

### Combinali quando serve

```
"Ho completato la feature X"
→ /review prima
→ poi /document
→ infine /commit
```

### Non saltare /review

Prima di ogni commit significativo, esegui `/review` per verificare:
- Type safety
- Security
- Performance
- Code quality

---

## ⚡ Workflow con Auto-Run

Alcuni workflow hanno step marcati con `// turbo` che si eseguono automaticamente:

- `/commit` → `pnpm check:fix:unsafe`, `git add/commit/push`
- `/debug` → `pnpm dev`, `pnpm check`
- `/review` → `pnpm check`, `pnpm check:fix`
- `/deploy` → `pnpm check:fix`, `pnpm build`, `git push`

---

## 🆚 Quale Workflow Scegliere?

| Situazione | Workflow |
|------------|----------|
| "Devo costruire questa feature nuova" | `/plan_new_feature` |
| "Devo modificare codice esistente" | `/refactor_actual_code` |
| "C'è un bug" | `/debug` |
| "Il codice è pronto?" | `/review` |
| "La pagina è lenta" | `/optimize` |
| "Sto per fare deploy" | `/deploy` |
| "Ho finito, devo committare" | `/commit` |
| "Devo documentare" | `/document` |

---

## 📂 Dove Trovare i Workflow

Tutti i workflow sono in:
```
.agent/workflows/
├── plan_new_feature.md
├── refactor_actual_code.md
├── debug.md
├── review.md
├── optimize.md
├── deploy.md
├── document.md
└── commit.md
```

Puoi modificarli per adattarli alle tue esigenze.
