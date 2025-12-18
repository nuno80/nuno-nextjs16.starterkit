---
description: Investigazione e risoluzione bug con analisi strutturata
---

# /debug - Investigazione Bug

Workflow strutturato per analizzare e risolvere bug.

## Quando Usarlo

- Bug report dall'utente
- Comportamento inaspettato
- Errori in console/log
- Test che falliscono

---

## Workflow

### 1. Raccogli Informazioni

Prima di tutto, chiedi o verifica:

- **Cosa dovrebbe succedere?**
- **Cosa succede invece?**
- **Come riprodurre?** (steps esatti)
- **Quando ha iniziato?** (commit, deploy recente?)

### 2. Riproduci il Bug

// turbo
```bash
pnpm dev
```

Naviga alla pagina/funzionalità interessata e verifica il comportamento.

### 3. Analizza i Log

Controlla in ordine:

1. **Console browser** - Errori JS, network failures
2. **Terminal server** - Errori Next.js, stack traces
3. **Database** - Query fallite (usa `pnpm db:studio`)

### 4. Isola la Causa

Usa queste tecniche:

```bash
# Cerca errori nel codice
rg -n "ERROR_MESSAGE" src/

# Trova uso di una funzione
rg -n "functionName" src/

# Controlla git blame
git log --oneline -10 -- path/to/file.tsx
```

### 5. Forma un'Ipotesi

Scrivi:

```markdown
## Ipotesi Bug

**Sintomo**: [cosa succede]
**Causa probabile**: [perché succede]
**File coinvolti**: [lista file]
**Fix proposto**: [come risolverlo]
```

### 6. Implementa Fix

Modifica il codice seguendo il fix proposto.

### 7. Verifica Fix

// turbo
```bash
pnpm check
```

Poi testa manualmente che il bug sia risolto.

### 8. Documenta

Se il bug era significativo, aggiorna:
- `.gemini/project-state/APP-LOGIC.md` se cambia la logica
- Aggiungi commento nel codice se la fix non è ovvia

---

## Anti-Pattern

- ❌ NON fare fix random sperando funzionino
- ❌ NON ignorare errori "minori" in console
- ❌ NON committare senza verificare che il fix funzioni
