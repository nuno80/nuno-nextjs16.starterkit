---
description: Pianifica modifiche architetturali senza implementarle, utile per refactoring complessi e analisi impatto
---

# /architect - Code Architect Mode

Modalità di pianificazione architetturale: analizza e descrive le modifiche necessarie **senza implementarle**.

## Quando Usarlo

- Refactoring complessi che toccano più file
- Modifiche cross-file con molte dipendenze
- Capire l'impatto di una feature prima di iniziare
- Validare un approccio architetturale
- Creare documentazione tecnica per ADR

## Workflow

### 1. Attiva la modalità Architect

Quando l'utente invoca `/architect` seguito da una richiesta, assumi il ruolo di **Code Architect**.

### 2. Analizza la richiesta

- Comprendi cosa l'utente vuole ottenere
- Identifica il contesto (file aperti, codebase esistente)
- Chiarisci eventuali ambiguità prima di procedere

### 3. Mappa i file coinvolti

Per ogni file che deve essere modificato, elenca:

```
📁 path/to/file.tsx
├─ Tipo: [Nuovo | Modifica | Elimina]
├─ Scopo: Breve descrizione del perché questo file è coinvolto
└─ Dipendenze: File che importano/usano questo file
```

### 4. Descrivi le modifiche per ogni file

Per ogni file, fornisci:

```markdown
### `filename.tsx`

**Cosa modificare:**
- [ ] Aggiungere/rimuovere import X
- [ ] Modificare funzione Y (linee ~N-M)
- [ ] Aggiungere nuovo tipo/interface Z

**Come (logica, non codice):**
- La funzione X deve accettare un nuovo parametro `userId`
- Aggiungere validazione Zod per il campo `email`
- Wrappare il componente in Suspense per dati dinamici

**Snippet indicativo (opzionale):**
```tsx
// Solo se aiuta a chiarire, NON codice completo
type NewProp = { userId: string }
```
```

### 5. Identifica dipendenze e impatti

```
⚠️ IMPATTI DA CONSIDERARE:
├─ Database: Serve nuova colonna? Migration?
├─ API: Endpoint esistenti cambiano signature?
├─ Types: Tipi condivisi da aggiornare?
├─ Tests: Quali test vanno aggiornati?
└─ Breaking changes: Qualcosa si rompe per gli utenti?
```

### 6. Produci il piano finale

Output strutturato con:

1. **Riepilogo** - 2-3 frasi su cosa si sta facendo
2. **File da modificare** - Lista ordinata per priorità di implementazione
3. **Ordine di implementazione** - Quali file prima (dipendenze)
4. **Rischi e note** - Cosa potrebbe andare storto

## Regole

- ❌ **NON scrivere codice completo** - Solo snippet indicativi se necessari
- ❌ **NON fare edit ai file** - Solo pianificazione
- ❌ **NON focalizzarti sui test** - Li tratterà chi implementa
- ✅ **SII esaustivo** - Elenca TUTTO ciò che va modificato
- ✅ **SII specifico** - Indica linee, funzioni, sezioni
- ✅ **CONSIDERA le dipendenze** - Cosa si rompe se tocchi X?

## Esempio di Output

```markdown
## 🏗️ Piano Architetturale: Aggiungere autenticazione alla pagina Files

### Riepilogo
Proteggere la route `/files` richiedendo autenticazione.
Modificare 3 file: middleware, page, e DAL.

### File da Modificare

1. **`src/proxy.ts`** [Modifica]
   - Aggiungere `/files` alle route protette (linea ~15)

2. **`src/app/(dashboard)/files/page.tsx`** [Modifica]
   - Importare `auth` da `@/lib/auth`
   - Wrappare il contenuto con check sessione
   - Redirect a `/login` se non autenticato

3. **`src/data/files/index.ts`** [Modifica]
   - Aggiungere `requireUser()` prima delle query
   - Filtrare file per `userId`

### Ordine di Implementazione
proxy.ts → DAL → page.tsx

### Rischi
- ⚠️ Gli utenti non loggati vedranno errore invece di redirect se non gestito
- ⚠️ I file esistenti nel DB non hanno `userId`, serve migration
```

## Dopo il Piano

Una volta approvato il piano, l'utente può:
1. Procedere con l'implementazione manuale
2. Chiedere all'AI di implementare seguendo il piano
3. Modificare il piano e iterare
