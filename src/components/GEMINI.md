# GEMINI.md - src/components (UI Components)

> 📌 **STARTER KIT - Living Document**: Questo file fornisce le linee guida base per la cartella `components/`. Aggiornalo man mano che il tuo progetto cresce.

---

## 🎯 Scopo

Questa cartella contiene tutti i **componenti React** riutilizzabili dell'applicazione.

---

## 📁 Struttura Attuale

```
src/components/
├── ui/               # Componenti shadcn/ui (generati)
├── dashboard/        # Componenti specifici dashboard
├── landing/          # Componenti landing page
└── [feature]/        # Componenti per feature specifiche
```

---

## ✅ Pattern da Seguire

### Server Component (Default)

```typescript
// src/components/example/Card.tsx
/**
 * FILE: src/components/example/Card.tsx
 * TYPE: Server Component
 */
interface CardProps {
  title: string
  children: React.ReactNode
}

export function Card({ title, children }: CardProps) {
  return (
    <div className="rounded-lg border p-4">
      <h3 className="font-semibold">{title}</h3>
      {children}
    </div>
  )
}
```

### Client Component (Solo quando necessario)

```typescript
// src/components/example/Counter.tsx
"use client"

/**
 * FILE: src/components/example/Counter.tsx
 * TYPE: Client Component
 *
 * WHY CLIENT:
 * - Usa useState per gestire il contatore
 * - Richiede interazione utente (click)
 */
import { useState } from "react"
import { Button } from "@/components/ui/button"

export function Counter() {
  const [count, setCount] = useState(0)
  return (
    <Button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </Button>
  )
}
```

---

## 🎨 Regole Styling

1. **Usare sempre shadcn/ui** per componenti UI base
2. **Tailwind CSS** per styling custom
3. **MAI usare** stili inline o CSS modules

### Aggiungere Componenti shadcn/ui

```bash
pnpm dlx shadcn@latest add [component-name]
```

---

## ❌ Anti-Pattern

- **NON** creare componenti UI da zero se esiste in shadcn/ui
- **NON** usare `"use client"` senza motivo (vedi Decision Matrix in GEMINI.md root)
- **NON** hardcodare colori → usa le variabili CSS di Tailwind

---

## 🔗 File di Riferimento

- Button: [button.tsx](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/components/ui/button.tsx)
- Card: [card.tsx](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/components/ui/card.tsx)

---

## 📝 TODO per il Tuo Progetto

- [ ] Documentare i componenti custom aggiunti
- [ ] Aggiungere esempi di composizione
- [ ] Creare una gallery/storybook se necessario
