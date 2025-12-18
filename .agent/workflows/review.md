---
description: Code review checklist per qualità e sicurezza
---

# /review - Code Review Checklist

Checklist strutturata per revisione codice prima di merge/deploy.

## Quando Usarlo

- Prima di merge su main
- Dopo implementazione feature
- Review codice di altri
- Self-review prima di commit

---

## Checklist

### 1. Type Safety ✅

// turbo
```bash
pnpm check
```

- [ ] Zero errori TypeScript
- [ ] Nessun uso di `any` (usare `unknown` + type guards)
- [ ] Props tipizzate correttamente
- [ ] Return types espliciti per funzioni pubbliche

### 2. Validazione Input ✅

- [ ] Tutti gli input utente validati con **Zod**
- [ ] Form usano `react-hook-form` + `@hookform/resolvers`
- [ ] API routes validano body/params
- [ ] Server Actions validano FormData

### 3. Security ✅

- [ ] Nessun dato sensibile esposto al client
- [ ] `server-only` usato dove necessario
- [ ] Query parametrizzate (no SQL injection)
- [ ] Auth check prima di operazioni sensibili

### 4. Performance ✅

- [ ] Componenti client solo dove necessario (`"use client"`)
- [ ] Immagini usano `next/image`
- [ ] Lazy loading per componenti pesanti
- [ ] Nessun fetch duplicato

### 5. Code Quality ✅

// turbo
```bash
pnpm check:fix
```

- [ ] Import organizzati
- [ ] Nessun codice commentato lasciato
- [ ] Nomi variabili/funzioni descrittivi
- [ ] Funzioni < 50 righe (preferibilmente)

### 6. Database ✅

- [ ] Query via DAL (`src/lib/dal/` o `src/data/`)
- [ ] Nessuna query diretta da componenti/pages
- [ ] Transazioni per operazioni multiple
- [ ] Index per query frequenti

### 7. Error Handling ✅

- [ ] Try/catch dove appropriato
- [ ] Errori user-friendly mostrati all'utente
- [ ] Errori loggati per debug
- [ ] Fallback UI per stati di errore

---

## Output

Dopo la review, produci:

```markdown
## Review Summary

**File reviewati**: [lista]
**Issues trovati**: X critici, Y minori
**Verdict**: ✅ Approvato / ⚠️ Richiede modifiche / ❌ Bocciato

### Issues Critici
- [descrizione + file:linea]

### Issues Minori
- [descrizione + file:linea]

### Note Positive
- [cosa è stato fatto bene]
```
