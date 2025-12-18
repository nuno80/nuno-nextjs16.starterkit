---
description: Pre-deploy checklist per rilascio sicuro in produzione
---

# /deploy - Pre-Deploy Checklist

Checklist da completare prima di ogni deploy in produzione.

## Quando Usarlo

- Prima di push su branch production
- Prima di deploy Vercel manuale
- Dopo merge di feature importante
- Hotfix in produzione

---

## Checklist Pre-Deploy

### 1. Code Quality ✅

// turbo
```bash
pnpm check:fix
```

- [ ] Zero errori Biome
- [ ] Zero warning TypeScript

### 2. Build ✅

// turbo
```bash
pnpm build
```

- [ ] Build completata senza errori
- [ ] Nessun warning critico
- [ ] Bundle size accettabile

### 3. Environment Variables ✅

Verifica che tutte le env vars siano configurate in produzione:

```bash
# Confronta con .env.local.exemple
cat .env.local.exemple
```

Checklist env vars:
- [ ] `DATABASE_URL` - Punta al DB produzione
- [ ] `DATABASE_AUTH_TOKEN` - Token Turso produzione
- [ ] `BLOB_READ_WRITE_TOKEN` - Token Vercel Blob
- [ ] `BETTER_AUTH_SECRET` - Secret produzione (diverso da dev!)
- [ ] `BETTER_AUTH_URL` - URL produzione

### 4. Database ✅

- [ ] Schema sincronizzato (`pnpm db:push` già eseguito in prod)
- [ ] Migrazioni applicate se necessarie
- [ ] Backup recente disponibile

### 5. Feature Flags ✅

Controlla `src/lib/config.ts`:

```typescript
// Verifica che le feature siano abilitate/disabilitate correttamente
export const config = {
  auth: {
    enabled: true, // ← Corretto per produzione?
  },
}
```

### 6. Git Status ✅

// turbo
```bash
git status
git log origin/main..HEAD --oneline
```

- [ ] Working directory pulito
- [ ] Tutti i commit pushati
- [ ] Branch corretto (main/production)

---

## Deploy

// turbo
```bash
git push origin main
```

Vercel farà deploy automatico.

---

## Post-Deploy Verification

1. **Apri il sito produzione** e verifica:
   - [ ] Home page carica
   - [ ] Login funziona (se auth abilitata)
   - [ ] Feature principale funziona

2. **Controlla Vercel Dashboard**:
   - [ ] Deploy completed
   - [ ] Nessun errore nei Function Logs

3. **Smoke Test**:
   - [ ] Naviga 3-4 pagine principali
   - [ ] Testa un form se presente
   - [ ] Verifica immagini caricano

---

## Rollback (se necessario)

```bash
# Via Vercel Dashboard: Deployments → Seleziona precedente → Promote to Production

# Oppure via git
git revert HEAD
git push origin main
```
