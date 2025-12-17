# GEMINI.md - src/lib (Business Logic & Utilities)

> 📌 **STARTER KIT - Living Document**: Questo file fornisce le linee guida base per la cartella `lib/`. Aggiornalo man mano che il tuo progetto cresce.

---

## 🎯 Scopo

Questa cartella contiene la **logica di business**, **utilities**, e il **Data Access Layer (DAL)**.

---

## 📁 Struttura Attuale

```
src/lib/
├── auth.ts           # Configurazione Better Auth
├── config.ts         # Feature flags (es. auth.enabled)
├── utils.ts          # Utility generiche (cn, ecc.)
├── dal/              # Data Access Layer
│   └── *.ts          # Funzioni per accesso DB
└── validations/      # Schemi Zod
    └── *.ts          # Schemi di validazione
```

---

## ✅ Pattern da Seguire

### Data Access Layer (DAL)

Tutte le operazioni DB passano dal DAL, MAI direttamente dalle pagine.

```typescript
// src/lib/dal/users.ts
import { db } from "@/db"
import { users } from "@/db/schema"
import { eq } from "drizzle-orm"

export async function getUserById(id: string) {
  const result = await db.select().from(users).where(eq(users.id, id))
  return result[0] ?? null
}

export async function createUser(data: { email: string; name: string }) {
  return db.insert(users).values(data).returning()
}
```

### Validazione con Zod

```typescript
// src/lib/validations/user.ts
import { z } from "zod"

export const createUserSchema = z.object({
  email: z.string().email("Email non valida"),
  name: z.string().min(2, "Nome troppo corto"),
})

export type CreateUserInput = z.infer<typeof createUserSchema>
```

### Utilizzo nelle Server Actions

```typescript
// src/app/users/actions.ts
"use server"

import { createUserSchema } from "@/lib/validations/user"
import { createUser } from "@/lib/dal/users"

export async function createUserAction(formData: FormData) {
  const parsed = createUserSchema.safeParse({
    email: formData.get("email"),
    name: formData.get("name"),
  })

  if (!parsed.success) {
    return { error: parsed.error.flatten() }
  }

  const user = await createUser(parsed.data)
  return { success: true, user }
}
```

---

## ❌ Anti-Pattern

- **NON** importare `db` direttamente nelle pagine o componenti
- **NON** usare `any` nelle funzioni DAL → tipizza sempre
- **NON** esporre oggetti DB direttamente → usa DTO pattern

---

## 🔗 File di Riferimento

- Config: [config.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/lib/config.ts)
- Auth: [auth.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/lib/auth.ts)
- Utils: [utils.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/lib/utils.ts)

---

## 📝 TODO per il Tuo Progetto

- [ ] Aggiungere funzioni DAL per le nuove entità
- [ ] Creare schemi Zod per ogni input utente
- [ ] Documentare i DTO utilizzati
