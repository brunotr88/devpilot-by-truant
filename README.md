# DevPilot by Truant

Un ecosistema completo di sviluppo per **Claude Code**. 12 skill coordinate + 4 agenti specializzati che ti guidano in ogni fase dello sviluppo software — dall'idea al merge.

Ispirato a [obra/superpowers](https://github.com/obra/superpowers) (84K+ stelle), ricostruito da zero per essere autonomo, modulare e facile da usare.

**[Italiano](#cos-e)** | **[English](#english)**

---

## Cos'e?

Claude Code e' potente ma generico. **DevPilot** gli insegna una metodologia di sviluppo strutturata con **3 Leggi Ferree**:

1. **Niente codice senza un test che fallisce prima** (TDD)
2. **Niente fix senza prima investigare la causa** (Debug)
3. **Niente "e' fatto" senza verificare davvero** (Verifica)

Piu' una pipeline completa: brainstorm -> piano -> implementa -> review -> merge.

## Cosa contiene

```
devpilot-by-truant/
├── CLAUDE.md                          # Regole del workflow (caricate automaticamente)
├── skills/                            # 12 skill coordinate
│   ├── brainstorming/                 # Progetta prima di scrivere codice
│   ├── writing-plans/                 # Piani implementativi TDD a piccoli step
│   ├── subagent-driven-development/   # Esegui i piani con sub-agenti
│   ├── dispatching-parallel-agents/   # Risolvi bug multipli in parallelo
│   ├── test-driven-development/       # Red -> Green -> Refactor
│   ├── systematic-debugging/          # Prima la causa, poi il fix
│   ├── verification-before-completion/# Dimostra che funziona prima di dirlo
│   ├── requesting-code-review/        # Quando e come chiedere una review
│   ├── receiving-code-review/         # Come gestire il feedback della review
│   ├── code-review-checklist/         # Checklist sicurezza, performance, anti-pattern
│   ├── finishing-a-development-branch/# Merge, PR, o scarta
│   └── using-git-worktrees/           # Workspace isolati
└── agents/                            # 4 agenti specialisti per la review
    ├── code-reviewer.md               # Orchestratore (lancia i 3 sotto)
    ├── security-reviewer.md           # OWASP, injection, auth, secrets
    ├── performance-reviewer.md        # Query N+1, algoritmi, caching
    └── architecture-reviewer.md       # SOLID, naming, conformita' CLAUDE.md
```

---

## Installazione Veloce (2 minuti)

### Passo 1: Clona il repo

```bash
git clone https://github.com/brunotr88/devpilot-by-truant.git
cd devpilot-by-truant
```

### Passo 2: Installa skill e agenti globalmente

Questo rende tutto disponibile in **ogni** progetto Claude Code:

```bash
# Copia le skill nella cartella globale di Claude
cp -r skills/* ~/.claude/skills/

# Copia gli agenti nella cartella globale di Claude
cp agents/* ~/.claude/agents/
```

### Passo 3: Aggiungi le regole del workflow

```bash
# Se NON hai ancora un CLAUDE.md globale:
cp CLAUDE.md ~/.claude/CLAUDE.md

# Se ne hai gia' uno, aggiungi in coda:
echo "" >> ~/.claude/CLAUDE.md
cat CLAUDE.md >> ~/.claude/CLAUDE.md
```

### Passo 4: Riavvia Claude Code

Chiudi e riapri Claude Code (o inizia una nuova conversazione). Fatto!

### Verifica l'installazione

Apri Claude Code su un qualsiasi progetto e scrivi:

```
Che skill hai disponibili?
```

Dovresti vedere tutte le 12 skill DevPilot elencate.

---

## Come funziona

### La Pipeline di Sviluppo

```
Tu: "Voglio costruire X"
         |
         v
    +--------------+
    | BRAINSTORMING |  <-- Esplora la tua idea, fa domande (1 alla volta),
    |               |      propone 2-3 approcci, scrive una spec di design
    +------+-------+
           |
           v
    +--------------+
    | WRITING-PLANS |  <-- Crea task step-by-step (2-5 min ciascuna),
    |               |      con codice esatto, test esatti, comandi esatti
    +------+-------+
           |
           v
    +--------------+
    |  IMPLEMENTA   |  <-- Esegue ogni task con TDD:
    |               |      scrivi test -> fallisce -> codice -> passa -> commit
    +------+-------+
           |
           v
    +--------------+
    | CODE REVIEW   |  <-- 3 specialisti in parallelo controllano il codice:
    |               |      sicurezza + performance + architettura
    +------+-------+
           |
           v
    +--------------+
    |   FINE        |  <-- Merge, crea PR, tieni, o scarta
    +--------------+
```

### Attivazione Automatica delle Skill

Non devi ricordare quale skill usare. Le regole nel `CLAUDE.md` dicono a Claude **quando** attivare ogni skill automaticamente:

| Tu dici... | Claude attiva... | Perche' |
|-----------|-------------------|---------|
| "Voglio creare una nuova feature" | `brainstorming` | Feature nuova = prima il design |
| "Crea il piano implementativo" | `writing-plans` | Design pronto, serve il piano |
| "Esegui il piano" | `subagent-driven-development` | Piano pronto, inizia a costruire |
| "Scrivi la funzione login" | `test-driven-development` | Scrivere codice = test prima |
| "I test falliscono" | `systematic-debugging` | Bug = investigare la causa |
| "E' fatto!" | `verification-before-completion` | Le affermazioni servono prove |
| "Rivedi il codice" | `code-reviewer` agent | Lancia 3 revisori specialisti |
| "Pronto per il merge" | `finishing-a-development-branch` | Workflow di completamento |
| "Fixami questi 3 bug" | `dispatching-parallel-agents` | Problemi indipendenti = parallelo |

### Quando le skill NON si attivano

| Tu dici... | Cosa succede | Perche' |
|-----------|-------------|---------|
| "Crea un video reel" | Usa la skill video (se installata) | Lavoro creativo = escluso |
| "Genera un'immagine" | Usa la skill immagini (se installata) | Lavoro creativo = escluso |
| "Cambia un valore nel config" | Nessuna skill speciale | Config non serve TDD |

---

## Le 3 Leggi Ferree

Regole non negoziabili che prevengono gli errori piu' comuni dell'AI nel coding:

### 1. Test-Driven Development

```
MALE: Claude scrive codice -> poi scrive test -> test passano subito (non dimostrano nulla)
BENE: Claude scrive test -> li guarda FALLIRE -> scrive codice minimo -> li guarda PASSARE
```

Perche'? I test scritti dopo il codice passano sempre. Passare subito non dimostra nulla. I test scritti prima dimostrano che il test cattura davvero il bug.

### 2. Debugging Sistematico

```
MALE: "Test falliti" -> Claude: "Provo a cambiare X" -> no -> "Provo Y" -> no -> "Provo Z"
BENE: "Test falliti" -> Claude legge errori -> riproduce -> traccia la causa -> POI fixxa
```

Perche'? Fix random sprecano tempo e creano nuovi bug. Trovare la causa prima = risolvere una volta sola.

### 3. Verifica Prima di Dire "Fatto"

```
MALE: Claude: "Ho fixato il bug, dovrebbe funzionare."
BENE: Claude: "Ho fixato il bug. Lancio i test... 47/47 passano. Bug risolto."
```

Perche'? "Dovrebbe funzionare" non e' una prova. Lancia il comando, leggi l'output, POI afferma.

---

## Sistema di Code Review

Quando Claude finisce una feature, lancia automaticamente **3 revisori specialisti in parallelo**:

```
                    +--- security-reviewer ---- OWASP, injection, auth, secrets
                    |
code-reviewer ------+--- performance-reviewer -- Query N+1, algoritmi, caching
(orchestratore)     |
                    +--- architecture-reviewer - SOLID, naming, pattern, CLAUDE.md
```

Ogni revisore assegna un punteggio di confidenza (0-100). Solo le issue con **confidenza >= 80** finiscono nel report finale. Questo elimina rumore e falsi positivi.

La review e' **proporzionale alle dimensioni**:
- Meno di 50 righe cambiate: review veloce inline (senza sub-agenti)
- 50-300 righe: review standard con tutti e 3 gli specialisti
- Piu' di 300 righe: review approfondita + controllo scope creep

---

## Esempi Pratici

### Esempio 1: Costruire una feature da zero

```
Tu: "Voglio aggiungere l'autenticazione JWT alla mia app Express"

Claude (brainstorming):
  "Capisco cosa ti serve. Che tipo di auth vuoi?
   a) JWT stateless tokens
   b) Session-based con cookie
   c) OAuth2 con provider (Google, GitHub)"

Tu: "a) JWT"

Claude (brainstorming):
  "Ecco 2 approcci:
   1. passport.js + jsonwebtoken (collaudato, tanti middleware)
   2. Middleware custom + libreria jose (piu' leggero, piu' controllo)
   Consiglio l'opzione 1 perche'..."

[... altre domande, poi scrive la spec, poi transizione a...]

Claude (writing-plans):
  "Creo il piano implementativo con 6 task:
   Task 1: Auth middleware (test -> implementa -> commit)
   Task 2: Login endpoint (test -> implementa -> commit)
   ..."

Claude (subagent-driven-development):
  [Lancia sub-agente per Task 1]
  [Sub-agente scrive test che fallisce, implementa, test passano, commit]
  [Code review: security-reviewer controlla gestione JWT]
  [Task 1 completata, passa alla Task 2...]

Claude (finishing-a-development-branch):
  "Tutte le task completate, 24/24 test passano. Cosa vuoi fare?
   1. Merge su main
   2. Crea PR
   3. Tieni il branch
   4. Scarta tutto"
```

### Esempio 2: Debug di un bug in produzione

```
Tu: "L'endpoint dei pagamenti restituisce errori 500 a intermittenza"

Claude (systematic-debugging):
  Fase 1 - Investigazione Causa:
  - Leggo i log degli errori... trovato: "connection timeout su payment_db"
  - Controllo cambiamenti recenti... git diff mostra nuova query aggiunta 2 giorni fa
  - Riproduco... succede quando > 100 richieste concorrenti

  Fase 2 - Analisi Pattern:
  - Endpoint /api/orders funzionante gestisce lo stesso carico
  - Differenza: /api/orders usa connection pooling, /api/payments no

  Fase 3 - Ipotesi:
  "Causa: l'endpoint pagamenti apre una nuova connessione DB per richiesta
   invece di usare il pool. Sotto carico, le connessioni si esauriscono."

  Fase 4 - Fix:
  [Scrive test che simula 100 richieste concorrenti]
  [Implementa connection pooling]
  [Test passa]
  [Verifica: "Lancio load test... 500 richieste, 0 errori. Risolto."]
```

---

## Domande Frequenti

### Devo installare tutte e 12 le skill?

Puoi installare solo quelle che vuoi. Ma funzionano meglio insieme — la pipeline le collega automaticamente.

### Funziona con altri tool AI?

Le skill sono progettate per Claude Code, ma il formato `.md` funziona con qualsiasi tool che supporta skill/prompt (Codex, Gemini CLI, Cursor, ecc.). Gli agenti sono specifici per Claude Code.

### Rallenta Claude?

No. Le skill caricano le istruzioni complete solo quando si attivano. A riposo, Claude legge solo le brevi descrizioni. Gli agenti partono solo quando vengono lanciati.

### Posso personalizzare le regole?

Si! Modifica `~/.claude/CLAUDE.md` per cambiare quando le skill si attivano. Modifica qualsiasi `SKILL.md` per cambiare il comportamento. Modifica gli agenti in `~/.claude/agents/`.

### E se ho gia' un CLAUDE.md?

Aggiungi le regole DevPilot in coda al tuo file. Non creano conflitti — aggiungono solo trigger per il workflow.

---

## Disinstallazione

```bash
# Rimuovi le skill
for skill in brainstorming writing-plans test-driven-development systematic-debugging \
  verification-before-completion subagent-driven-development dispatching-parallel-agents \
  finishing-a-development-branch using-git-worktrees requesting-code-review \
  receiving-code-review code-review-checklist; do
  rm -rf ~/.claude/skills/$skill
done

# Rimuovi gli agenti
for agent in code-reviewer security-reviewer performance-reviewer architecture-reviewer; do
  rm -f ~/.claude/agents/$agent.md
done

# Rimuovi le regole workflow da ~/.claude/CLAUDE.md (modifica manualmente)
```

---

---

# English

## What is this?

When you use Claude Code, it's powerful but generic. **DevPilot** teaches Claude Code a structured development methodology with **3 Iron Laws**:

1. **No code without a failing test first** (TDD)
2. **No fix without root cause investigation first** (Debugging)
3. **No "it's done" without running verification first** (Verification)

Plus a full pipeline: brainstorm -> plan -> implement -> review -> merge.

## Quick Install (2 minutes)

```bash
git clone https://github.com/brunotr88/devpilot-by-truant.git
cd devpilot-by-truant

# Install skills and agents globally
cp -r skills/* ~/.claude/skills/
cp agents/* ~/.claude/agents/

# Add workflow rules
# If you don't have a CLAUDE.md yet:
cp CLAUDE.md ~/.claude/CLAUDE.md
# If you already have one:
# echo "" >> ~/.claude/CLAUDE.md && cat CLAUDE.md >> ~/.claude/CLAUDE.md

# Restart Claude Code
```

## How it works

### Automatic Skill Activation

| You say... | Claude activates... | Why |
|-----------|-------------------|-----|
| "Let's build a new feature" | `brainstorming` | New feature needs design first |
| "Create the implementation plan" | `writing-plans` | Design ready, needs planning |
| "Execute the plan" | `subagent-driven-development` | Plan ready, start building |
| "Write a login function" | `test-driven-development` | Writing code = test first |
| "Tests are failing" | `systematic-debugging` | Bug = investigate root cause |
| "It's done!" | `verification-before-completion` | Claims need proof |
| "Review my code" | `code-reviewer` agent | 3 specialist reviewers |
| "Ready to merge" | `finishing-a-development-branch` | Branch completion |
| "Fix these 3 bugs" | `dispatching-parallel-agents` | Independent problems = parallel |

### When skills do NOT activate

| You say... | Why |
|-----------|-----|
| "Create a video reel" | Creative work is excluded |
| "Generate an image" | Creative work is excluded |
| "Change a config value" | Config changes don't need TDD |

### The 3 Iron Laws

1. **TDD**: No production code without a failing test first
2. **Debugging**: No fixes without root cause investigation first
3. **Verification**: No completion claims without fresh verification evidence

### Code Review System

After each feature, Claude dispatches 3 parallel specialist reviewers (security + performance + architecture). Only findings with confidence >= 80 make the final report.

For full details on the pipeline, examples, and FAQ, see the [Italian section above](#cos-e) — the content is identical.

---

## Credits

- Inspired by [obra/superpowers](https://github.com/obra/superpowers) — the original agentic skills framework
- Top skills from [skills.sh](https://skills.sh) analyzed and integrated
- Built with Claude Code + Claude Opus 4.6

## License

MIT License — use it, fork it, improve it.
