# FieldOps ERP

🇬🇧 [English](#english) · 🇮🇹 [Italiano](#italiano)

---

<a name="english"></a>
## English

Portfolio project: an ERP for a fictional plant-installation company (photovoltaic, plumbing, HVAC). Built to demonstrate backend architecture, data integrity under concurrency, and an AI-assisted development workflow — see [`CLAUDE.md`](CLAUDE.md) for the working rules that drive it.

### Modules

| Module | Scope | Status |
|---|---|---|
| CRM | Leads, deals, customers, contacts | in progress |
| Administration | Quotes, invoices, credit notes with fiscal numbering | planned |
| Warehouse | Products, delivery notes (DDT), stock movements ledger | planned |
| Jobs | Jobs, construction sites, work items | phase 2 |
| Access control | Roles (superadmin, office, worker) + policies | planned |

### Stack

- **Backend:** PHP 8.4, Laravel, MySQL 8 (InnoDB)
- **Frontend:** Vue 3 + TypeScript, Inertia.js, Tailwind CSS, Vite
- **Auth:** Laravel Fortify (headless) with Inertia pages
- **Tooling:** Docker, Pest, Larastan, Laravel Boost

### Key architectural decisions

- **Deals always belong to a customer.** A lead is a standalone entity; converting it creates (or links) a customer, so a customer can have many deals and no polymorphic relations are needed. `deals.lead_id` is kept only for funnel tracing.
- **Separate document tables** (`quotes`, `invoices`, `credit_notes`) with dedicated line tables sharing an identical structure. Real foreign keys everywhere — no `morphs` — for full referential integrity. The quote → invoice → credit note chain is modeled with explicit FKs.
- **Concurrency-safe fiscal numbering.** Per-type, per-year counters in `document_counters`, incremented with `SELECT ... FOR UPDATE` inside the issuing transaction. No gaps, no duplicates under parallel requests.
- **Documents are snapshots.** Customer data, product description and unit price are copied onto the document at issue time. An invoice never changes because a price list or an address changed.
- **Money as integer cents.** No floats anywhere. Line totals are MySQL generated columns; document totals are recomputed and verified server-side.
- **Stock is a ledger.** No mutable `stock` column: quantity on hand is the sum of `stock_movements` (append-only, signed quantities, each movement referencing the document or job that caused it).
- **Fiscal documents are never deleted.** Issued documents move through an explicit state machine (draft → issued → paid / cancelled); cancellation is a state, not a `DELETE`.

The full schema lives in [`database/schema.dbml`](database/schema.dbml) — paste it into [dbdiagram.io](https://dbdiagram.io) to view the diagram.

### Getting started

#### Local development (recommended)

PHP runs locally, MySQL runs in Docker:

```bash
git clone https://github.com/<user>/fieldops-erp.git && cd fieldops-erp
cp .env.example .env
docker compose up -d          # starts MySQL only
composer install
php artisan key:generate
php artisan migrate --seed
npm install
composer run dev              # serves PHP + Vite together
```

#### Full container build

The `Dockerfile` produces a self-contained image (PHP-FPM + Nginx + built assets) so the project runs anywhere:

```bash
docker build -t fieldops-erp .
docker run -p 8080:8080 --env-file .env fieldops-erp
```

### Testing

```bash
php artisan test
```

Concurrency-critical paths (fiscal numbering, stock movements, state transitions) are covered by dedicated tests simulating parallel transactions. Static analysis runs with Larastan.

### License

MIT — demonstration project; the company and all data are fictional.

---

<a name="italiano"></a>
## Italiano

Progetto portfolio: un gestionale per un'azienda fittizia di installazione impianti (fotovoltaico, idraulico, HVAC). Nato per dimostrare architettura backend, integrità dei dati in concorrenza e un workflow di sviluppo assistito da AI — vedi [`CLAUDE.md`](CLAUDE.md) per le regole di lavoro che lo guidano.

### Moduli

| Modulo | Contenuto | Stato |
|---|---|---|
| CRM | Lead, trattative, clienti, referenti | in corso |
| Amministrazione | Preventivi, fatture, note di credito con numerazione fiscale | pianificato |
| Magazzino | Prodotti, DDT, registro movimenti di stock | pianificato |
| Commesse | Commesse, cantieri, voci di lavoro | fase 2 |
| Accessi | Ruoli (superadmin, ufficio, operaio) + policy | pianificato |

### Stack

- **Backend:** PHP 8.4, Laravel, MySQL 8 (InnoDB)
- **Frontend:** Vue 3 + TypeScript, Inertia.js, Tailwind CSS, Vite
- **Auth:** Laravel Fortify (headless) con pagine Inertia
- **Tooling:** Docker, Pest, Larastan, Laravel Boost

### Decisioni architetturali chiave

- **Ogni trattativa appartiene a un cliente.** Il lead è un'entità a sé; la conversione crea (o collega) un cliente, così un cliente può avere più trattative senza relazioni polimorfiche. `deals.lead_id` resta solo per tracciare il funnel.
- **Tabelle documento separate** (`quotes`, `invoices`, `credit_notes`) con tabelle righe dedicate a struttura identica. Foreign key reali ovunque — niente `morphs` — per integrità referenziale piena. La catena preventivo → fattura → nota di credito è modellata con FK esplicite.
- **Numerazione fiscale sicura in concorrenza.** Contatori per tipo e anno in `document_counters`, incrementati con `SELECT ... FOR UPDATE` nella stessa transazione di emissione. Nessun buco, nessun duplicato sotto richieste parallele.
- **I documenti sono snapshot.** Dati cliente, descrizione prodotto e prezzo unitario vengono copiati sul documento al momento dell'emissione. Una fattura non cambia se cambia il listino o un indirizzo.
- **Importi in centesimi interi.** Niente float. I totali riga sono generated column MySQL; i totali documento vengono ricalcolati e verificati lato server.
- **Il magazzino è un registro.** Nessuna colonna `stock` mutabile: la giacenza è la somma dei `stock_movements` (append-only, quantità con segno, ogni movimento riferito al documento o alla commessa che lo ha generato).
- **I documenti fiscali non si cancellano.** I documenti emessi seguono una state machine esplicita (bozza → emesso → pagato / annullato); l'annullamento è uno stato, non una `DELETE`.

Lo schema completo è in [`database/schema.dbml`](database/schema.dbml) — incollalo su [dbdiagram.io](https://dbdiagram.io) per vedere il diagramma.

### Avvio

#### Sviluppo locale (consigliato)

PHP gira in locale, MySQL in Docker:

```bash
git clone https://github.com/<user>/fieldops-erp.git && cd fieldops-erp
cp .env.example .env
docker compose up -d          # avvia solo MySQL
composer install
php artisan key:generate
php artisan migrate --seed
npm install
composer run dev              # serve PHP + Vite insieme
```

#### Build container completa

Il `Dockerfile` produce un'immagine autosufficiente (PHP-FPM + Nginx + asset compilati), lanciabile ovunque:

```bash
docker build -t fieldops-erp .
docker run -p 8080:8080 --env-file .env fieldops-erp
```

### Test

```bash
php artisan test
```

I percorsi critici per la concorrenza (numerazione fiscale, movimenti di magazzino, transizioni di stato) sono coperti da test dedicati che simulano transazioni parallele. L'analisi statica gira con Larastan.

### Licenza

MIT — progetto dimostrativo; l'azienda e tutti i dati sono fittizi.
