# 📘 Task Manager collaborativo full stack — README di avvio progetto

Questo repository è pensato come base di partenza per una attività didattica in cui gli studenti progettano e sviluppano, in modo incrementale, una applicazione web **stile Trello** con:

- **frontend in JavaScript vanilla**
- **backend in Python con FastAPI**
- **database iniziale SQLite**
- **migrazione successiva a DuckDB**
- supporto a **multiutente**, **multiprogetto** e **task condivise**

L’idea non è partire subito scrivendo codice “a caso”, ma abituarsi a lavorare come in un progetto reale: prima si chiariscono i requisiti, poi si progetta, si approva la soluzione, infine si sviluppa.

## Obiettivo dell’applicazione

L’applicazione deve permettere a più utenti di collaborare nella gestione delle attività di progetto.

Ogni utente deve poter:

- vedere i **propri task personali**
- vedere i **task del team**
- lavorare su **più progetti**
- assegnare task a uno o più utenti del team
- classificare i task con **label** preimpostate o personalizzabili
- definire **milestone**
- indicare **stima di inizio**, **durata stimata** e stato di avanzamento
- filtrare i task per progetto, utente, label, milestone, stato e tempi

L’interfaccia deve includere almeno:

- una **pagina personale** con i task dell’utente
- una **pagina di team/progetto**
- una **Kanban board**
- una vista **Gantt interattiva**, con possibilità di modificare date o durata tramite mouse

## Approccio didattico suggerito

Il lavoro viene diviso in **4 prompt progressivi**. Ogni prompt deve portare a un deliverable chiaro, approvabile e migliorabile. L’obiettivo è allenare gli studenti a:

- leggere e interpretare requisiti
- fare progettazione prima del coding
- documentare le decisioni tecniche
- separare responsabilità tra database, backend e frontend
- ragionare su estendibilità e manutenzione del software

## Architettura generale attesa

Una possibile struttura del progetto può essere questa:

```text
project-root/
├─ README.md
├─ docs/
│  ├─ db-schema.md
│  ├─ backend-design.md
│  ├─ frontend-design.md
│  ├─ db-migration.md
│  └─ mockup/
│     └─ mockup.svg
├─ backend/
│  ├─ app/
│  │  ├─ main.py
│  │  ├─ api/
│  │  ├─ services/
│  │  ├─ repositories/
│  │  ├─ models/
│  │  ├─ schemas/
│  │  └─ db/
│  └─ requirements.txt
├─ frontend/
│  ├─ index.html
│  ├─ css/
│  ├─ js/
│  ├─ assets/
│  ├─ manifest.json
│  └─ service-worker.js
└─ tests/
```

## Nota importante sulla separazione del database

Nel punto 4 chiedi il cambio da SQLite a DuckDB con un pattern dedicato alla separazione del layer dati.

Il nome che probabilmente stavi cercando è **Repository Pattern**.

In pratica:

- il resto dell’applicazione **non deve conoscere** i dettagli del database concreto
- il codice applicativo dialoga con **interfacce astratte**
- si possono avere implementazioni diverse, ad esempio:
  - `SQLiteTaskRepository`
  - `DuckDBTaskRepository`

Se vuoi impostarlo ancora meglio, puoi combinarlo con:

- **Dependency Injection** per scegliere l’implementazione concreta
- **Ports and Adapters (Hexagonal Architecture)** se vuoi dare una struttura più evoluta

Per una attività scolastica, il compromesso migliore è:

- **Repository Pattern + Dependency Injection**

---

# I 4 prompt per la costruzione incrementale

Di seguito trovi quattro prompt già pronti. Sono scritti in modo da guidare un agente AI o un assistente di coding a produrre prima la progettazione e solo dopo il codice.

---

## Prompt 1 — Progettazione database, schema Mermaid, approvazione e sviluppo

