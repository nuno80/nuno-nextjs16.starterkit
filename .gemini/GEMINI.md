# GEMINI.md - Guida per AI Developer

> File snello per guidare l'AI nello sviluppo. Per dettagli, consultare `docs/` e i `GEMINI.md` nelle sottocartelle.

---

## 🚀 Inizio Sessione

Prima di qualsiasi attività:

1. **Leggere README.md** - Tech stack e overview progetto
2. **Leggere sempre `docs/nextjs16-reference.md`** - per ottimizzare l'uso di Next.js 16
3. **Lettura SELETTIVA** - Consulta SOLO i file rilevanti per il task corrente (vedi JIT Index)

> **⚠️ IMPORTANTE**: Non leggere l'intera documentazione - satura il contesto inutilmente.

---

## 🏗️ Tech Stack

| Categoria | Tecnologia |
|-----------|------------|
| Framework | Next.js 16 (App Router) |
| UI | React 19, Tailwind v4, shadcn/ui |
| Database | Turso (LibSQL) + Drizzle ORM |
| Auth | Better Auth (disabilitata di default) |
| File Storage | Vercel Blob |
| Code Quality | Biome + Husky |
| Package Manager | pnpm (obbligatorio) |

---

## 🎯 Principi Fondamentali

1. **Server-First Architecture** → Default: Server Components, `"use client"` solo per interattività
2. **Type Safety** → Zero `any` (usare `unknown` + type guards), Zod per validazione
3. **Security by Default** → Validazione SEMPRE, DTO pattern: mai esporre oggetti DB
4. **Separation of Concerns** → Components → Server Actions → DAL

---

## 🧠 Optimization Mindset (Reminder)

> "The best code is no code. The second best is code that already exists."

**LEVER**: **L**everage existing → **E**xtend before creating → **V**erify → **E**liminate duplication → **R**educe complexity

**Prima di creare nuovo codice, chiediti:**
1. Esiste già qualcosa di simile? → **Estendilo**
2. Posso aggiungere campi a una tabella esistente? → **Fallo**
3. Il nuovo codice è <50 righe? → Probabilmente puoi **estendere**

> **Per task complessi**: Consulta `docs/optimization-mindset.md`

---

## ⚠️ Regole Non Negoziabili

1. **Zero `any`** - Usa `unknown` o tipi espliciti
2. **Validazione input** - Sempre con Zod
3. **pnpm** - Mai npm o yarn
4. **shadcn/ui** - Per componenti UI, mai raw HTML
5. **Biome** - Deve passare prima di ogni commit

---

## 📁 JIT Index (what to open, not what to paste)

### Struttura Codice (con GEMINI.md dedicati)

| Directory | Scopo | Guida Dedicata |
|-----------|-------|----------------|
| `src/app/` | Pages e API routes | [src/app/GEMINI.md](src/app/GEMINI.md) |
| `src/components/` | Componenti React | [src/components/GEMINI.md](src/components/GEMINI.md) |
| `src/lib/` | Business Logic, DAL, Validazioni | [src/lib/GEMINI.md](src/lib/GEMINI.md) |
| `src/db/` | Schemi Drizzle | [src/db/GEMINI.md](src/db/GEMINI.md) |

### Documentazione Dettagliata (`docs/`)

| Tipo di Task | File da Consultare |
|--------------|-------------------|
| Database query/schema | `docs/database.md` |
| Autenticazione/autorizzazione | `docs/authentication.md` |
| Setup credenziali | `docs/environment-setup.md` |
| Pattern Next.js 16 (quick) | `docs/nextjs16-reference.md` |
| SEO/Metadata/Sitemap/OG | `docs/nextjs16-guide.md` |
| Linting/formatting | `docs/code-style.md` |

### Stato Progetto

| Risorsa | File |
|---------|------|
| Task da fare | `.gemini/project-state/BACKLOG.md` |
| Debug / Logica app | `.gemini/project-state/APP-LOGIC.md` |

### Quick Find Commands

```bash
# Cerca una funzione
rg -n "functionName" src/**

# Trova un componente
rg -n "export.*ComponentName" src/components

# Trova route handlers
rg -n "export.*(GET|POST)" src/app/api
```

---

## ⌨️ Comandi Essenziali

```bash
# Sviluppo
pnpm dev              # Start server

# Code Quality
pnpm check            # Verifica errori
pnpm check:fix        # Corregge errori

# Database
pnpm db:push          # Push schema
pnpm db:studio        # Apri Drizzle Studio
```

---

## 🤔 Workflow Pre-Coding (OBBLIGATORIO)

**Prima di scrivere qualsiasi codice**, per ogni nuovo task:

1. **Analizza la richiesta** - Leggi attentamente cosa viene chiesto
2. **Fai domande di chiarimento** - Non procedere con assunzioni
3. **Attendi conferma** - Solo dopo aver ricevuto risposte, procedi

> **⚠️ REGOLA**: Mai scrivere codice basandosi su assunzioni.

---

## 📦 Estensioni per Progetti Specifici

Quando ricevi un **PRD dedicato** per un nuovo modulo/app, crea un'estensione:

```
.gemini/extensions/[nome-progetto]/
├── gemini-extension.json  # Config obbligatoria
└── GEMINI.md              # Contesto progetto specifico
```

---

## 📚 Riferimenti Dettagliati

Per approfondimenti, consultare sempre la cartella `docs/`:

- [Environment Setup](docs/environment-setup.md) - Credenziali
- [Database](docs/database.md) - Drizzle + Turso
- [Authentication](docs/authentication.md) - Better Auth
- [Code Style](docs/code-style.md) - Biome
- [Next.js 16](docs/nextjs16-reference.md) - Pattern e best practice
