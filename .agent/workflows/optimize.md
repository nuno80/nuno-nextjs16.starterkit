---
description: Analisi performance e ottimizzazione bundle/pagine
---

# /optimize - Performance Optimization

Workflow per analizzare e migliorare le performance dell'app.

## Quando Usarlo

- Pagine lente a caricare
- Bundle size troppo grande
- Query DB lente
- Prima di launch in produzione

---

## Workflow

### 1. Build Analysis

// turbo
```bash
pnpm build
```

Analizza l'output:
- **Route sizes** - Pagine > 100KB sono sospette
- **First Load JS** - Dovrebbe essere < 100KB per pagine critiche
- **Static vs Dynamic** - Preferisci Static dove possibile

### 2. Bundle Analysis (se necessario)

```bash
# Installa analyzer se non presente
pnpm add -D @next/bundle-analyzer

# Poi esegui
ANALYZE=true pnpm build
```

Cerca:
- Librerie duplicate
- Import non tree-shakati
- Dipendenze inutilizzate

### 3. Component Check

Verifica per ogni componente client:

```markdown
| Componente | Motivo "use client" | Può essere Server? |
|------------|---------------------|-------------------|
| Button.tsx | onClick handler     | No                |
| Card.tsx   | Nessuno!            | ✅ Sì             |
```

### 4. Image Optimization

Verifica che tutte le immagini usino:

```tsx
import Image from 'next/image'

// ✅ Corretto
<Image src="/hero.jpg" width={800} height={600} alt="Hero" />

// ❌ Evita
<img src="/hero.jpg" alt="Hero" />
```

### 5. Database Query Check

Apri Drizzle Studio e controlla:

// turbo
```bash
pnpm db:studio
```

- Query N+1 (loop di select)
- Missing indexes
- Select * invece di select specifici

### 6. Lazy Loading

Per componenti pesanti:

```tsx
import dynamic from 'next/dynamic'

const HeavyChart = dynamic(() => import('./HeavyChart'), {
  loading: () => <Skeleton />,
})
```

---

## Output

```markdown
## Performance Report

### Bundle
- Total First Load JS: XXX KB
- Largest chunks: [lista]

### Findings
| Issue | Impatto | Fix |
|-------|---------|-----|
| [descrizione] | Alto/Medio/Basso | [soluzione] |

### Quick Wins
1. [fix facile con alto impatto]
2. ...

### Richiede Refactoring
1. [fix complesso]
2. ...
```
