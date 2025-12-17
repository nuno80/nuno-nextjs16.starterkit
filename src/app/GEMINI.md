# GEMINI.md - src/app (App Router)

> 📌 **STARTER KIT - Living Document**: Questo file fornisce le linee guida base per la cartella `app/`. Aggiornalo man mano che il tuo progetto cresce.

---

## 🎯 Scopo

Questa cartella contiene tutte le **pagine** e **API routes** dell'applicazione Next.js 16 (App Router).

---

## 📁 Struttura Attuale

```
src/app/
├── (auth)/           # Route group per autenticazione (login, signup)
├── (dashboard)/      # Route group per area protetta
├── api/              # API Route Handlers
├── layout.tsx        # Layout globale
└── page.tsx          # Home page
```

---

## ✅ Pattern da Seguire

### Pagine (Server Components di default)

```typescript
// src/app/esempio/page.tsx
export default async function EsempioPage() {
  // Fetch dati direttamente qui (Server Component)
  const data = await fetchData()
  return <div>{data}</div>
}
```

### Metadata

```typescript
// src/app/esempio/page.tsx
import type { Metadata } from "next"

export const metadata: Metadata = {
  title: "Titolo Pagina",
  description: "Descrizione per SEO",
}
```

### Server Actions

Definirle in file separati con `"use server"`:

```typescript
// src/app/esempio/actions.ts
"use server"

import { z } from "zod"

const schema = z.object({ name: z.string() })

export async function createItem(formData: FormData) {
  const parsed = schema.safeParse({ name: formData.get("name") })
  if (!parsed.success) return { error: "Invalid data" }
  // ... logica DB via DAL
}
```

### API Routes

```typescript
// src/app/api/esempio/route.ts
import { NextResponse } from "next/server"

export async function GET() {
  return NextResponse.json({ message: "Hello" })
}

export async function POST(request: Request) {
  const body = await request.json()
  // Validare SEMPRE con Zod
  return NextResponse.json({ received: body })
}
```

---

## ❌ Anti-Pattern

- **NON** usare `"use client"` nelle pagine se non strettamente necessario
- **NON** chiamare direttamente il DB dalle pagine → usa il DAL (`src/lib/dal/`)
- **NON** esporre dati sensibili nelle API senza validazione

---

## 🔗 File di Riferimento

- Layout: [layout.tsx](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/app/layout.tsx)
- Home: [page.tsx](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/app/page.tsx)
- API example: `src/app/api/` (quando implementato)

---

## 📝 TODO per il Tuo Progetto

- [ ] Documentare le nuove pagine aggiunte
- [ ] Aggiungere esempi di route dinamiche
- [ ] Descrivere middleware personalizzati
