# Changelog

Tutte le modifiche rilevanti a questo progetto sono documentate in questo file.

Il formato è basato su [Keep a Changelog](https://keepachangelog.com/it-IT/1.1.0/),
e questo progetto aderisce al [Semantic Versioning](https://semver.org/lang/it/).

## [Unreleased]

### Added
- Documentazione `src/data/GEMINI.md` per il Data Access Layer di Vercel Blob
- Riferimento a `src/data/` nel JIT Index del GEMINI.md principale

### Changed
- Sostituito `<img>` con `<Image />` di next/image in `ProfileSettings.tsx` per ottimizzazione LCP
- Convertito stato `orgData` in costante in `Sidebar.tsx` (era useState non necessario)
- Rimosso tipo vuoto `HeroSectionProps` in `HeroSection.tsx`

### Removed
- Dipendenza `@dnd-kit/core` (non utilizzata)
- Dipendenza `@dnd-kit/sortable` (non utilizzata)
- Dipendenza `@dnd-kit/utilities` (non utilizzata)
- Dipendenza `html5-qrcode` (non utilizzata, si usa `qrcode`)

---

## [1.0.0] - 2025-12-14

### Added
- **Next.js 16** con App Router e Turbopack
- **React 19** con supporto per React Compiler
- **Tailwind CSS v4** con configurazione moderna
- **shadcn/ui** componenti pre-configurati
- **Drizzle ORM** con Turso (LibSQL) per database edge
- **Better Auth** (opt-in) con email + Google OAuth
- **Vercel Blob** integrazione per file storage con dashboard
- **Biome** per linting e formatting unificato
- **Husky + lint-staged** per pre-commit hooks
- Landing page con Navbar, Hero, Features, Footer
- Dashboard con Sidebar responsive e pagine:
  - `/dashboard` - Overview
  - `/files` - Gestione file (upload, delete)
  - `/impostazioni` - Settings con persistenza DB
- Sistema di documentazione AI-friendly:
  - `GEMINI.md` principale con JIT Index
  - GEMINI.md dedicati per ogni sottocartella (`app/`, `components/`, `db/`, `lib/`, `data/`)
  - `docs/` con guide dettagliate
- Utility functions:
  - `src/lib/utils/export.ts` - Export PDF/Excel
  - `src/lib/utils/qr.ts` - Generazione QR codes
- Pattern implementati:
  - Server-First Architecture
  - Data Access Layer (DAL)
  - Server Actions per mutations
  - DTO pattern per sicurezza

### Security
- Validazione input con Zod
- Server-only protection per DAL
- Separazione credenziali con `.env.local`

---

## Tipi di Modifiche

- **Added** - Nuove funzionalità
- **Changed** - Modifiche a funzionalità esistenti
- **Deprecated** - Funzionalità che saranno rimosse
- **Removed** - Funzionalità rimosse
- **Fixed** - Bug fix
- **Security** - Correzioni di sicurezza

[Unreleased]: https://github.com/nuno80/nuno-nextjs16.starterkit/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/nuno80/nuno-nextjs16.starterkit/releases/tag/v1.0.0
