# GEMINI.md - src/db (Database Layer)

> 📌 **STARTER KIT - Living Document**: Questo file fornisce le linee guida base per la cartella `db/`. Aggiornalo man mano che il tuo progetto cresce.

---

## 🎯 Scopo

Questa cartella contiene la configurazione **Drizzle ORM** e tutti gli **schemi del database**.

---

## 📁 Struttura Attuale

```
src/db/
├── index.ts          # Connessione DB (singleton)
├── schema.ts         # Export di tutti gli schemi
└── schema/           # Schemi per entità
    ├── users.ts
    └── [entity].ts
```

---

## ✅ Pattern da Seguire

### Definizione Schema

```typescript
// src/db/schema/users.ts
import { sqliteTable, text, integer } from "drizzle-orm/sqlite-core"

export const users = sqliteTable("users", {
  id: text("id").primaryKey(),
  email: text("email").notNull().unique(),
  name: text("name").notNull(),
  createdAt: integer("created_at", { mode: "timestamp" })
    .notNull()
    .$defaultFn(() => new Date()),
})
```

### Export Centralizzato

```typescript
// src/db/schema.ts
export * from "./schema/users"
// export * from "./schema/posts"  // Aggiungere qui nuovi schemi
```

### Connessione DB

```typescript
// src/db/index.ts
import { drizzle } from "drizzle-orm/libsql"
import { createClient } from "@libsql/client"
import * as schema from "./schema"

const client = createClient({
  url: process.env.DATABASE_URL!,
  authToken: process.env.DATABASE_AUTH_TOKEN,
})

export const db = drizzle(client, { schema })
```

---

## ⌨️ Comandi Database

```bash
# Push schema al DB (sviluppo)
pnpm db:push

# Genera migrazione
pnpm db:generate

# Applica migrazioni (produzione)
pnpm db:migrate

# Apri Drizzle Studio (GUI)
pnpm db:studio
```

---

## ❌ Anti-Pattern

- **NON** modificare direttamente il DB in produzione
- **NON** usare `any` nei tipi Drizzle
- **NON** creare nuove connessioni → usa sempre `db` da `index.ts`

---

## 🔗 File di Riferimento

- Connessione: [index.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/db/index.ts)
- Schema Export: [schema.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/db/schema.ts)
- Config Drizzle: [drizzle.config.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/drizzle.config.ts)

---

## 📝 TODO per il Tuo Progetto

- [ ] Aggiungere schemi per le nuove entità
- [ ] Documentare le relazioni tra tabelle
- [ ] Configurare seed script se necessario