```text
Agisci come un software architect e database designer.

Devo progettare il database di una applicazione task manager collaborativa stile Trello, con frontend JavaScript vanilla e backend Python/FastAPI.
Il database iniziale deve essere SQLite.

Requisiti funzionali dell’app:
- multiutente
- multiprogetto
- task personali e task condivise nel team
- pagina personale con i task del singolo utente
- pagina team/progetto con i task condivisi
- label per classificare i task
- label preimpostate ma anche modificabili dall’utente nel nome e nel colore
- milestone associate ai progetti
- tempi stimati: data/ora di inizio prevista e durata stimata
- stato del task compatibile con una board Kanban
- assegnazione dei task agli utenti
- possibilità di filtrare task per utente, team, progetto, label, milestone, stato e tempi
- preparazione futura a una vista Gantt interattiva

Voglio che tu esegua il lavoro in 4 fasi rigorose.

FASE 1 — Analisi
1. Elenca le entità principali.
2. Elenca attributi principali per ogni entità.
3. Descrivi le relazioni tra le entità.
4. Evidenzia eventuali punti ambigui o decisioni progettuali da fissare.

FASE 2 — Proposta del modello dati
1. Proponi uno schema relazionale normalizzato.
2. Spiega le chiavi primarie e le chiavi esterne.
3. Indica eventuali vincoli di unicità, nullabilità e cancellazione.
4. Giustifica le scelte pensando a estendibilità e manutenzione.

FASE 3 — Documentazione
1. Genera un file Markdown chiamato `docs/db-schema.md`.
2. Nel file inserisci:
   - descrizione discorsiva dello schema
   - elenco tabelle
   - relazioni principali
   - ipotesi progettuali
   - diagramma Mermaid ER oppure class-like per rappresentare lo schema dati
3. Non passare ancora alla creazione SQL definitiva finché non dichiari esplicitamente che il modello è pronto per approvazione.

FASE 4 — Solo dopo approvazione simulata
Quando hai finito la progettazione, scrivi una sezione finale chiamata `APPROVAZIONE RICHIESTA` con un breve riepilogo.
Subito dopo, produci anche una versione `APPROVATA` e genera:
- script SQL SQLite completo di creazione tabelle
- vincoli
- indici utili per filtri e query frequenti
- eventuali dati iniziali di esempio, come alcuni stati Kanban e label predefinite

Vincoli di output:
- usa nomi coerenti e leggibili in inglese per tabelle e campi
- commenta le scelte più importanti
- non semplificare troppo il modello: deve essere credibile come progetto reale ma adatto a studenti
- mantieni separati chiaramente analisi, progettazione, approvazione e sviluppo
```

---

## Prompt 2 — Progettazione backend FastAPI REST con UML, approvazione e sviluppo

```text
Agisci come un backend architect specializzato in Python e FastAPI.

Parti da un database già progettato per una applicazione task manager collaborativa stile Trello con:
- utenti
- team
- progetti
- task
- assegnazioni
- label personalizzabili
- milestone
- tempi stimati
- viste personali e di team

Voglio progettare il backend come API REST con FastAPI.

Lavora in 4 fasi.

FASE 1 — Analisi architetturale
1. Definisci i moduli principali del backend.
2. Spiega come separare:
   - API routes
   - business logic/services
   - accesso ai dati
   - modelli/schemi di input-output
3. Indica come gestire validazione, errori HTTP e coerenza dei dati.

FASE 2 — Progettazione delle classi e dei componenti
1. Definisci le classi principali.
2. Definisci responsabilità e relazioni tra classi.
3. Crea uno schema UML class diagram in formato Mermaid.
4. Salva il risultato in un file Markdown chiamato `docs/backend-design.md`.

FASE 3 — Progettazione API
1. Elenca gli endpoint REST principali per:
   - autenticazione simulata o gestione utenti
   - progetti
   - task
   - label
   - milestone
   - filtri
   - dashboard personale
   - dashboard team
2. Per ogni endpoint indica:
   - metodo HTTP
   - path
   - scopo
   - payload in ingresso
   - risposta attesa
3. Descrivi anche una possibile struttura JSON per le risposte.

FASE 4 — Solo dopo approvazione simulata
Quando la progettazione è completa, crea una sezione `APPROVAZIONE RICHIESTA`.
Poi produci una sezione `APPROVATA` e genera il codice iniziale del backend con:
- progetto FastAPI organizzato in cartelle
- `main.py`
- router principali
- modelli Pydantic
- service layer
- repository astratti
- implementazione iniziale per SQLite
- esempi di endpoint funzionanti

Vincoli tecnici:
- usa FastAPI
- usa Pydantic per validazione
- prepara il codice a supportare in futuro un cambio di database
- mantieni chiara la separazione tra logica applicativa e persistenza
- commenta il codice in modo didattico
- genera prima il progetto e poi il codice
```

