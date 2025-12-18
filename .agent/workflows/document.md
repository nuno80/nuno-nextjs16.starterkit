---
description: Aggiorna documentazione dopo completamento feature
---

# /document - Documentazione Feature

Workflow per documentare feature completate e mantenere la documentazione aggiornata.

## Quando Usarlo

- Dopo aver completato una feature
- Quando aggiungi nuove API
- Quando cambi comportamenti esistenti
- Prima di passare il progetto ad altri

---

## Workflow

### 1. Identifica Cosa Documentare

Per la feature appena completata, rispondi:

- **What**: Cosa fa questa feature?
- **Why**: Perché esiste? Quale problema risolve?
- **How**: Come si usa? (API, UI, config)
- **Where**: Dove si trova il codice?

### 2. Aggiorna GEMINI.md Appropriato

Identifica il GEMINI.md corretto da aggiornare:

| Tipo Modifica | File da Aggiornare |
|---------------|-------------------|
| Nuova pagina/route | `src/app/GEMINI.md` |
| Nuovo componente | `src/components/GEMINI.md` |
| Nuova tabella DB | `src/db/GEMINI.md` |
| Nuova funzione DAL | `src/lib/GEMINI.md` o `src/data/GEMINI.md` |
| Setup/Config | `docs/` appropriato |

### 3. Template Documentazione

Per nuove funzionalità, usa questo template:

```markdown
## [Nome Feature]

**Scopo**: Breve descrizione (1 riga)

**File principali**:
- `path/to/main.tsx` - Descrizione
- `path/to/helper.ts` - Descrizione

**Uso**:
\`\`\`typescript
// Esempio di codice
import { myFeature } from '@/lib/myFeature'
myFeature.doSomething()
\`\`\`

**Note**:
- Punto importante 1
- Punto importante 2
```

### 4. Aggiorna README (se necessario)

Se la feature è user-facing o cambia il setup:

```markdown
## Features (in README.md)

- ✅ **Nuova Feature** - Breve descrizione
```

### 5. Aggiorna BACKLOG (se necessario)

Se hai completato task dal backlog:

```bash
# Apri e aggiorna
.gemini/project-state/BACKLOG.md
```

Cambia `[ ]` in `[x]` per i task completati.

### 6. Aggiorna APP-LOGIC (se cambia la logica)

Se la feature cambia il comportamento dell'app:

```bash
.gemini/project-state/APP-LOGIC.md
```

Documenta:
- Nuovo flusso utente
- Nuove regole business
- Cambiamenti a flussi esistenti

---

## Checklist Finale

- [ ] GEMINI.md appropriato aggiornato
- [ ] Esempi di codice funzionanti
- [ ] Link ai file corretti
- [ ] README aggiornato (se user-facing)
- [ ] BACKLOG task marcati completati
- [ ] APP-LOGIC aggiornato (se logica cambia)

---

## Anti-Pattern

- ❌ NON lasciare TODO nella documentazione
- ❌ NON documentare codice che non esiste ancora
- ❌ NON copiare codice - usa link ai file
- ❌ NON essere prolisso - sii conciso e preciso
