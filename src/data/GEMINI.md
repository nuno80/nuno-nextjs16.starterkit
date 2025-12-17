# GEMINI.md - src/data (Data Access Layer)

> 📌 **STARTER KIT - Living Document**: Questo file documenta il Data Access Layer (DAL) centralizzato per operazioni server-side.

---

## 🎯 Scopo

Questa cartella contiene il **Data Access Layer (DAL)** centralizzato che gestisce:
- **Accesso al database** con pattern server-only
- **Server Actions** per operazioni CRUD
- **Integrazione Vercel Blob** per file storage

---

## 📁 Struttura

```
src/data/
├── index.ts          # Export centralizzato di tutti i moduli DAL
├── server-only.ts    # Utility server-only (requireUser, requireAdminUser)
├── files/            # DAL per gestione file (Vercel Blob)
│   ├── index.ts      # Funzioni DAL: getAllFiles, getFileById, createFile, deleteFile
│   └── actions.ts    # Server Actions: fetchAllFiles, deleteFileAction
└── users/            # DAL per gestione utenti
    └── index.ts      # Funzioni DAL per utenti
```

---

## ✅ Pattern da Seguire

### Server-Only Protection

Tutti i moduli DAL usano `import 'server-only'` per garantire che non vengano mai importati in componenti client.

```typescript
// src/data/files/index.ts
import 'server-only'
import { db, files } from '@/db'





export async function getAllFiles() {
  const allFiles = await db.select().from(files).orderBy(desc(files.uploadedAt))
  return { success: true, data: allFiles }
}
```

### Server Actions per Client

Le operazioni che devono essere chiamate dal client usano Server Actions:

```typescript
// src/data/files/actions.ts
'use server'

import { del } from '@vercel/blob'
import { deleteFile } from './index'

export async function deleteFileAction(id: number) {
  // 1. Elimina record dal DB
  const result = await deleteFile(id)
  // 2. Elimina file da Vercel Blob
  if (result.data) await del(result.data.blobUrl)
  return { success: true }
}
```

---

## 📤 Vercel Blob Integration

Il modulo `files/` gestisce l'upload e storage di file con **Vercel Blob**:

- **Upload**: Gestito da API route `/api/files/route.ts`
- **Metadata**: Salvati nel database (tabella `files`)
- **Delete**: Server Action che elimina sia da DB che da Blob storage

### Configurazione Richiesta

```env
# .env.local
BLOB_READ_WRITE_TOKEN=vercel_blob_rw_xxxxx
```

### Uso nella Dashboard

La pagina `/files` nella dashboard permette:
- ✅ Drag & drop upload
- ✅ Visualizzazione lista file
- ✅ Eliminazione file
- ✅ Validazione tipo file (immagini, PDF, documenti)
- ✅ Limite dimensione (15MB)

---

## 🔗 File di Riferimento

- Export centralizzato: [index.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/data/index.ts)
- Files DAL: [files/index.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/data/files/index.ts)
- Files Actions: [files/actions.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/data/files/actions.ts)
- API Upload: [api/files/route.ts](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/src/app/api/files/route.ts)
- Guida Vercel Blob: [docs/guida-vercel-blob.md](file:///home/nuno/programmazione/nuno-nextjs16.starterkit/docs/guida-vercel-blob.md)

---

## ❌ Anti-Pattern

- **NON** importare funzioni DAL in componenti client → usa Server Actions
- **NON** esporre `blobUrl` direttamente senza validazione
- **NON** eliminare file dal DB senza eliminare da Blob (data inconsistency)

---

## 📝 Note

Questa cartella è complementare a `src/lib/dal/` che può contenere logica DAL più generica. La separazione permette di:

1. Tenere il DAL per Vercel Blob isolato
2. Avere un namespace chiaro per imports (`@/data/files`)
3. Facilitare l'estensione con altri moduli (es. `src/data/posts/`)