---

## Prompt 3 — Progettazione frontend PWA responsive, mockup SVG, approvazione e sviluppo

```text
Agisci come un frontend architect ed esperto di UX/UI per applicazioni web gestionali.

Devo progettare il frontend di una applicazione task manager collaborativa stile Trello.
Tecnologie richieste:
- HTML
- CSS
- JavaScript vanilla
- nessun framework frontend
- architettura PWA
- design responsive
- interfaccia chiara, semplice e leggibile

Funzionalità da supportare:
- gestione utente
- pagina personale con i propri task
- pagina con i task del team
- filtro task per progetto, utente, label, milestone, stato, tempi
- board Kanban
- vista Gantt interattiva
- interazione con mouse per modificare temporalmente i task nella vista Gantt
- visualizzazione label colorate e personalizzabili
- visualizzazione milestone e tempi stimati

Voglio che il lavoro sia svolto in 4 fasi.

FASE 1 — Analisi UX
1. Identifica le pagine o viste principali.
2. Descrivi i componenti dell’interfaccia.
3. Spiega il flusso di navigazione dell’utente.
4. Indica come rendere l’interfaccia comprensibile anche a chi la usa per la prima volta.

FASE 2 — Progettazione UI
1. Proponi la struttura delle pagine:
   - login o scelta utente simulata
   - dashboard personale
   - dashboard team/progetto
   - Kanban board
   - Gantt
   - gestione label
2. Definisci layout responsive per desktop e mobile.
3. Spiega palette colori, tipografia e principi di accessibilità.

FASE 3 — Mockup
1. Genera mockup in formato SVG.
2. Salva almeno un file `docs/mockup/mockup.svg`.
3. Genera anche un file `docs/frontend-design.md` con:
   - descrizione delle viste
   - componenti riutilizzabili
   - eventi JavaScript principali
   - strategia PWA
   - strategia di caching minima
   - struttura delle chiamate API verso il backend
4. Fermati prima dello sviluppo finale e inserisci una sezione `APPROVAZIONE RICHIESTA`.

FASE 4 — Solo dopo approvazione simulata
Aggiungi una sezione `APPROVATA` e genera il frontend iniziale con:
- `index.html`
- file CSS separati
- file JavaScript separati per moduli logici
- manifest PWA
- service worker base
- viste per dashboard personale, team, Kanban e Gantt
- esempio di drag and drop per Kanban
- esempio di interazione con mouse nella timeline Gantt
- codice leggibile e commentato

Vincoli tecnici:
- niente framework
- niente librerie pesanti se non strettamente necessarie
- preferisci componenti semplici, modulari e facilmente leggibili dagli studenti
- spiega bene come il frontend dialoga con il backend REST
- l’interfaccia deve essere moderna ma essenziale
```

---

## Prompt 4 — Migrazione da SQLite a DuckDB con Repository Pattern

