# Optimization Mindset

> "The best code is no code. The second best code is code that already exists and works."

Questo documento stabilisce i principi di ottimizzazione per ridurre la complessità del codice. Leggilo quando stai progettando **nuove feature complesse** o facendo **refactoring significativi**.

---

## 🎯 The LEVER Framework

| Lettera | Principio | Azione |
|---------|-----------|--------|
| **L** | Leverage existing patterns | Cerca codice simile nel progetto |
| **E** | Extend before creating | Estendi funzioni/tabelle esistenti |
| **V** | Verify through reactivity | Usa revalidation invece di sync manuale |
| **E** | Eliminate duplication | Rimuovi codice ridondante |
| **R** | Reduce complexity | Semplifica sempre |

---

## 📋 Pre-Implementation Checklist

Prima di scrivere codice, completa questa analisi:

### 1. Pattern Recognition (5 min)

- [ ] Quali funzionalità simili esistono già?
- [ ] Quali query/mutation gestiscono dati correlati?
- [ ] Quali componenti UI mostrano informazioni simili?
- [ ] Quali hook gestiscono stato simile?

### 2. Reuse Opportunities

- [ ] Posso estendere una tabella esistente invece di crearne una nuova?
- [ ] Posso aggiungere campi a una query esistente?
- [ ] Posso migliorare un hook con nuove computed properties?
- [ ] Posso modificare un componente con rendering condizionale?

---

## 📊 Decision Framework: Extend vs Create

| Criterio | Estendi (+) | Crea Nuovo (-) |
|----------|-------------|----------------|
| Struttura dati simile esiste | +3 | -3 |
| Posso riusare indici esistenti | +2 | -2 |
| Query esistenti ritornano dati correlati | +3 | -3 |
| Componenti UI mostrano info simili | +2 | -2 |
| Richiederebbe <50 righe per estendere | +3 | -3 |
| Introdurrebbe dipendenze circolari | -5 | +5 |
| Dominio significativamente diverso | -3 | +3 |

**Score > 5**: Estendi codice esistente
**Score < -5**: Crea nuova implementazione
**Score -5 to 5**: Richiede analisi più approfondita

---

## 🛠️ Three-Pass Approach

### Pass 1: Discovery (No Code)
- Trova tutto il codice correlato esistente
- Documenta i pattern attuali
- Identifica i punti di estensione

### Pass 2: Design (Minimal Code)
- Scrivi solo le modifiche alle interfacce
- Aggiorna le definizioni dei tipi
- Pianifica il flusso dati

### Pass 3: Implementation (Optimized Code)
- Implementa con massimo riuso
- Aggiungi solo logica essenziale
- Documenta il perché delle scelte

---

## 🚫 Anti-Pattern da Evitare

### 1. "Just One More Table" Trap

Ogni nuova tabella aggiunge:
- Complessità dello schema
- Necessità di join
- Sfide di sincronizzazione
- Overhead di migrazione

**Chiediti**: Questi dati possono vivere in una tabella esistente?

### 2. "Similar But Different" Excuse

Prima di creare `getUserTrialStatus` quando esiste già `getUserStatus`:
- `getUserStatus` può ritornare anche i campi trial?
- Posso aggiungere un parametro `includeTrial`?
- Posso derivare ciò che serve con computed properties?

### 3. "UI Drives Database" Mistake

MAI creare strutture database per matchare i componenti UI. Invece:
- Salva i dati nella forma più logica
- Usa query per trasformare per la UI
- Lascia che i componenti computino i valori di display

---

## ✅ Review Checklist

Prima di fare commit del codice ottimizzato:

- [ ] Estesa tabella esistente invece di crearne una nuova
- [ ] Riusate query esistenti con aggiunte
- [ ] Sfruttati hook e componenti esistenti
- [ ] Nessuna logica di state management duplicata
- [ ] Documentato perché sono state scelte le estensioni
- [ ] Mantenuta backward compatibility
- [ ] Campi aggiunti sono opzionali (`v.optional` / Zod `.optional()`)
- [ ] Nessuna dipendenza circolare introdotta
- [ ] Performance uguale o migliore
- [ ] Riduzione codice >50%

---

## 📚 Riferimenti

- [GEMINI.md principale](../.gemini/GEMINI.md) - Guida root
- [Next.js 16 Reference](./nextjs16-reference.md) - Pattern tecnici dettagliati
- [Database](./database.md) - Drizzle ORM patterns