```text
Agisci come un software architect con esperienza in refactoring e portabilità del layer dati.

Ho già una applicazione task manager collaborativa con:
- frontend JavaScript vanilla
- backend FastAPI in Python
- persistenza iniziale in SQLite

Ora voglio migrare il supporto dati a DuckDB senza stravolgere il resto dell’applicazione.
Voglio introdurre una separazione pulita del layer database usando il Repository Pattern, eventualmente supportato da Dependency Injection.

Lavora in 4 fasi.

FASE 1 — Analisi del refactoring
1. Spiega quali parti dell’applicazione non dovrebbero dipendere direttamente dal database concreto.
2. Elenca i problemi tipici di un backend troppo legato a SQLite.
3. Spiega come il Repository Pattern risolve il problema.
4. Se utile, cita anche Ports and Adapters come evoluzione possibile, ma mantieni il progetto didatticamente semplice.

FASE 2 — Progettazione dell’astrazione
1. Definisci le interfacce repository principali.
2. Indica quali metodi devono esporre.
3. Progetta una cartella `repositories/` con:
   - interfacce astratte
   - implementazione SQLite
   - implementazione DuckDB
4. Spiega come selezionare l’implementazione attiva.

FASE 3 — Documentazione del refactoring
1. Genera un file `docs/db-migration.md`.
2. Inserisci:
   - motivazioni del refactoring
   - schema architetturale
   - vantaggi e limiti
   - diagramma Mermaid o UML semplificato delle dipendenze
   - strategia di migrazione dei dati da SQLite a DuckDB
3. Inserisci una sezione `APPROVAZIONE RICHIESTA`.

FASE 4 — Solo dopo approvazione simulata
Aggiungi una sezione `APPROVATA` e genera:
- interfacce repository astratte
- implementazione concreta SQLite
- implementazione concreta DuckDB
- factory o dependency provider per scegliere il database
- esempio di refactoring di uno o due service già esistenti per usare i repository
- script di migrazione dei dati da SQLite a DuckDB
- note su eventuali differenze SQL da gestire

Vincoli tecnici:
- non riscrivere inutilmente tutta l’app
- preserva il più possibile API e servizi già progettati
- mantieni il codice leggibile e didattico
- fai emergere chiaramente la separazione delle responsabilità
```

---

# Suggerimenti operativi per gli studenti

Per ottenere risultati migliori, conviene usare i prompt in sequenza, senza saltare direttamente al codice finale. Ogni passaggio dovrebbe produrre un artefatto concreto da controllare:

1. **Prompt 1** → schema dati e file `docs/db-schema.md`
2. **Prompt 2** → schema backend e file `docs/backend-design.md`
3. **Prompt 3** → mockup e file `docs/frontend-design.md` + SVG
4. **Prompt 4** → refactoring dei repository e file `docs/db-migration.md`

In questo modo gli studenti si abituano a una idea molto importante nello sviluppo software: **prima si progetta, poi si costruisce, poi si migliora senza demolire tutto**.

# Possibile consegna didattica

Puoi presentare l’attività più o meno così:

> Realizzare in gruppo una applicazione web collaborativa per la gestione di task e progetti, ispirata a Trello, con backend Python/FastAPI, frontend JavaScript vanilla e persistenza dati inizialmente in SQLite e successivamente in DuckDB. Il lavoro dovrà essere documentato con schemi, diagrammi, mockup e scelte architetturali motivate, simulando un processo di sviluppo reale.

# Competenze allenate

Questa attività allena competenze molto interessanti perché costringe gli studenti a lavorare su più piani contemporaneamente:

- analisi dei requisiti
- modellazione dei dati
- progettazione API REST
- progettazione di interfacce web responsive
- organizzazione del codice
- separazione tra livelli applicativi
- refactoring architetturale
- ragionamento sull’evoluzione di un sistema software

# Conclusione

Questo README può essere usato come base per una repository da forkare, assegnando a ogni gruppo il compito di seguire i prompt in ordine. Il vantaggio è che il lavoro non parte dal “fammi il codice”, ma da una richiesta più professionale: progettare, farsi approvare la soluzione e poi svilupparla in modo coerente.
